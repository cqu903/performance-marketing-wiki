---
title: IDFA (Identifier for Advertisers)
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [device-id, privacy, attribution, ios]
sources: [raw/articles/4408847686161-device-identifiers.md, raw/articles/360011890298-ios14-att-skan-faq.md, raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]
confidence: high
---

# IDFA (Identifier for Advertisers)

IDFA 是 Apple 为 iOS 设备分配的广告标识符，用于跨 App/网站追踪用户行为。IDFA 是 iOS 确定性归因的基石——广告点击时媒体 App 传入的 IDFA 与 App 首开后 SDK 上报的 IDFA 一致即完成精准匹配。iOS 14.5 后 IDFA 受 [[att]] 框架约束。

## ATT 对 IDFA 的影响

- iOS 14.5+ 默认未授权状态下 IDFA 不可用（返回全零字符串）。
- [[att]] 弹窗仅在 App 首开时弹出，用户授权后 SDK 才能获取有效 IDFA。
- 拒绝追踪后传入的 IDFA 为全零无效字符串，传统 ID 精准匹配失效。

## 点击阶段 vs 激活阶段的 IDFA 可用性

IDFA 的可用性不仅受 ATT 影响，还取决于链路阶段：

- **点击广告阶段**：Safari/H5/浏览器 JS 完全没有系统权限读取本机 IDFA。仅媒体 App（抖音/Meta 等）可在跳转 [[onelink]] 时被动传入 IDFA 参数。
- **App 首开激活阶段**：[[appsflyer]] SDK 是全链路唯一稳定、可靠获取 IDFA 的渠道。

| 环境 | 能否获取 IDFA | 说明 |
|---|---|---|
| 媒体 App 内点击（用户已 ATT 授权） | 能（媒体 App 传入） | 跳转 OneLink 时自动追加 &idfa=xxxx |
| 媒体 App 内点击（用户拒绝追踪） | 传入全零无效值 | AF 点击记录仅留存弱指纹 |
| 纯浏览器/短信外链 | 不能 | URL 不携带 idfa 字段 |
| App 首开 SDK 上报 | 能（受 ATT 控制） | 全链路唯一稳定获取渠道 |
| H5 网页 JS | 不能 | 无系统权限 |

## 无 IDFA 场景的归因替代

IDFA 失效后，AF 启用四层优先级并行归因：
1. [[skadnetwork|SKAN]]（Apple 隐私归因，最高优先级）
2. af_clickid 精准匹配（依赖 [[af-smartscript|H5 SmartScript]] 缓存）
3. IDFV 弱关联（几乎不适用外部买量）
4. 概率指纹建模兜底

详见 [[install-attribution-matching]]。

## 对广告平台的影响

| 平台 | IDFA 缺失的影响 |
|---|---|
| [[google-ads]] | iOS 搜索库存不归因；引入 gbraid 标识符替代；结合建模转化 |
| [[meta-ads]] | 依赖 [[aem|AEM]] 聚合衡量；事件合格性要求更严格 |
| AppsFlyer | 用户级 Raw Data 不完整；概率模型/平台建模占比上升 |

## 对 X Wallet 的操作要点

1. iOS 明确 ATT 弹窗策略；授权率要单独监控。
2. 确认 AppsFlyer SDK 版本正确采集 IDFA（受 ATT 控制后的值）。
3. iOS 投放搭配 H5 落地页 + AF SmartScript，用 af_clickid 补充 IDFA 缺失。
4. 不要把 iOS 归因问题简单归咎于代理投放——IDFA 受限是系统性约束。

## 相关标识符

| 标识符 | 平台 | ATT 影响 | 用途 |
|---|---|---|---|
| IDFA | iOS | 是 | 跨 App 广告追踪 |
| IDFV | iOS | 否（同供应商内） | 同开发者 App 交叉推广/重装识别 |
| GAID | Android | 无（无 ATT 框架） | Android 确定性归因基础 |
| OAID | Android（中国） | 无 | 非 Google Play 环境 |

## 相关页面
- [[device-identifiers-and-privacy]] — IDFA/IDFV/GAID/OAID 标识符体系。
- [[att]] — 决定 IDFA 可用性的隐私框架。
- [[skadnetwork]] — IDFA 缺失后的 Apple 隐私归因。
- [[install-attribution-matching]] — IDFA 在四层归因匹配中的位置。
- [[ios-privacy-skan-aem]] — IDFA 限制对广告平台的完整影响。
- [[ios-w2a-vs-android-a2a]] — IDFA 限制推导 iOS W2A 策略。
