---
title: 安装归因匹配：商店无参数、IDFA 可用性与无 IDFA 四层匹配
created: 2026-07-05
updated: 2026-07-05
type: concept
tags: [attribution, device-id, skan, privacy, data-quality]
sources: [raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]
confidence: high
---

# 安装归因匹配：商店无参数、IDFA 可用性与无 IDFA 四层匹配

归因的全部设备与渠道证据，在广告点击阶段就永久存入 AppsFlyer 后台；App Store / Google Play 本身不参与归因线索存储。安装匹配依靠两条独立数据流完成绑定：点击时存储的设备指纹 + App 首开后 SDK 上报的设备信息。^[raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]

## 商店无参数时 AF 如何捕获 install

完整流程（广告 → H5 → 商店 → 安装激活）：

### 步骤 1：点击广告，AF 永久存储点击触点
1. 用户点击广告 OneLink → 浏览器请求 AF 服务器；
2. AF 写入 click 记录：渠道 pid、campaign、素材、全局唯一 `af_clickid`、IP、UA、精确点击时间；
3. AF 重定向 H5 页面，归因参数拼接在 H5 URL 上。

### 步骤 2：H5 点击下载跳转商店（商店无 AF 参数）
- 无 AF Smart Script：按钮纯普通商店链接，跳转后商店 URL 干净；
- 部署 AF Smart Script：脚本读取 URL 内 `af_clickid` 存入 localStorage；可选动态生成新 OneLink 跳转商店，新增第二条点击触点（参见 [[onelink-click-tracking]]）。

两种场景下，App Store / Google Play 本身均不携带归因参数。

### 步骤 3：App 首开，SDK 上报 install
App 内置 AF SDK，用户首次打开自动采集上报，与商店无任何交互：
1. SDK 采集设备标识、IP、机型、系统版本；
2. 封装 install 事件发送至 AF；
3. AF 启动点击记录与安装设备的多层匹配。

### 步骤 4：AF 多层匹配绑定
1. Android 一级精准匹配：Google Install Referrer，读取商店 referrer 内 `af_clickid` 直接精准匹配；
2. iOS 有 IDFA 场景：点击时媒体传递的 IDFA 与 SDK 上报 IDFA 一致即匹配；
3. 兜底概率指纹：IDFA 失效/无 referrer 时，依靠 IP、机型、系统、时间窗口组合匹配；
4. 匹配成功标记渠道归属，无匹配标记为自然量 organic。

关键：商店仅作为 App 下载载体，不参与归因线索存储。所有匹配依据来自"广告点击时存入后台的设备指纹"与"App 打开后 SDK 上报的设备信息"双向校验。

## 点击阶段能否获取 IDFA

核心结论：**Safari、H5 网页、浏览器 JS 完全没有系统权限读取本机 IDFA**。IDFA 仅 iOS 原生 App 可通过系统框架获取；ATT 弹窗仅在 App 首开时弹出，点击广告阶段 App 尚未安装。AF 服务器无法主动抓取 IDFA，只能被动接收广告媒体 App 拼接在 OneLink 内的 IDFA 参数。^[raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]

### 两种点击环境
- **环境 A（媒体 App 内点击）**：抖音/Meta/快手等媒体 App 已获取用户 ATT 授权，跳转 AF OneLink 时自动追加 `&idfa=xxxx`。若用户拒绝追踪，传入的 IDFA 为全零无效字符串。
- **环境 B（纯浏览器）**：短信外链、Safari 直接打开 OneLink，无媒体 App 读取 IDFA，URL 不携带 idfa 字段，AF 点击记录仅留存 IP、UA、时间等弱指纹。

### 阶段区分
- 点击链路：网页无读取 IDFA 能力，仅媒体 App 可被动传入；
- App 首开激活链路：**AF SDK 是全链路唯一稳定、可靠获取 IDFA 的渠道**，绝大多数 iOS 精准归因依靠 SDK 上报的 IDFA 完成。

H5 落地页即便部署 AF SmartScript，也仅能读取 URL 渠道参数、缓存 `af_clickid`，依旧无法获取本机 IDFA。

## iOS 无 IDFA（ATT 拒绝追踪）四层归因匹配

用户拒绝追踪后，AF SDK 读取 IDFA 为全零字符串，传统 ID 精准匹配失效。AF 启用四层优先级并行方案，全程遵守末次点击规则。^[raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]

