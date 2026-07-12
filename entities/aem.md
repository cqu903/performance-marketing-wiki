---
title: Aggregated Event Measurement (AEM)
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [meta-ads, privacy, event-mapping, ios]
sources: [raw/articles/19228737402129-meta-ios-aem.md, raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md]
confidence: high
---

# Aggregated Event Measurement (AEM)

AEM 是 [[meta-ads]] 在 iOS 14+ 下用于应对 [[att]]、[[skadnetwork|SKAN]] 等隐私限制的事件衡量和优化方案。它支持 iOS 14+ 设备的拉新和再互动事件衡量，是 Meta iOS 投放信号链路的核心组件。

## 核心机制

- AEM 在 iOS 14+ 隐私限制下对事件做聚合衡量，而非用户级精确追踪。
- 拉新广告组中 AEM 和 SKAN 可能同时生效；如果广告组选择 Apple SKAdNetwork，则只有 SKAN 生效，[[appsflyer]] 对 Meta 应用使用情况的衡量能力会受限。
- AEM 高级数据共享可向 Meta 发送更多非设备 ID 信息以改善优化，但可能放大 Meta 与 AppsFlyer 的安装差异。

## AEM 合格性要求

事件要满足 AEM 条件才能被 Meta 用于优化：
1. 事件要能带上合规可用的 IP、IDFV 等信息。
2. S2S 事件不能缺 IP 或传无效 IP。
3. IDFA 字段不能传非空且非全零值导致 Meta 误判为 opt-in 事件。
4. AppsFlyer 回传映射通常要设置为"所有媒体源，包括自然媒体源"，而不是"仅该合作伙伴"。
5. 变更后 Meta 可能需要 2-3 天才重新判定合格。

## 与事件来源范围的关联

在 Meta iOS App 广告中，由于 IDFA 可用性下降、事件可能延迟或聚合，Meta 的可用学习信号更少。因此 iOS / AEM 冷启动和事件验证阶段，不宜过早把 [[appsflyer]] 事件来源限制为"仅 Meta 来源"。详见 [[event-source-scope-and-learning]]。

## AEM 不合格时的排查

1. 检查事件发送范围是否设为"所有媒体源，包括自然媒体源"。
2. 检查 Meta 事件管理器连接方式。
3. 确认 S2S 事件 IP/IDFV/IDFA 空值处理符合 Meta 要求。
4. 等待 2-3 天让 Meta 重新判定。

## 对 X Wallet 的操作要点

1. 注册完成、申请开始、资料提交这类中浅层事件优先保证 AEM 合格和稳定回传。
2. 授信、放款这类深层事件可作为质量信号测试，但不要在量级不足时直接作为唯一优化事件。
3. 代理复盘 iOS Meta 时，必须同时说明 AppsFlyer 传统归因、Meta AEM/SKAN 状态和事件管理器状态，不能只交一张 Ads Manager 截图。

## 相关页面
- [[ios-privacy-skan-aem]] — AEM 在 iOS 隐私全景中的位置。
- [[meta-ads]] — Meta ads SRN 对接与 AEM 配置。
- [[event-source-scope-and-learning]] — AEM 场景下事件来源范围的策略。
- [[postback-and-event-mapping]] — AEM 合格性对事件回传的要求。
- [[data-discrepancy-playbook]] — AEM 不合格导致的数据差异排查。
- [[att]] — AEM 存在的根因框架。
