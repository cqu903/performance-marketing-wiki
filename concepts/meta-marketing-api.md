---
title: Meta Marketing API
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [meta-ads, integration, permissions, data-quality]
sources: [raw/articles/20260624-meta-marketing-api-overview-get-started.md, raw/articles/20260624-meta-marketing-api-auth-insights.md]
confidence: high
---

# Meta Marketing API

Meta Marketing API 是基于 Graph API 的广告自动化接口，用于跨 Facebook、Instagram、Messenger、WhatsApp 创建、管理和分析广告。它对应的是广告账号操作层：campaign / ad set / ad / creative 创建与管理、暂停删除、效果数据读取。它不同于 AppsFlyer 的 [[meta-ads]] 对接；后者主要解决归因、事件回传、AEM/SKAN 和跨渠道数据口径。^[raw/articles/20260624-meta-marketing-api-overview-get-started.md]

对 X Wallet 来说，Meta Marketing API 的价值主要有三类：第一，拉取 Ads Insights 做代理监督和内部 BI；第二，批量管理 campaign/ad set/ad/creative；第三，在有足够权限和审计机制后，让 agent 做半自动投放操作。不要一开始就把写权限开放给 agent。

## 广告对象结构

| 层级 | 作用 | X Wallet 管理重点 |
| --- | --- | --- |
| Campaign | 最高层结构，代表单一 objective，并约束下层广告配置 | 目标必须和业务阶段一致：拉新、再营销、深层事件优化不要混在同一目标逻辑里。 |
| Ad set | 配置预算、周期、targeting、billing、optimization goal、bid | 通常按 audience / placement / optimization event 拆，便于预算和效果归因。 |
| Ad creative | 视觉和素材元素；创建后不能修改，可在 creative library 复用 | 素材版本、文案、落地页和合规审核要有命名规则。 |
| Ad | 实际投放对象，绑定 creative，在 ad set 下运行 | 一个 ad set 内可放多个 ads 测试图片、视频、文案和 placements。 |

## 接入前提

1. 活跃 Meta ad account：用于运行 campaign、管理 billing、追踪花费和表现。
2. Meta Developer account。
3. Meta App：在 App Dashboard 创建，并添加 Marketing API product。
4. Authorization：给 app / user / system user 授予广告相关权限。
5. Authentication：每次 Marketing API 调用都需要 access token。
6. 对只读报表至少需要 `ads_read`；对管理广告需要 `ads_management`。

## 权限与 Access Tier

Meta 的 Marketing API Access Tier 控制 rate limits、Business Manager API 访问和 system user 容量。Limited access 是默认层级，主要用于开发，按 ad account 重度限流，不适合生产级 live advertisers；Full access 需要 App Review，按 ad account 轻度限流，并可访问完整 Business Manager / Catalog APIs。升级 Full access 的要求包括最近 15 天至少 500 次成功 Marketing API calls，且最近 500 次调用错误率低于 15%。^[raw/articles/20260624-meta-marketing-api-auth-insights.md]

一个关键差异：Meta 文档明确说所有 access level 的调用都针对 production data。它不像 Google Ads API 那样有 developer token 的测试账号访问级别作为天然隔离层。因此 X Wallet 做 Meta 自动化时，更要靠权限、灰度账号、操作白名单和审计日志控制风险。

## Token 选择

| Token 类型 | 适合场景 | 风险/注意点 |
| --- | --- | --- |
| User access token | Graph API Explorer 调试、人工授权流程 | 短期 token 通常 1-2 小时；长期 token 也可能因密码变化或权限撤销失效。 |
| System user access token | 服务器到服务器、定时脚本、内部数据管道、agent 后端 | 更适合生产自动化；需要在 Business Manager 中管理 system user 和资产权限。 |

Token 必须安全存储，通过 HTTPS 传输，权限最小化，并定期检查有效性。不要把 token 写入 wiki、代码仓库或聊天记录。

## Ads Insights API

Ads Insights API 提供 Meta ads 的 performance data 和 statistics，可以通过 ad account、campaign、ad set、ad 等对象的 insights edge 读取。默认 GET 通常返回过去 30 天基础指标；可以用 parameters、fields、breakdowns 精细控制，例如 `date_preset=last_7d`、`fields=clicks`、`breakdowns=gender`。^[raw/articles/20260624-meta-marketing-api-auth-insights.md]

X Wallet 如果要先落地一个安全版本，应从 Ads Insights 只读管道开始：按 ad account / campaign / ad set / ad 拉 spend、impressions、clicks、conversions、CPA 等指标，再和 [[postback-and-event-mapping]]、[[data-discrepancy-playbook]] 中的 AppsFlyer / Meta AEM / SKAN 口径分开核对。

## X Wallet 落地顺序

1. 在 [[agency-and-mcc-governance]] 的资产表中补充 Business Manager、Ad Account ID、Meta App ID、System User、token owner、权限范围。
2. 先申请和验证 `ads_read`，建立 Ads Insights 只读报表管道。
3. 核对 Meta Ads Manager、AppsFlyer、内部 BI 的时区、归因窗口、事件定义和 iOS AEM/SKAN 口径。
4. 再评估 `ads_management`；写操作必须有白名单、预算上限、人工确认阈值和审计日志。
5. Creative 自动化要和素材合规、命名规则、落地页规则绑定，不要只做“批量上传”。

## 相关页面

- [[meta-ads]]
- [[postback-and-event-mapping]]
- [[agency-and-mcc-governance]]
- [[data-discrepancy-playbook]]
- [[ios-privacy-skan-aem]]
