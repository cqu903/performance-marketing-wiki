---
title: Google Ads 广告类型 × 适用目标决策框架
created: 2026-07-19
updated: 2026-07-19
type: concept
tags: [google-ads, pmax, uac, acquisition, branding, remarketing, targeting, reference, xwallet]
sources: [raw/articles/20260719-google-ads-types-by-goal-infographic.md]
confidence: high
---

# Google Ads 广告类型 × 适用目标决策框架

> 一张图看清 Google Ads 9 种广告类型分别最擅长什么目标，用于排兵布阵前的第一层判断。
> 原始信息图存档见 `raw/articles/20260719-google-ads-types-by-goal-infographic.md`。^[raw/articles/20260719-google-ads-types-by-goal-infographic.md]

## 营销漏斗定位
任何广告类型在决策前，先归位到漏斗的四个阶段：
1. **制造需求**（Build Demand）——建立品牌认知、激发潜在兴趣。
2. **承接需求**（Capture Demand）——捕捉主动搜索、明确意向用户。
3. **促成转化**（Drive Conversion）——把意向转化为安装/购买/到店。
4. **再营销/激活**（Remarketing/Activation）——回访未转化、唤醒老用户。

漏斗上游的预算应服务于下游转化效率；脱离漏斗位置只比"哪个广告类型便宜"是常见误区。

## 适用性符号
- **✓** 适合（首选）
- **●** 可用但非首选
- **—** 不适合

## 广告类型 × 目标矩阵

| 广告类型 | 品牌曝光 | 需求激发 | 网站流量/线索 | App 安装 | App 内转化 | 电商销售 | 本地到店 | 再营销 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 搜索广告（Search） | ● | — | ✓ | — | ● | ● | ● | ● |
| Performance Max（PMax） | ● | ● | ✓ | — | — | ✓ | ✓ | ✓ |
| App Campaigns（UAC） | ● | ● | — | ✓ | ✓ | — | — | ✓ |
| Demand Gen | ✓ | ✓ | ✓ | ● | — | ● | — | ✓ |
| 视频广告（Video） | ✓ | ✓ | ● | ● | — | ● | ● | ✓ |
| 展示广告（Display） | ✓ | ● | ● | — | — | ● | ● | ✓ |
| 购物广告（Shopping） | — | — | ● | — | — | ✓ | ● | ● |
| 智能广告（Smart） | ● | — | ● | — | — | ● | ● | — |
| 本地/酒店/垂类广告 | ● | — | ● | — | — | ● | ✓ | ● |

## 每种类型的定位与适用

- **搜索广告（Search）**——承接高意图搜索。适合获客、咨询、申请、官网转化。用户主动表达需求，转化路径短，CPA 通常最稳。
- **Performance Max（PMax）**——跨渠道自动化。适合电商销售、线索收集、门店到店。一个系列覆盖 Google 全库存（Search/Display/YouTube/Gmail/Discover/Maps），机器学习自动分配，控制粒度最弱但覆盖最广。
- **App Campaigns（UAC）**——专注 App 增长。适合安装、注册、激活、付费、申请等应用内事件。详见 [[uac]]、[[uac-bidding-and-operations]]、[[uac-algorithm-internals]]。
- **Demand Gen**——信息流式广告（Discover/Gmail/YouTube Feed）。适合品牌种草、拉新、视觉化创意测试。替代已下线的 Discovery Ads，是 Facebook/Instagram Feed 的 Google 侧对标。
- **视频广告（Video）**——品牌讲述、产品教育、YouTube 触达与再营销。可与 UAC 协同产出 EVC（互动浏览转化），详见 [[evc-engaged-view-conversion]]、[[evc-optimization-playbook]]。
- **展示广告（Display）**——大范围覆盖与低成本再营销。注意流量质量，GDN 网络质量参差，通常作为辅助而非主力。
- **购物广告（Shopping）**——电商商品销售。直接展示商品、价格与商家信息，承接高购买意图。App 产品不适用。
- **智能广告（Smart）**——中小商家快速投放。简单但控制力较低，不推荐规模化或精细化运营使用。
- **本地/酒店/垂类广告**——门店、酒店、旅游等垂直行业的到店或预订目标。X Wallet 纯数字业务基本不涉及。

## 决策速查
1. 要承接明确需求 → **搜索广告**。
2. 要做 App 增长 → **App Campaigns**。
3. 要做品牌种草 → **Demand Gen / Video**。
4. 要做电商成交或门店到店 → **PMax / Shopping / 本地广告**。

## X Wallet 应用注释

X Wallet 是香港持牌贷款 App，纯数字业务、无实体门店、无电商商品，因此：

- **主力渠道**：[[google-ads]] 体系下 X Wallet 的核心战场是 **App Campaigns（UAC）**——所有 App 安装、注册、授信申请、放款转化都走 UAC。出价迁移、代理事件选择、EVC 协同参见 [[uac-bidding-and-operations]]、[[golden-proxy-event]]。
- **承接搜索意图**：用户在 Google 搜索"贷款""借錢""private loan"等关键词时，**搜索广告**是承接明确借贷需求的最高效入口，CPA 可控、转化路径短。建议与 UAC 并行，UAC 负责拉新量级，Search 负责收割高意图搜索流量。
- **品牌种草**：**Demand Gen + Video** 组合用于教育市场认知（贷款产品特性、额度、放款速度），为搜索和 UAC 制造上游需求。Video 可同时为 UAC 产出 EVC 转化。
- **再营销**：对浏览落地页未完成申请的用户，**Display/Demand Gen 再营销 + UAC re-engagement** 形成回访闭环。
- **不适用**：Shopping（无实体商品）、本地/酒店/垂类广告（无门店）、Smart（控制力不足，不适合月预算 HKD 50 万级别的精细化运营）。
- **PMax 的取舍**：PMax 对 X Wallet 的价值有限——它强项在电商成交和到店，App 安装与 App 内转化在其矩阵里是"—"。X Wallet 的 App 增长应优先 UAC，PMax 仅在需要跨渠道覆盖 Web 落地页线索时考虑，且要注意 PMax 会与 Search/UAC 争抢预算和流量，需分层预算隔离。

## 相关页面
- [[google-ads]] — Google Ads 作为 SRN 的对接、Link ID、转化导入与 MCC 治理。
- [[uac]] — UAC 实体页：自动化投放、分平台隔离、EVC 与量化分析。
- [[uac-bidding-and-operations]] — UAC 三阶段出价迁移与运维排查。
- [[uac-algorithm-internals]] — UAC 算法内部解构。
- [[evc-optimization-playbook]] — EVC 分版位素材优化与协同。
- [[dv360-vs-google-ads]] — DV360 与 Google Ads 全维度对比。
- [[agency-and-mcc-governance]] — 代理与 MCC 治理。
