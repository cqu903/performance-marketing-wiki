---
title: AppTrackingTransparency (ATT)
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [privacy, attribution, ios]
sources: [raw/articles/360011890298-ios14-att-skan-faq.md, raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md, raw/articles/20260712-ios-w2a-vs-android-a2a-uac-strategy.md]
confidence: high
---

# AppTrackingTransparency (ATT)

ATT 是 Apple 在 iOS 14.5+ 引入的隐私框架，要求广告主在需要跨 App/网站跟踪并读取 IDFA 时获得用户授权。ATT 是 iOS 归因能力退化的根因框架——所有 iOS 隐私限制（[[skadnetwork|SKAN]]、[[aem|AEM]]、Google 建模转化）都因 ATT 引入的 IDFA 限制而起。

## 核心机制

- iOS 14.5+ 默认未授权状态下 IDFA 不可用。
- 只有用户在广告主 App 和媒体 App 侧都授权，跨 App 跟踪链路才更完整。
- 拒绝 ATT 后：非自然量可能下降、自然量可能上升，再营销和再归因能力变弱。

## ATT 弹窗是产品/合规决策

是否展示 ATT 弹窗不是纯技术开关：
- 如果不展示弹窗，就不应期待拿到 IDFA。
- 如果展示，弹窗时机、解释文案和首屏体验会影响授权率。
- ATT 弹窗仅在 App 首开时弹出——用户点击广告阶段 App 尚未安装，弹窗不可能提前出现。

## 点击阶段 vs App 激活阶段的 IDFA 可用性

ATT 授权只发生在 App 内：
- **点击广告阶段**：Safari/H5/浏览器 JS 完全没有系统权限读取本机 IDFA。仅媒体 App（抖音/Meta 等）可在跳转 [[onelink]] 时被动传入 IDFA 参数。若用户拒绝追踪，传入的 IDFA 为全零无效字符串。
- **App 首开激活阶段**：[[appsflyer]] SDK 是全链路唯一稳定、可靠获取 IDFA 的渠道。

详见 [[install-attribution-matching]]。

## 对广告平台的影响

| 平台 | ATT 影响 |
|---|---|
| AppsFlyer | IDFA 缺失后依赖四层归因：SKAN → af_clickid → IDFV → 概率指纹 |
| [[meta-ads]] | ATT + SKAN + AEM 三重限制下 iOS 事件可用性和回传延迟加剧 |
| [[google-ads]] | iOS ACI 搜索库存不归因；仅对 iOS 点击归因，不对展示归因；引入 gbraid 标识符 |
| UAC 投放 | opt-in 率偏低导致设备级用户数据缺失，iOS 原生 A2A 信号单薄——推导出 iOS W2A 优先策略 |

## 对 X Wallet 的操作要点

1. 明确 ATT 弹窗策略：先设计 pre-prompt 和弹窗时机，避免用户刚打开 App 就被无上下文打断。
2. 授权率要单独监控，作为 iOS 投放效果的前置指标。
3. 不要把 iOS 归因问题简单归咎于代理投放——ATT 是系统性约束。
4. iOS 报表按"传统归因 / SKAN / 平台建模 / 后端实际贷款结果"分层复盘。

## 相关页面
- [[ios-privacy-skan-aem]] — ATT 引入后的完整 iOS 隐私影响全景。
- [[skadnetwork]] — Apple 隐私保护归因框架。
- [[aem]] — Meta 在 iOS 14+ 下的事件衡量方案。
- [[idfa]] — 受 ATT 直接影响的设备标识符。
- [[install-attribution-matching]] — ATT 拒绝后的四层归因匹配。
- [[ios-w2a-vs-android-a2a]] — ATT 约束推导 iOS W2A 投放策略。
- [[device-identifiers-and-privacy]] — IDFA/IDFV/GAID 标识符体系。
