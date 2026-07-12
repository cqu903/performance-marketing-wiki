---
title: Universal App Campaign (UAC)
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [google-ads, acquisition, xwallet]
sources: [raw/articles/20260712-uac-quantitative-analysis-handbook.md, raw/articles/20260712-evc-observation-optimization-mmp-scope.md, raw/articles/20260712-ios-w2a-vs-android-a2a-uac-strategy.md, raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]
confidence: high
---

# Universal App Campaign (UAC)

UAC（现 Google 官方称 Google App Campaign）是 [[google-ads]] 面向移动 App 安装和互动的自动化投放产品。X Wallet 在 Google 渠道的投放主要通过 UAC 执行。UAC 的核心特征是：算法自动管理版位、出价、人群，广告主只需设定目标和预算。

## 版本与优化目标

- **UAC 1.0**：安装优化（tCPI），以 first_open 为目标。
- **UAC 2.5**：应用内事件优化（tROAS / tCPA），支持 [[evc-engaged-view-conversion|EVC]] 等深度转化目标。X Wallet 目前核心使用 UAC 2.5。
- **深度优化**：注册/留资/付费等深度行为事件，需要足够转化样本量才能稳定学习。

## 强制分平台机制

Google App Campaign 创建时**强制分平台**：一个 Campaign 只能选 iOS 或 Android，天然支持两套独立策略：
- iOS 计划：W2A 链路 + 浅层优化目标 + SKAN CV 编码。
- Android 计划：原生 A2A + 深度事件 + tROAS 出价。

详见 [[ios-w2a-vs-android-a2a]]。

## 版位覆盖

- YouTube Shorts（竖版 9:16，EVC 核心流量位）
- YouTube In-Stream（横版 16:9，长叙事种草）
- AdMob 激励视频（以点击转化为主，基本无 EVC）
- Google 搜索/展示/Discover

## EVC 与 UAC 2.5

UAC 2.5 的深度优化依赖 [[evc-engaged-view-conversion|EVC]] 作为优质流量信号。EVC 用户普遍留存更高、LTV 更高，是模型深度学习的关键输入。素材优化目标：撑满 10 秒观看门槛把用户留在 EVC 归因池。详见 [[evc-optimization-playbook]]。

## 量化分析工具

| 工具 | 适用场景 | 详见 |
|---|---|---|
| [[creative-correlation-analysis]] | 大样本（≥100 转化）：皮尔逊+斯皮尔曼双相关系数量化特征→转化规律 | concept 页 |
| [[bayesian-beta-smoothing]] | 小样本（<100 转化）：贝叶斯 Beta 共轭先验平滑转化率偏差 | concept 页 |

两套方法联动构成"大样本挖特征 / 小样本平滑评级"的双模型工作流。

## RankScore 竞价联动

UAC 的竞价基于 RankScore 机制：10s 观看率属于广告质量分的正向因子；EVC 表现优秀的素材质量分上涨 → 竞价排名变强 → 拿到更多低成本曝光 → 形成"好素材 → 更高质量分 → 更低成本曝光 → 更多 EVC"的正循环。

## X Wallet 投放策略要点

1. iOS / Android 分计划搭建，完全隔离出价和转化跟踪。
2. 素材复盘不能只看点击 CPA——优质种草向素材点击偏低但 EVC 转化量高、LTV 强。
3. tROAS 出价必须纳入 EVC 营收做整体 ROAS 核算。
4. 归因窗口：安装 30 天 / 深度付费 60 天，不可盲目压缩。

## 相关页面
- [[google-ads]] — UAC 所在的 SRN 对接与 AppsFlyer 集成层。
- [[golden-proxy-event]] — UAC 2.5 核心策略框架：代理事件筛选三步法。
- [[uac-algorithm-internals]] — UAC 算法内部解构：RankScore 公式与冷启动机制。
- [[uac-bidding-and-operations]] — 三阶段出价迁移、预算配置与运维排查。
- [[enhanced-conversions-app]] — 增强型转化与混合变现数据工程。
- [[evc-engaged-view-conversion]] — UAC 2.5 核心优质流量指标。
- [[evc-optimization-playbook]] — UAC 素材分版位优化与出价实操。
- [[ios-w2a-vs-android-a2a]] — UAC 投放平台差异化策略。
- [[creative-correlation-analysis]] — UAC 素材特征→转化量化分析。
- [[bayesian-beta-smoothing]] — UAC 小样本转化率修正。
- [[lookback-window-strategy]] — UAC 归因窗口配置。
