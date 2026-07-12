---
title: UAC 出价迁移与运维排查手册
created: 2026-07-12
updated: 2026-07-12
type: concept
tags: [google-ads, acquisition, operations, xwallet]
sources: [raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]
confidence: high
---

# UAC 出价迁移与运维排查手册

承接 [[golden-proxy-event]]（代理事件选择）和 [[uac-algorithm-internals]]（算法原理），本页聚焦出价策略如何从冷启动迁移到深度优化，以及日常运维的检查清单和故障排查。^[raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]

## 三阶段出价迁移法

直接上线 tROAS 通常是灾难性的——数据稀疏导致算法无法收敛。应采用阶梯式迁移。

### 阶段一：基准建立期（Weeks 1-2）
- 出价策略：**Maximize Conversions**（最大化转化次数）
- 目标：[[golden-proxy-event|代理事件]]
- 设置：**不设 tCPA 上限**
- 目的：让算法自由探索，高价买入第一批核心种子用户，自动测算市场真实 CPA（如 $5.00），最快通过冷启动

### 阶段二：成本控制期（Weeks 3-6）
- 出价策略：**Target CPA (tCPA)**
- 设置：tCPA = 阶段一测出真实 CPA × 1.1（如 $5.50）
- 目的：保证量级前提下控制成本，算法已积累足够数据稳定运行 UAC 2.5 模型

### 阶段三：价值收割期（Weeks 7+）
- 出价策略：**Target ROAS (tROAS)**
- 前提：账户过去 30 天积累 ≥50 次真实"付费"转化（非代理事件）
- 操作：新建 Campaign，tROAS 目标设为"购买"
- 如果跑不动 → 数据仍不足 → 退回阶段二继续积累

### Google 官方 VBB（Value-Based Bidding）迁移条件
Google 官方文档（[12398131](https://support.google.com/google-ads/answer/12398131)）要求：过去 5 天至少 30 次转化才有资格切换到 tROAS 或 Max Conversion Value。tROAS 初始值建议设为低于历史 ROAS（如历史 500%，设 400%），给模型调整空间。

## 预算与出价黄金比例

日预算 ≥ 15 倍 × tCPA

- tCPA = $5 → 日预算 ≥ $75（建议 $100）
- 日预算仅 $20（4 倍 tCPA）→ 系统判为"Limited by Budget"→ 降频竞价 → 只拿低质量流量 → Campaign 跑死

Google 官方最佳实践（[14104492](https://support.google.com/google-ads/answer/14104492)）同样强调检查"Limited by budget"状态，预算受限时优先提高预算而非降低出价。

## 转化窗口微调

| 优化阶段 | 窗口建议 | 原因 |
|---|---|---|
| UAC 1.0（安装优化） | 30 天默认 | 安装高频，窗口影响小 |
| **UAC 2.5（代理事件）** | **缩短至 3-7 天** | 代理事件应在安装后很快发生；窗口过长 → 数据回传慢 + 晚转化用户价值偏低 |
| UAC 3.0（深度付费） | 30-60 天 | 深度转化天然滞后 |

代理事件窗口缩短操作：检查代理事件的时间分布曲线（Google Ads → Segment → Days to Conversion），找到覆盖 90% 转化所需天数，设为转化窗口。这迫使算法寻找"快速反应"高质量用户，加快学习速度。

Google 官方文档（[9832641](https://support.google.com/google-ads/answer/9832641)）提供"Days to Conversions"分段工具辅助选择窗口。

⚠️ 与 [[lookback-window-strategy]] 的区别：lookback-window-strategy 讲的是 MMP 侧的归因回溯窗口（Google 30d click / Meta 7d click），本页讲的是 Google Ads 内部的**转化窗口**（Conversion Window）设置，两者是不同层面的概念。

## 每日/每周检查清单

| 频率 | 检查项 | 指标 | 动作阈值 |
|---|---|---|---|
| 每日 | 预算消耗率 | Cost / Budget | < 50% 且 CPA 达标 → 提高出价或扩充素材 |
| 每日 | CPA 波动 | Actual CPA vs Target | ±20% 正常；连续 3 天超标 → 降级素材或收紧定向 |
| 每周 | 素材表现 | Asset Strength | 替换 3-5 个"Low"表现素材，上传新概念 |
| 每周 | 事件转化率 | Install-to-Event Rate | 骤降 → 检查 App 新版本 Bug 或新手引导过难 |
| 每双周 | 受众信号 | Audience Signal | 更新 Customer Match 列表（最新高价值用户） |

## 常见故障排查

### 故障一：Campaign 突然跑不动（零消耗或极低消耗）
- **原因 1：竞价竞争力下降** → 竞对提价 or 素材疲劳导致 CTR 下降拉低 eCPM
  - 解法：上传新素材（最有效），或微调 tCPA +10%
- **原因 2：风控/学习期重置** → 最近一次性修改 >20% 预算
  - 解法：不动，静置 3-5 天等待算法重新校准

### 故障二：CPA 达标，但后端 ROAS 极差
- **原因：代理事件选错** → 被羊毛党攻破，或与付费的相关性变弱
- 解法：重新做 [[golden-proxy-event]] 相关性分析；换更深事件（如"注册"→"身份验证"）；排除低质量设备/地域

### 故障三：Google Ads 转化数远多于 MMP
- **原因：浏览转化（View-Through）被计入**
- 解法：正常现象。Google Ads Columns 拆分浏览转化单独看。优化参考含浏览转化数据（代表助攻价值），业绩汇报剔除。

这与 [[data-discrepancy-playbook]] 的排查逻辑一致，但此处特指 Google DDA + SAN 自归因逻辑导致的 Google 侧转化数偏高。

## Google 官方 Maximize Conversions 补充

Google 官方文档（[16550675](https://support.google.com/google-ads/answer/16550675)）明确 Maximize Conversions 的三种变体：
- **Maximize conversions for installs**：最大化安装量，不关注 CPI/质量
- **Maximize conversions for in-app actions**：最大化应用内行为，适合 iOS（ATT 限制下难以设定精确目标）
- **Maximize conversion value**：最大化转化价值（搭配 tROAS 前的过渡）

iOS 场景因 ATT 限制，设定精确 tCPA/tROAS 困难，Maximize Conversions 是推荐的初始出价策略。

## 相关页面
- [[golden-proxy-event]] — 出价迁移的前提：先选对代理事件。
- [[uac-algorithm-internals]] — RankScore 竞价公式与算法学习机制。
- [[evc-optimization-playbook]] — EVC 视角下的出价与素材优化。
- [[lookback-window-strategy]] — MMP 侧归因回溯窗口（与 Google Ads 转化窗口的区别）。
- [[data-discrepancy-playbook]] — Google 与 MMP 数据差异排查。
- [[bayesian-beta-smoothing]] — 小样本素材/计划的转化率公平评级。
