---
title: iOS 隐私、SKAN 与 AEM
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [privacy, skan, meta-ads, google-ads, data-quality]
sources: [raw/articles/360011890298-ios14-att-skan-faq.md, raw/articles/19228737402129-meta-ios-aem.md, raw/articles/115002504686-google-ads-integration.md, raw/articles/207447053-appsflyer-attribution-model.md, raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md]
confidence: high
---

# iOS 隐私、SKAN 与 AEM

iOS 14.5 之后，ATT、SKAN 和广告平台自己的隐私建模会让 iOS 数据更“聚合、延迟、不完整”。这不是 AppsFlyer 单点配置能完全解决的问题，但配置不完整会进一步放大误差。X Wallet 看 iOS 投放时，必须把用户级归因、SKAN 聚合归因、Meta AEM、Google 建模转化分开理解，不能用 Android 或历史 iOS 的口径套过去。

## 核心概念

### ATT：决定 IDFA 是否可用于跨 App/网站跟踪
ATT（AppTrackingTransparency）要求广告主在需要跨 App/网站跟踪并读取 IDFA 时获得用户授权。iOS 14.5+ 默认未授权状态下 IDFA 不可用；只有用户在广告主 App 和媒体 App 侧都授权，跨 App 跟踪链路才更完整。拒绝 ATT 后，非自然量可能下降、自然量可能上升，再营销和再归因能力也会变弱。^[raw/articles/360011890298-ios14-att-skan-faq.md]

注意：是否展示 ATT 弹窗是产品/合规决策，不是纯技术开关。如果不展示弹窗，就不应期待拿到 IDFA；如果展示，弹窗时机、解释文案和首屏体验会影响授权率。

### SKAN：Apple 的隐私保护归因，不等于 AppsFlyer 传统归因
SKAN（SKAdNetwork）由 Apple 侧发送隐私保护回传，广告平台再转发给 AppsFlyer，AppsFlyer 解析转化值并汇总到 SKAN 报表。它能提供确定性但聚合的 iOS 付费广告归因，不能替代 AppsFlyer 传统归因里的长期 LTV、用户级明细和跨渠道分析。^[raw/articles/360011890298-ios14-att-skan-faq.md]

SKAN 的几个业务含义：
- 回传延迟是机制设计的一部分，不是平台“慢”。SKAN 4 窗口 1 通常 24-48 小时；窗口 2-3 可延迟 24-144 小时；Google 的 SKAN 原始数据有时需要看之后最多约 13 天的文件才完整。^[raw/articles/360011890298-ios14-att-skan-faq.md]
- SKAN 只衡量安装后较短窗口内的行为，所以对贷款 App 这种长决策链路天然不完整。
- SKAN 转化值（CV）要围绕早期质量信号设计，例如注册完成、申请开始、资料提交，而不是只盯最终放款。
- SKAN 报表和 AppsFlyer 传统报表可能同时记录同一批 iOS 安装，并且一个口径显示自然量、另一个口径显示非自然量；两套口径不能简单相加。^[raw/articles/360011890298-ios14-att-skan-faq.md]

### Meta AEM：Meta 在 iOS 14+ 下补足事件衡量和优化的方案
Meta AEM（Aggregated Event Measurement）支持 iOS 14+ 设备的拉新和再互动事件衡量。拉新场景里，AEM 和 SKAN 可能同时生效；如果广告组选择 Apple SKAdNetwork，则只有 SKAN 生效，AppsFlyer 对 Meta 应用使用情况的衡量能力会受限。^[raw/articles/19228737402129-meta-ios-aem.md]

AEM 对事件回传有明确要求：事件要能带上合规可用的 IP、IDFV 等信息；S2S 事件不能缺 IP 或传无效 IP；IDFA 字段不能传非空且非全零值导致 Meta 误判为 opt-in 事件；AppsFlyer 回传映射通常要设置为“所有媒体源，包括自然媒体源”，而不是“仅该合作伙伴”。变更后 Meta 可能需要 2-3 天才重新判定合格。^[raw/articles/19228737402129-meta-ios-aem.md]

### Google iOS：IDFA 限制、建模转化和 gbraid
Google Ads 对接前提仍要求应用收集 IDFA / GAID，但 iOS 14+ 后 Google 对不同库存的归因能力不同：iOS ACI 搜索库存不归因；iOS ACI 展示库存需要广告主 App 和媒体侧 App 都有设备 ID 授权，且 Google 目前仅对 iOS 点击归因、不对展示归因。Google 会扩展模型转化能力，并通过 gbraid 等标识符在没有设备 ID 时支持部分 iOS 再互动和聚合口径。^[raw/articles/115002504686-google-ads-integration.md]

