---
title: Meta Pixel
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [meta-ads, integration, event-mapping]
sources: [raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]
confidence: high
---

# Meta Pixel

Meta Pixel（前身为 Facebook Pixel）是嵌入网页的前端 JavaScript 埋点，通过浏览器 Cookie 采集用户在 H5 页面上的行为事件并上报 [[meta-ads]]。Pixel 是 W2A 链路中"网页层"的基础转化跟踪组件，与 [[meta-capi|CAPI]] 构成双轨上报。

## 采集方式与定位

- 嵌入 H5 页面，靠浏览器 Cookie 标识用户。
- 采集页面浅层行为：页面浏览、CTA 按钮点击、简单填表。
- 实时投喂 Meta 做人群触发和再营销。

## Pixel vs CAPI

| 维度 | Pixel（前端 JS） | [[meta-capi|CAPI]]（服务器端） |
|---|---|---|
| 采集方式 | 浏览器 Cookie | 业务后端直接调 Meta API |
| 信号损耗 | 高（Safari ITP、第三方 Cookie 拦截、广告拦截、页面跳转中断） | 极低 |
| 数据深度 | 仅页面浅层行为 | 后端深度转化（授信、放款） |
| iOS 隐私影响 | 严重（ATT/ITP 双重限制） | 小 |
| 标识符 | Cookie / fbp | SHA256 加密手机号、fbclid、fbp、设备信息 |
| EMQ | 低 | 高 |

## W2A 链路中的角色

在 [[w2a-data-flow|信贷 W2A 三层数据流]]的第一层（H5 网页层）：
- **Pixel**：采集实时浅层行为（页面浏览、CTA 按钮点击、简单填表）。
- **CAPI**：上报确定性网页转化（网页申请提交），弥补 Pixel 前端丢失。
- **AF Web-S2S**：补全 AF 网页层数据空白。

行业标准方案是 Pixel + CAPI 双轨上报，分工明确：Pixel 做实时浅层信号，CAPI 做确定性深度转化。

## Pixel 的局限性

信贷业务链路极长（网页提交 → 风控审核 → 授信 → 放款 → 还款），其中授信、放款等核心商业转化的数据仅存储在信贷风控后端，前端 H5 和 App 均拿不到。Pixel 无法采集这类后端事件，必须依赖 CAPI。

## 相关页面
- [[meta-capi]] — 服务器端转化上报通道，与 Pixel 构成双轨。
- [[w2a-data-flow]] — Pixel 在信贷 W2A 三层数据流第一层的角色。
- [[meta-ads]] — Pixel 所属的广告平台。
- [[af-vs-meta-capi]] — AF postback 与 CAPI/Pixel 的能力对比。
