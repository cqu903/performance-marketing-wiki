---
title: Meta ads
created: 2026-06-24
updated: 2026-06-24
type: entity
tags: [meta-ads, srn, integration, event-mapping, privacy]
sources: [raw/articles/207033826-meta-ads-integration.md, raw/articles/4410480904081-meta-ads-in-app-event-mapping.md, raw/articles/4410481130641-meta-ads-discrepancies.md, raw/articles/19228737402129-meta-ios-aem.md]
confidence: high
---

# Meta ads

Meta ads 也是 SRN。AppsFlyer 对 Meta 的价值主要在于统一归因视图、事件回传、AEM/SKAN/iOS 隐私限制下的可用数据治理，以及与 Google 等渠道并排比较。^[raw/articles/207033826-meta-ads-integration.md]

## 必备配置
- 在 AppsFlyer Partner Marketplace 启用 Meta ads 对接。
- 配置激活归因、浏览归因、再互动/再归因（如跑再营销）。
- 映射应用内事件；贷款 App 应优先映射能代表质量的深层漏斗事件，而不只是注册。
- iOS 需要关注 ATT、SKAN 和 AEM 限制；详见 [[ios-privacy-skan-aem]]。

## 推荐窗口
Meta 默认点击回溯窗口 7 天、浏览回溯窗口 24 小时。AppsFlyer 的窗口设置最好与 Meta 默认保持一致，否则会出现渠道侧与 AppsFlyer 侧的归因差异。详见 [[lookback-window-strategy]]。

## AEM 注意点
Meta AEM 对 iOS 事件优化有额外要求。若事件不符合 AEM 条件，常见处理是确保 SDK/事件正常、事件映射发送到“所有媒体源，包括自然媒体源”，并在 Meta 事件管理器检查首选连接方法。变更后通常需要等待 2-3 天。^[raw/articles/19228737402129-meta-ios-aem.md]

拉新广告组可能同时使用 AEM 和 SKAN；如果选择 Apple SKAdNetwork，则只有 SKAN 对该广告系列生效，AppsFlyer 对 Meta 应用使用情况的衡量能力会受限。AEM 高级数据共享可向 Meta 发送更多非设备 ID 信息以改善优化，但可能放大 Meta 与 AppsFlyer 的安装差异，启用前要确认平台政策和本地合规要求。详见 [[ios-privacy-skan-aem]]。^[raw/articles/19228737402129-meta-ios-aem.md]

## X Wallet iOS 操作重点
- 注册完成、申请开始、资料提交这类中浅层事件优先保证 AEM 合格和稳定回传。
- 授信、放款这类深层事件可作为质量信号测试，但不要在量级不足时直接作为唯一优化事件。
- 代理复盘 iOS Meta 时，必须同时说明 AppsFlyer 传统归因、Meta AEM/SKAN 状态和事件管理器状态，不能只交一张 Ads Manager 截图。

## 相关页面
- [[google-vs-meta-integration]]
- [[postback-and-event-mapping]]
- [[data-discrepancy-playbook]]
