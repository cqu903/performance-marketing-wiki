# Wiki Schema

## Domain
效果营销知识库。覆盖：移动归因（AppsFlyer / SKAN / ATT）、投放平台机制（Google Ads / UAC / DV360 / Meta / TikTok）、代理体系、衡量与优化、素材与创意、数据差异与隐私、平台政策变更，以及 X Wallet（香港持牌贷款 App）业务场景应用。

## Conventions
- 文件名：英文小写短横线，无空格（如 `dv360-vs-google-ads.md`）
- Wiki 页面使用 YAML frontmatter；`raw/` 原始资料也带 `source_url`、`ingested`、`sha256`
- 页面之间使用 `[[wikilinks]]`，每个概念/对比/查询页至少 2 个出站链接
- 更新页面时必须 bump `updated` 日期
- 新页面必须加入 `index.md`；每次动作必须追加 `log.md`
- `raw/` 为不可变原始资料，只追加新版本，不直接改旧来源
- 多来源综合页在关键段落后使用 provenance marker：`^[raw/articles/xxx.md]`

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
confidence: high | medium | low
contested: true                  # optional
contradictions: [page-slug]      # optional
---
```

## Tag Taxonomy
- **平台/对象**：appsflyer, google-ads, dv360, meta-ads, tiktok, youtube, agency, srn, mmp
- **归因机制**：attribution, lookback-window, device-id, skan, retargeting, reattribution, view-through
- **配置**：integration, event-mapping, postback, conversion-import, cost, permissions, timezone
- **投放与优化**：bidding, targeting, creative, frequency-capping, brand-safety, spo, pmp, programmatic, lift-testing, uac, pmax
- **质量控制**：discrepancy, troubleshooting, checklist, privacy, data-quality, ivt, brand-lift
- **业务场景**：xwallet, hk-market, lending-app, acquisition, remarketing, branding
- **行业**：agency-tier, rebate, account-setup, compliance
- **元维基**：comparison, timeline, policy-change, reference

Rule: 每个页面的 tag 必须出现在 taxonomy 中。需要新 tag 先在 SCHEMA 加，再用。

## Page Thresholds
- 核心实体（AppsFlyer、Google Ads、DV360、Meta ads、UAC、代理商）单独建 entity 页。
- 涉及配置决策、归因准确度、优化杠杆、关键事件回传的主题建 concept / comparison / query 页。
- 只出现一次且不影响配置/优化的术语不单独建页。
- 页面超过约 200 行时拆分。
- Archive：内容完全被新内容取代时移到 `_archive/`，从 index 删除。

## Update Policy
当新来源与旧内容冲突：先看来源更新时间；无法判断时保留两种说法，标记 `contested: true` 并在 log 中提示人工复核。
