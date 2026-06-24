---
title: Google Ads
created: 2026-06-24
updated: 2026-06-24
type: entity
tags: [google-ads, srn, integration, event-mapping, cost]
sources: [raw/articles/115002504686-google-ads-integration.md, raw/articles/4417303339921-google-ads-faq-discrepancies.md, raw/articles/115004970386-google-ads-mcc-multiple-agencies.md, raw/articles/20260624-google-ads-api-get-started.md, raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md]
confidence: high
---

# Google Ads

Google Ads 是 SRN（Self Reporting Network）。AppsFlyer 不是通过普通点击链接完全独立归因 Google App Campaign，而是使用 Google 授权接口、Link ID、转化导入和事件映射来交换数据。^[raw/articles/115002504686-google-ads-integration.md]

Google Ads API 是另一个层面的能力：它是 Google Ads 的程序化接口，用于账号、广告系列、报告、预算、出价、素材等对象的自动化管理。X Wallet 做投放自主化时，应把 AppsFlyer 对接视为“归因和事件回传层”，把 [[google-ads-api]] 视为“广告账号操作和报告自动化层”。^[raw/articles/20260624-google-ads-api-get-started.md]

## 必备配置
- 在 Google Ads 创建 Third-party app analytics 的 Link ID，并选择 AppsFlyer。
- 在 AppsFlyer Partner Marketplace 启用 Google Ads，填入 Link ID。
- 确保 App SDK 采集 IDFA / GAID；这是 Google Ads 对接前提之一。
- 导入 `first_open`；如果跑再互动，导入 `session_start`。
- 在 AppsFlyer 映射应用内事件到 Google Ads，并选择合适的事件来源。

## 推荐窗口
Google 默认点击回溯窗口 30 天，浏览回溯窗口 24 小时。AppsFlyer 文档建议 Google Ads 点击窗口设置为 30 天、浏览窗口设置为 1 天，以减少差异。详见 [[lookback-window-strategy]]。

## 事件回传原则
首次启用 Google Ads 应用内事件映射时，`af_app_opened` 会自动定义并映射到 `session_start`；事件来源建议先设为“所有媒体渠道，包括自然流量”，这样 Google 能收到足够信号。后续如果要限制优化信号，再按投放目标调整。^[raw/articles/115002504686-google-ads-integration.md]

## iOS 隐私注意点
Google Ads 在 iOS 14+ 下不能按旧的设备 ID 逻辑理解所有库存。Google iOS 应用不总是使用 ATT 框架下的 IDFA；iOS ACI 搜索库存不归因，iOS ACI 展示库存需要广告主 App 和媒体侧 App 都有设备 ID 授权，且 Google 目前仅对 iOS 点击归因、不对展示归因。没有设备 ID 的场景下，Google 会使用模型转化以及 gbraid 等标识符支持部分 iOS 再互动和聚合口径。详见 [[ios-privacy-skan-aem]]。^[raw/articles/115002504686-google-ads-integration.md]

对 X Wallet 来说，Google iOS App Campaign、YouTube、Web-to-App、再互动要分开复盘。看不到完整用户级归因时，先检查 Link ID、转化导入、事件来源、SKAN/隐私口径，再判断投放质量。

## 多账号/MCC
同一个 App 的 Android 和 iOS 需要不同 Link ID。多个账号投同一个 App 时，应共享同一个 Link ID，而不是让每个代理各自创建。最高层 MCC 或广告主主账号持有 Link ID 通常更易治理。详见 [[agency-and-mcc-governance]]。

## API 自动化注意点
如果 X Wallet 要自建 Google Ads 自动化，需要先确认公司是否已有 developer token；Google 通常每家公司授予一个 token。内部自动化优先使用服务账号工作流，并只给必要账号和必要权限。所有写操作（预算、出价、素材、campaign 状态）都应先从测试账号或低风险账号灰度，不应让 agent 直接无审计地改生产投放。详见 [[google-ads-api]]。^[raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md]

## 相关页面
- [[google-vs-meta-integration]]
- [[google-ads-api]]
- [[postback-and-event-mapping]]
- [[data-discrepancy-playbook]]