## 对 AppsFlyer 报表的影响
- 用户级 Raw Data 在 iOS 上会更不完整；ATT 未授权用户通常缺 IDFA，部分归因字段会为空或受限。
- SKAN 数据进 SKAN 面板/API，传统归因数据进常规面板/API；同一业务问题经常需要同时看两套数据。
- iOS 非自然安装可能被低估，自然安装可能被高估；这不一定说明渠道无效。
- 再营销、再归因、延迟深度链接受隐私影响更大；旧的 `onConversionDataSuccess` 延迟深链方法可能在 iOS 14.5+ 下不可用，AppsFlyer 建议使用 UDL，且 UDL 只返回深链相关参数，不返回 media_source/campaign 等归因参数。^[raw/articles/360011890298-ios14-att-skan-faq.md]
- SKAN 的短窗口会低估贷款类长漏斗价值，因此 iOS ROI 需要结合 SKAN 早期信号、AppsFlyer 传统事件、广告平台建模结果和后端实际放款数据。

## 对广告平台优化的影响
| 维度 | Meta ads | Google Ads | X Wallet 解读 |
|---|---|---|---|
| 拉新归因 | ATT/SKAN/AEM 同时影响 | IDFA 限制、SKAN/建模转化、gbraid | iOS 安装量不要只看单一平台数字 |
| 事件优化 | AEM 合格性和事件映射影响大 | 转化导入、事件来源、模型转化影响大 | 先保证中浅层事件稳定，再测试深层事件 |
| 数据延迟 | AEM 变更后常需 2-3 天；SKAN 另有延迟 | 转化导入最多约 6 小时；SKAN 原始数据可能更晚 | iOS 评估窗口至少拉长到 3-7 天，重要复盘看 7-14 天 |
| 归因颗粒度 | 部分聚合、部分用户级受限 | iOS 搜索/展示规则不同，gbraid 偏聚合 | 不要要求代理解释到每条用户级点击 |

## X Wallet 应对措施
1. SDK 与标识符：确认 AppsFlyer SDK 支持当前 SKAN 版本；确认 IDFA 请求、IDFV、SKAN、UDL 配置。老 SDK 可能不支持 SKAN 4，甚至影响 App Store 审核。^[raw/articles/360011890298-ios14-att-skan-faq.md]
2. ATT 策略：如果决定弹 ATT，先设计 pre-prompt 和弹窗时机，避免用户刚打开 App 就被无上下文打断；授权率要单独监控。
3. SKAN 转化值：把前 24-48 小时内可触发且能代表质量的事件放进 CV 设计：注册完成、申请开始、资料提交、初审通过优先；放款成功量低且滞后，不适合作为唯一早期信号。
4. Meta AEM：冷启动和事件验证阶段，不宜过早把 AppsFlyer 事件来源限制为“仅 Meta 来源”；中浅层核心事件更适合先使用所有媒体源，待事件稳定和 Meta 自身量级足够后再评估收紧。详见 [[event-source-scope-and-learning]]。^[raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md]
5. Google iOS：App Campaign 与 Web-to-App/再互动分开看；iOS 搜索、展示、YouTube 的归因可用性不同，不能把“看不到完整确定性归因”直接判定为素材或投放失败。
6. 报表节奏：iOS 不看单日实时结论。日常优化看 3-7 天滚动，SKAN/深层事件复盘看 7-14 天，并在报表中标注“传统归因 / SKAN / 平台建模”三种口径。
7. 代理管理：要求代理交付配置截图和变更记录，而不是只解释结果波动。关键检查项包括 ATT/SKAN 状态、Meta AEM 合格性、Google Link ID/转化导入、事件来源范围、回溯窗口。

## 常见误判
- “AppsFlyer iOS 比平台少，所以 AppsFlyer 不准”：可能是 ATT/SKAN/AEM/平台自归因口径差异，不是单点错误。
- “SKAN 有归因，所以不需要传统归因”：错。SKAN 短窗口、聚合、延迟，不能替代长期 LTV 和后端质量分析。
- “把所有深层事件都传给平台就能提升优化”：量级不足或事件滞后会让模型学不到；贷款业务要先保证中浅层质量事件稳定。
- “iOS 当天数据不好马上停广告”：过早。隐私延迟和建模回填会造成短期误判。

## 相关页面
- [[device-identifiers-and-privacy]]
- [[postback-and-event-mapping]]
- [[event-source-scope-and-learning]]
- [[data-discrepancy-playbook]]
- [[google-vs-meta-integration]]
