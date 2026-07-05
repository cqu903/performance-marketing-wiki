---
title: Meta CAPI（Conversions API）
created: 2026-07-05
updated: 2026-07-05
type: concept
tags: [meta-ads, integration, event-mapping, privacy, data-quality]
sources: [raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]
confidence: high
---

# Meta CAPI（Conversions API）

Meta Conversions API（CAPI）是 Meta 官方的服务器端转化上报通道。业务后端直接调用 Meta 开放接口推送转化事件，完全绕过浏览器、移动端设备、前端脚本，不受隐私政策、页面跳转、拦截规则影响，信号稳定、损耗极低。它解决的是 Pixel 前端埋点在隐私收紧后信号丢失加剧的问题，尤其在 W2A 链路和信贷后端深度转化场景中不可替代。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

## Pixel vs CAPI

| 维度 | Pixel（前端 JS 埋点） | CAPI（服务器端上报） |
|---|---|---|
| 采集方式 | 嵌入 H5 页面，靠浏览器 Cookie | 业务后端直接调 Meta API |
| 信号损耗 | 高：Safari ITP、第三方 Cookie 拦截、广告拦截插件、页面跳转中断、网络波动 | 极低：绕过浏览器和设备端 |
| 数据深度 | 仅页面浅层行为（浏览、点击、简单填表） | 可上报后端深度转化（授信、放款） |
| iOS 隐私影响 | 严重：ATT/ITP 双重限制 | 小：不依赖浏览器环境 |
| 标识符能力 | 仅 Cookie/fbp | SHA256 加密手机号、fbclid、fbp、设备信息多维信号 |
| EMQ 匹配分值 | 低 | 高 |

## W2A 中的 Pixel + CAPI 双轨

W2A 链路天生存在网页断层（广告→H5→商店→App），行业标准方案是 Pixel + CAPI 双轨上报，分工明确：

- **Pixel**：采集实时浅层行为（页面浏览、CTA 按钮点击、简单填表），实时投喂 Meta 做人群触发和再营销。
- **CAPI**：上报确定性、高权重深度转化（网页申请提交、授信、放款），弥补前端丢失数据。

信贷业务链路极长（网页提交申请 → 风控审核 → 授信 → 放款 → 还款），其中授信、放款属于核心商业转化，数据仅存储在信贷风控后端，前端 H5 和 App 均拿不到。这类决定投放 ROI 的高价值事件，仅能依靠后端对接 CAPI 回传 Meta。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

## CAPI 独有能力：EMQ 提升

CAPI 支持上传 SHA256 加密用户手机号、fbclid、fbp、设备信息等多维度信号，大幅提升 Meta Event Match Quality（EMQ）匹配分值。这直接解决了 iOS 隐私限制、无 IDFA 导致的匹配失效问题——这是 AppsFlyer 中转 postback 链路无法实现的能力，因为 AF 回传仅能传递设备 ID。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

详见 [[af-vs-meta-capi]] 中对 AF postback 与 CAPI 原生上报的能力对比。

## CAPI 不能替代 AppsFlyer

CAPI 是单渠道优化工具——只服务 Meta 广告投放，无法聚合 Google、ASA、自然量等多渠道数据，没有反作弊能力、统一对账能力。全局归因、渠道治理、财务对账仍依赖 [[appsflyer]]。两套系统必须并行接入。详见 [[af-vs-meta-capi]]。

## 与 Marketing API 的区别

[[meta-marketing-api]] 是广告账号操作层（campaign/ad set/ad/creative 管理、Insights 报表），CAPI 是转化事件上报层。两者数据流方向相反：Marketing API 从 Meta 拉（reporting），CAPI 往 Meta 推（events）。

## X Wallet 落地要点

1. 风控后端对接 CAPI，推送授信/放款等深度转化事件。
2. H5 落地页部署 Pixel + CAPI 双轨：Pixel 采集浅层行为，CAPI 上报网页申请提交。
3. CAPI 上报时携带加密手机号、fbclid/fbp 等多维信号，提升 EMQ。
4. 同一事件同时上报 AF（S2S），不做二选一。详见 [[w2a-data-flow]]。

## 相关页面
- [[af-vs-meta-capi]]
- [[w2a-data-flow]]
- [[meta-ads]]
- [[meta-marketing-api]]
- [[postback-and-event-mapping]]
- [[ios-privacy-skan-aem]]
