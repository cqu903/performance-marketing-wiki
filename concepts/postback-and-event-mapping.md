---
title: 事件回传与映射
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [event-mapping, postback, google-ads, meta-ads, data-quality]
sources: [raw/articles/115002504686-google-ads-integration.md, raw/articles/4410480904081-meta-ads-in-app-event-mapping.md, raw/articles/4417303339921-google-ads-faq-discrepancies.md, raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md]
confidence: high
---

# 事件回传与映射

事件回传不是“把所有事件都传给广告平台”。正确做法是：把能表达用户质量、且量级足够让模型学习的关键事件传给 Google / Meta，避免把噪声事件当优化目标。^[raw/articles/115002504686-google-ads-integration.md]

事件来源范围也不是简单的“各渠道只收各自归因事件”。对注册完成、申请开始、申请提交这类中浅层关键事件，冷启动和验证期通常使用“所有媒体渠道，包括自然流量”，以补足训练和事件识别信号；对审批通过、授信额度、放款成功、金额等深层敏感事件，再按量级、合规和渠道质量决定是否收紧为“仅该合作渠道”。详见 [[event-source-scope-and-learning]]。^[raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md]

## Google Ads
- 必须导入 `first_open`，否则 Google Ads 可能看不到安装转化。
- 再互动广告建议导入 `session_start`；不导入会造成 AppsFlyer 和 Google Ads 再营销数据巨大差异，也会影响 Google 优化人群。
- 首次映射应用内事件后，需要实际触发事件，最多等待约 6 小时才可能在 Google Ads 中看到。^[raw/articles/4417303339921-google-ads-faq-discrepancies.md]
- 事件来源初期建议选择“所有媒体渠道，包括自然流量”，以便 Google 能收到足够训练信号；之后再根据投放目标收紧。

## Meta ads
- 在 AppsFlyer 中配置 Meta ads 应用内事件映射。
- iOS 事件还要关注 AEM 条件；若 AEM 不合格，检查事件是否映射到所有媒体源、Meta 事件管理器连接方式，以及等待 2-3 天生效。
- Meta 数据差异通常来自归因窗口、隐私限制、事件去重/延迟和平台自归因口径。

## iOS 隐私下的事件设计
- SKAN 转化值只覆盖安装后早期窗口，贷款 App 应优先放入能在 24-48 小时内稳定触发的质量信号：注册完成、申请开始、资料提交、初审通过。
- ATT 未授权用户缺 IDFA，广告平台更依赖汇总/建模信号；事件命名、触发时机和来源范围要稳定，避免频繁改定义。
- S2S 深层事件如果用于 Meta AEM，要确认 IP、IDFV、IDFA 空/全零处理符合 Meta 要求；否则事件可能不合格。
- 放款成功适合做价值校验和后端 cohort 分析，但量级不足时不宜直接作为 iOS 冷启动唯一优化目标。详见 [[ios-privacy-skan-aem]]。

## X Wallet 推荐事件层级
详见 [[xwallet-event-taxonomy]]。简化版：
1. `af_complete_registration` 或自定义注册完成：量大，适合早期学习。
2. 申请开始/提交资料：比注册更接近业务质量。
3. 授信/审批通过：质量高但量可能不足，适合做价值回传或深层优化测试。
4. 放款成功：终局价值事件，通常量低，不一定适合作为唯一优化目标。

## 验收清单
- AppsFlyer Raw Data 能看到事件。
- AppsFlyer Partner 配置中事件已映射。
- Google Ads / Meta 侧能看到对应转化 action/event。
- 事件来源、归因窗口、币种和时区记录清楚。

## 相关页面
- [[xwallet-event-taxonomy]]
- [[event-source-scope-and-learning]]
- [[google-vs-meta-integration]]
- [[data-discrepancy-playbook]]
- [[ios-privacy-skan-aem]]
