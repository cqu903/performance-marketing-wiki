---
title: AppsFlyer 归因模型
created: 2026-06-24
updated: 2026-07-05
type: concept
tags: [attribution, device-id, srn, data-quality]
sources: [raw/articles/207447053-appsflyer-attribution-model.md, raw/articles/4408847686161-device-identifiers.md, raw/articles/208338403-lookback-windows.md, raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]
confidence: high
---

# AppsFlyer 归因模型

AppsFlyer 的归因模型回答一个问题：一次激活、再互动或再归因，应该归给哪个媒体触点。对投放优化来说，核心不是“所有平台数字完全一致”，而是规则清楚、可复现、能指导预算分配。^[raw/articles/207447053-appsflyer-attribution-model.md]

## 核心规则
- 拉新获客：用户下载并首次启动 App 后，AppsFlyer 将首次启动计为激活时间戳。
- 再营销：已安装用户打开 App 记为再互动；卸载后重新安装可记为重装激活/再归因。
- 多个有效互动存在时，AppsFlyer 优先点击，其次展示；优先确定性归因，其次概率模型。^[raw/articles/207447053-appsflyer-attribution-model.md]

## 主要归因方式
1. Referrer：Android 主要方式之一，Google Play 和部分第三方商店支持。
2. 设备 ID 匹配：IDFA、IDFV、GAID、OAID、Android ID 等。
3. 概率模型：当追踪码或广告标识符不可用时的备选，窗口通常更短，最长 24 小时。
4. AAP / SKAN：隐私保护限制下的汇总或 Apple 归因机制。
5. 深度链接：常用于再营销和跨平台跳转。

## 对 X Wallet 的含义
- Android 香港 Google Play 流量：GAID + Install Referrer 是归因准确度基础。
- iOS 流量：IDFA 授权率、IDFV、SKAN/AEM 配置会直接影响 Meta 和 Google 的可见转化。
- 贷款业务的“高质量事件”通常发生在激活之后较深位置，因此 [[postback-and-event-mapping]] 比只看安装更重要。

## 末次点击与触点产生时机
末次点击规则的"触点"产生时机默认是用户点击广告素材的瞬间，而不是 H5 落地页按钮的二次点击。当 H5 下载按钮挂载独立 OneLink 时会产生第二条触点并抢占归因；直跳商店 vs H5 落地页在无效点击损耗、网页埋点、SKAN 稳定性上也有实质差异。详见 [[onelink-click-tracking]]。

## 相关页面
- [[device-identifiers-and-privacy]]
- [[lookback-window-strategy]]
- [[ios-privacy-skan-aem]]
- [[onelink-click-tracking]]
- [[install-attribution-matching]]
