---
title: DV360 衡量与优化框架
created: 2026-07-19
updated: 2026-07-19
type: concept
tags: [dv360, bidding, targeting, creative, frequency-capping, brand-safety, spo, lift-testing, view-through, policy-change, reference]
sources: [raw/articles/dv360-vs-google-ads-framework-2026.md]
confidence: high
---

# DV360 衡量与优化框架

DV360 区别于 [[uac]]/[[google-ads]] 的核心：衡量上多了可见度、触达、增量三层指标，优化上多了库存筛选、SPO、跨媒体频控、Custom Bidding 四个独有杠杆。^[raw/articles/dv360-vs-google-ads-framework-2026.md] 平台概述见 [[dv360]]，对比见 [[dv360-vs-google-ads]]。

## 衡量体系

### Floodlight 转化追踪基建

- Floodlight 标签隶属 Campaign Manager 360，覆盖 Web / App（结合 [[appsflyer|MMP]]）/ CTV（server-side）/ 线下转化
- 支持 1-30 天可配置 **view-through 归因窗口**
- DV360 不是 SAN（自归因网络），App 场景需 MMP 做实际归因判定——与 [[google-ads]]/[[meta-ads]] 不同
- **2026 年更新**：Floodlight 与 Inventory Availability 报表迁移至 Instant Reporting，6 月起分阶段上线

### 指标分层

| 层级 | 核心指标 | 衡量方式 |
|------|---------|---------|
| 投放健康 | Win rate、Pacing、CPM | 平台原生报表 |
| 曝光质量 | Active View 可见度、IVT 率 | 内置 Active View + 第三方验证（IAS/DoubleVerify） |
| 触达 | Unique Reach、跨媒体频次分布 | Reach 报表；2026 起新增 Reach Overlap |
| 品牌 | 认知/记忆/购买意向 | Brand Lift 调研 |
| 效果 | CPA、ROAS、post-view vs post-click 占比 | Floodlight / MMP |
| 增量 | 相对自然转化的提升 | Conversion Lift、Geo 实验 |

### post-view 转化不能忽视

程序化买量中 post-view 占比往往很高，只看 last-click 会系统性低估 DV360 贡献、高估搜索渠道效果。建议两条腿走路：
- 报表层启用 Data-Driven Attribution（DDA）
- 大预算 CTV/品牌盘做 Lift 实验，用增量数据说话

Deep-dive 路径：Floodlight raw log → BigQuery Data Transfer → 自建归因模型，不受界面报表维度限制。

## 优化杠杆（按优先级）

### 1. 出价
- **起步**：Fixed Bid 摸底价
- **进阶**：tCPA / tROAS 自动出价
- **成熟**：Custom Bidding（Goal Builder 或自定义脚本，按"可见度 × 转化概率"等变量给每次展示定价）——DV360 独有杀器

### 2. 库存与供应路径优化（SPO）
- 砍掉低可见度（<50%）、高 IVT 的 Exchange/网站
- 对稳定媒体谈 PMP / PG 锁量锁价

### 3. 频控
- 跨 Exchange 统一频次上限（3-5 次/周）——程序化买量最大浪费是同一用户被多交易所重复触达
- 用 Reach Overlap 报表识别跨 Campaign 重复触达

### 4. 受众
优先级：一方数据（CRM / Floodlight 再营销）> Google 受众 > 三方数据
- **Optimized Targeting**：ML 驱动的受众扩展，逻辑类似 [[uac-algorithm-internals|UAC 的受众信号]]——"提示"而非"圈定"
- **Lookalike 受众（2026 新）**：2026-06-15 起取代 Audience Expansion，支持 Narrow 2.5% / Balanced 5% / Broad 10%，默认排除种子，可跨 Campaign 复用

### 5. 素材与创意
- 素材尺寸全覆盖 + DCO（动态创意优化）
- 预算放 IO 层用自动分配，让系统在 line item 间调仓

### 6. 品牌安全（Brand Safety）
2026 年 8 月硬性截止：Digital Content Labels 和敏感类目排除弃用，由 Inventory Modes / Content Themes 取代。未完成映射的账户将变为 unrestricted serving。

## 优化操作节奏

| 阶段 | 动作 |
|------|------|
| 第 1-2 周 | Fixed Bid 探底 + 全库存跑量，确认 Floodlight/MMP 回传链路 |
| 第 3 周起 | 按可见度/CPA 砍低效库存，收紧跨 Exchange 频控 |
| 第 5 周起 | 切 tCPA/tROAS 或 Custom Bidding；配 Optimized Targeting / Lookalike 拓量 |
| 稳定期 | 每月一次 Lift 实验校准归因；持续 SPO，谈 PMP/PG 锁量 |

改动纪律与 [[uac-bidding-and-operations|UAC 运营]] 一致：一次一个变量。

## 2026 年平台变更速览

| 时间 | 变更 | 影响 |
|------|------|------|
| 2025-03 起 | Optimized Targeting 不再支持 Fixed Bidding line item | 固定出价账户需切自动/自定义出价 |
| 2026-06 | Floodlight/Inventory 报表迁移至 Instant Reporting | 报表口径切换期需交叉验证 |
| 2026-06-15 | Audience Expansion 弃用，Lookalike 受众取代 | 老设置不自动迁移，需手动重配 |
| 2026-08-12 | 创意轮播设置弃用，按出价目标自动匹配创意 | A/B 测试方法论需调整 |
| 2026-08 | 内容标签/类目排除弃用 → Inventory Modes/Content Themes | 品牌安全策略必须完成映射 |

## 相关页面
- [[dv360]] — DV360 平台实体
- [[dv360-vs-google-ads]] — 平台选择决策
- [[uac-bidding-and-operations]] — UAC 三阶段出价迁移（对照参考）
- [[uac-algorithm-internals]] — UAC 受众信号逻辑（Optimized Targeting 类比）
- [[agency-tier-system]] — DV360 seat/代理开户门槛
