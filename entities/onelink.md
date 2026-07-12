---
title: OneLink
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [appsflyer, attribution, integration, retargeting]
sources: [raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]
confidence: high
---

# OneLink

OneLink 是 [[appsflyer]] 的核心归因链接产品（Tracking Link）。它向 AppsFlyer 后台上报"点击触点"，是归因匹配的第一条独立数据流——广告点击时存储的设备指纹 + App 首开后 SDK 上报的设备信息，双向校验完成安装绑定。

## 点击上报时机

归因链接向 AF 上报点击的默认时机 = 用户点击广告素材的瞬间，而不是落地页 H5 内部按钮的二次点击。理清链路上产生 1 条还是 2 条触点，是排查再营销抢占、渠道 CVR 偏低、SKAN 注册失败等问题的前提。详见 [[onelink-click-tracking]]。

## 两种触点场景

| 场景 | 触点数 | 末次归因 |
|---|---|---|
| 广告绑定原生 AF 链接，H5 按钮无独立 OneLink | 1 条（广告点击） | 归该广告 |
| H5 下载按钮单独挂载独立 OneLink | 2 条（广告 + H5 按钮） | H5 按钮时间更晚，抢占归因 |

## 直跳商店 vs H5 落地页

| 维度 | 直跳商店 | H5 落地页 |
|---|---|---|
| 无效点击损耗 | 极低 | 高（大量浏览不下载） |
| 网页行为埋点 | 不支持 | 支持（搭配 [[af-smartscript|SmartScript]]） |
| Web 浏览归因(VTA) | 无法开启 | 可开启 |
| iOS SKAN 稳定性 | 最优 | H5 加载延迟小幅增加注册失败 |

## OneLink 在归因匹配中的角色

广告点击 OneLink → AF 服务器存储点击记录（渠道 pid、campaign、素材、全局唯一 af_clickid、IP、UA、精确点击时间）→ AF 重定向 H5 页面。App 首开后 SDK 上报设备信息，AF 启动点击记录与安装设备的多层匹配。详见 [[install-attribution-matching]]。

关键：App Store / Google Play 本身不参与归因线索存储。所有匹配依据来自 OneLink 点击时存入后台的设备指纹与 SDK 上报的设备信息。

## af_clickid 精准归因

OneLink 点击生成全局唯一 af_clickid。搭配 H5 落地页部署 [[af-smartscript|AF SmartScript]] 缓存 af_clickid → App 打开后 SDK 读取缓存随 install 上报 → AF 精准命中对应点击记录。这是 ATT 拒绝追踪场景下唯一不依赖 IDFA 的精准归因路径。详见 [[install-attribution-matching]] 第二层匹配。

## X Wallet 操作要点

1. iOS 投放强烈建议搭配 H5 落地页 + AF SmartScript，启用 af_clickid 精准匹配。
2. 再营销场景若在 H5 下载按钮挂独立 OneLink，要预见到该按钮会抢占归因——这是 [[attribution-model]] 末次点击规则的预期行为。
3. iOS 投放优先走直跳商店链路可获得最稳定的 SKAN 注册；H5 链路需评估 SKAN 注册失败率与漏斗埋点收益的权衡。

## 相关页面
- [[onelink-click-tracking]] — OneLink 点击上报时机与链路差异的完整分析。
- [[install-attribution-matching]] — OneLink 点击触点存储后的多层归因匹配。
- [[attribution-model]] — OneLink 归因链接对应的末次点击规则。
- [[af-smartscript]] — H5 落地页缓存 af_clickid 的脚本产品。
- [[ios-privacy-skan-aem]] — H5 跳转对 SKAN 注册稳定性的影响。
- [[w2a-data-flow]] — 信贷 W2A 链路中 OneLink 的数据流角色。