### 第一层：SKAdNetwork（SKAN，苹果官方隐私归因，最高优先级）
- 媒体 App 点击广告时调用苹果 SKAN 接口，向系统报备渠道、活动签名；
- 用户安装后，苹果延迟 24~64 小时匿名推送转化数据给 AF；
- 回传仅含广告网络 ID、活动 ID、转化值，无用户设备标识；
- 优点：100% 合规、无误匹配；缺点：延迟高、仅活动层级、无法细分素材、晚转化丢失。

### 第二层：af_clickid 精准匹配（落地页必备高精度方案）
- 生效前提：媒体 App 内点击广告 + H5 部署 AF SmartScript 缓存 `af_clickid`；
- 广告点击生成全局唯一 `af_clickid` → 网页脚本存 localStorage → App 打开后 SDK 读取缓存随 install 上报；
- AF 后台直接通过 `af_clickid` 精准命中对应点击记录，不受 IDFA 影响，准确率 100%；
- 纯浏览器打开链接、无 H5 缓存时该链路失效。

### 第三层：IDFV 弱关联（几乎不适用外部买量）
IDFV 为单开发者名下 App 共享标识符，跨媒体、跨开发者渠道无法匹配；仅自有多 App 内部流量可辅助串联，投放买量场景基本无价值。

### 第四层：概率指纹建模兜底
- 核心信号：公网 IP；辅助信号：机型、iOS 版本、时区、语言；
- 约束：激活时间必须在归因窗口内；多条匹配取末次点击；
- 多维信号重合计算匹配置信分，达阈值匹配成功，过低标记自然量；
- 短板：公共 WiFi 同 IP 易误匹配、动态 IP 切换导致归因丢失。

### 带 H5 落地页完整闭环案例（用户拒绝 IDFA）
抖音广告 → AF OneLink 跳转自有 H5 → H5 按钮跳转 App Store → 安装后 ATT 拒绝追踪：
1. 点击阶段：抖音传入全零 IDFA，AF 存储点击记录，H5 脚本缓存 `af_clickid`；
2. 商店跳转：无任何归因参数；
3. App 首开：SDK 上报缓存的 `af_clickid`，无有效 IDFA；
4. AF 匹配：优先等待 SKAN 回传，同步校验 `af_clickid` 精准命中广告点击，完成归因。

## 关键问答
1. **关闭 IDFA 后末次点击规则是否生效？** 完全生效。SKAN、af_clickid 匹配、概率指纹多触点场景均优先选择时间最近的点击。
2. **直跳商店 vs H5 落地页，哪种无 IDFA 丢失更少？** H5 落地页搭配 SmartScript 可缓存 `af_clickid` 启用第二层精准匹配，归因丢失率显著低于直跳商店链路。但 SKAN 注册稳定性略低（参见 [[onelink-click-tracking]]）。
3. **安卓是否存在同类问题？** 安卓无 ATT 框架，无法关闭 GAID；依靠 Google Install Referrer 的 clickid 实现确定性匹配，指纹兜底丢失率远低于 iOS。

## 对 X Wallet 的含义
1. **iOS 投放强烈建议搭配 H5 落地页 + AF SmartScript**：第二层 af_clickid 匹配是 ATT 拒绝追踪场景下唯一不依赖 IDFA 的精准归因路径，对香港 iOS 流量尤其关键。
2. **af_clickid 缓存链路需开发验证**：确认 H5 SmartScript 正确缓存、SDK 正确读取回传，这是 iOS 归因准确度的隐藏依赖，常被忽略。
3. **概率指纹的公共 WiFi 误匹配风险**：香港公共 WiFi 普及，同 IP 用户多，概率指纹兜底误匹配概率可能高于预期；排查"异常归因到某渠道"时要考虑此因素。
4. **Android 优先保证 Install Referrer 链路完整**：GAID + Referrer 是确定性匹配基础，丢失率远低于 iOS，资源配置可更激进。

## 相关页面
- [[onelink-click-tracking]] — OneLink 点击上报时机与直跳/H5 链路差异。
- [[ios-privacy-skan-aem]] — SKAN 回传延迟、AEM 合格性、iOS 隐私建模全景。
- [[device-identifiers-and-privacy]] — IDFA/IDFV/GAID 标识符与隐私限制。
- [[attribution-model]] — 确定性/概率归因、末次点击规则基础。
