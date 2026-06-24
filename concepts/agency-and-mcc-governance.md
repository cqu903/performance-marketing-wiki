---
title: 代理与 MCC 治理
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [agency, permissions, google-ads, cost, data-quality]
sources: [raw/articles/115003294203-agency-data-for-advertisers.md, raw/articles/115005972889-data-access-permissions.md, raw/articles/115004970386-google-ads-mcc-multiple-agencies.md, raw/articles/4417309205521-google-ads-cost-revenue.md, raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md]
confidence: high
---

# 代理与 MCC 治理

X Wallet 当前通过代理执行投放时，关键原则是：代理可以操作广告，但归因资产、事件定义、Link ID、成本连接和核心数据权限应由广告主持有或可随时接管。

## Google Link ID / MCC
- AppsFlyer 对每个 App 记录一个 Google Link ID；Android 和 iOS 分别配置。
- 多个 Google Ads 账号投同一 App 时，应共享同一个 Link ID。
- 建议由广告主主账号或最高层 MCC 创建并共享 Link ID，代理离场时更容易拆分。^[raw/articles/115004970386-google-ads-mcc-multiple-agencies.md]

## 代理数据权限
AppsFlyer 支持广告主、代理和广告平台的不同数据访问权限。代理流量需要在报表中可识别，广告主也要能看清哪些数据来自代理。^[raw/articles/115003294203-agency-data-for-advertisers.md]

## 成本连接
Google 成本、点击、展示数据依赖正确的 Google 账号连接；成本数据可能需要 ROI360。代理开启成本对接但没有正确 Customer ID 权限时，广告主看到的成本可能不完整。^[raw/articles/4417309205521-google-ads-cost-revenue.md]

## X Wallet 治理建议
1. 建立“账号资产表”：Google Link ID、Customer ID、MCC 层级、Meta App ID、AppsFlyer App ID、各账号 owner。
2. 代理只拿执行权限；事件映射、Link ID 创建、成本连接至少要广告主可见可控。
3. 每次新增代理/MCC 前，先确认 Recording Manager 和转化导入责任归属。
4. 报表按代理、渠道、campaign、国家/地区拆分，避免代理流量混入内部投放。
5. 如果启用 [[google-ads-api]]，资产表还要记录 developer token owner、Google Cloud Project、OAuth/service account owner、授权账号和权限级别。developer token 通常公司级复用，不应由代理私自持有。

## 相关页面
- [[google-ads]]
- [[google-ads-api]]
- [[data-discrepancy-playbook]]
- [[xwallet-implementation-checklist]]
