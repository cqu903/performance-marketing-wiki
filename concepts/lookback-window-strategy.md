---
title: 回溯窗口策略
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [lookback-window, attribution, google-ads, meta-ads, discrepancy]
sources: [raw/articles/208338403-lookback-windows.md, raw/articles/115002504686-google-ads-integration.md, raw/articles/207033826-meta-ads-integration.md]
confidence: high
---

# 回溯窗口策略

回溯窗口决定一次点击/展示之后多长时间内的激活仍可归因给该触点。窗口过长会抬高付费渠道归因；窗口过短会把真实受广告影响的转化打成自然量。^[raw/articles/208338403-lookback-windows.md]

## AppsFlyer 基础规则
- 点击回溯窗口：通常 1-30 天或 1-23 小时；默认 7 天。
- 浏览回溯窗口：通常 1-24 小时；默认 24 小时。
- 概率模型：自动调整，最长 24 小时，不受链接参数中的较长窗口影响。
- SRN 应优先与各自平台默认窗口对齐，降低数据差异。

## Google / Meta 建议值
| 渠道 | 点击窗口 | 浏览窗口 | 备注 |
|---|---:|---:|---|
| Google Ads | 30 天 | 24 小时 | AppsFlyer 文档建议 Google 点击 30 天、浏览 1 天；iOS Google 不认领展示时无需启用浏览归因。 |
| Meta ads | 7 天 | 24 小时 | 与 Meta 默认一致，减少 Meta 与 AppsFlyer 差异。 |

## X Wallet 实操建议
1. 拉新 Campaign：Google 30d click / 1d view；Meta 7d click / 1d view。
2. 再营销 Campaign：单独启用再互动窗口，避免把老用户行为混入拉新质量评估。
3. 每次改窗口必须记录日期；改动前后至少分段看 7-14 天，不要把窗口变化误判为素材或出价变化。

> **注意区分**：本页讲的是 MMP 侧的归因回溯窗口（click/view lookback），与 [[uac-bidding-and-operations]] 中 Google Ads 内部的"转化窗口"（Conversion Window）是不同层面的概念。前者决定哪些触点能被归因，后者决定广告点击后多久内的转化被算法计入学习。

## 相关页面
- [[google-vs-meta-integration]]
- [[data-discrepancy-playbook]]
- [[attribution-model]]
- [[deeplink-deferred-deeplink-limitations]] — DDL 场景下的 15 分钟"点击→首开"匹配窗口（与本页 MMP 归因回溯窗口属不同层面）。
