---
title: EVC 素材与投放优化实操手册
created: 2026-07-12
updated: 2026-07-12
type: concept
tags: [google-ads, acquisition, xwallet]
sources: [raw/articles/20260712-evc-observation-optimization-mmp-scope.md]
confidence: medium
---

# EVC 素材与投放优化实操手册

承接 [[evc-engaged-view-conversion]] 的指标定义与观测口径，本页聚焦"怎么做"：视频素材分版位怎么剪、出价怎么调、归因窗口怎么对齐、常见误区怎么避。^[raw/articles/20260712-evc-observation-optimization-mmp-scope.md]

## 一、视频素材分版位优化

核心逻辑：所有素材优化的目的是**撑满 10 秒观看门槛**，把用户留在 EVC 归因池里，再谈转化。

### YouTube Shorts（竖版 9:16，EVC 核心流量位）
- 前 3 秒强抓注意力：用高光爽点做开篇，UGC 真人解说风格，减少枯燥铺垫。
- 降低强 CTA 弹窗（"立即下载"按钮）：避免用户被按钮干扰、提前划走，提升 10s 观看完成率，扩充 EVC 池子。

### YouTube In-Stream（横版 16:9）
- 中长叙事素材，在第 7~9 秒安排高吸引力剧情拐点，把 10 秒留存拉高。
- 主打长线世界观种草——这类被动种草产生的 EVC 用户 LTV 显著偏高，适合筛选高价值付费人群。

### AdMob 激励视频
- 几乎无 EVC 产出（以点击转化为主），优化重心放在 CTR、CVR 即可，不纳入 EVC 体系评估。

## 二、出价、流量分配与竞价联动

### tROAS 出价必须纳入 EVC 营收
做 UAC 2.5 tROAS 目标设置时，不要只参考点击转化 ROAS，把 EVC 带来的营收并入整体 ROAS 核算；给 EVC 转化表现好的素材分配更多预算。

### 相似受众扩量
对持续产出高 LTV EVC 转化的受众包，开启相似受众（Lookalike）拓展，复制高质量被动种草人群。

### RankScore 竞价联动
10s 观看率属于广告质量分的正向因子；EVC 表现优秀的素材质量分上涨，竞价排名变强，拿到更多低成本 YouTube 曝光——形成"好素材 → 更高质量分 → 更低成本曝光 → 更多 EVC"的正循环。

## 三、归因窗口配置校准

在 MMP、Google Ads 之间对齐 EVC 归因窗口期：
- EVC 安装归因窗口建议 30 天。
- 深度付费转化窗口可拉长至 60 天。
- **不要盲目压缩浏览归因窗口期**——会直接截断大量真实 EVC 数据。

窗口策略细节见 [[lookback-window-strategy]]。

## 四、避坑清单（高频误区）

1. **只盯点击 CPA 做素材淘汰**：优质种草向素材点击偏低但 EVC 转化量高、用户 LTV 强，单看点击 CPA 会误杀。解决：复盘同时看 EVC 转化与 EVC-ROAS，配合 [[bayesian-beta-smoothing]] 做小样本公平评级。

2. **混淆普通曝光浏览转化（VT）与 EVC**：普通 VT 仅要求曝光完成，EVC 强制 10s 观看门槛，二者用户质量差异显著，**不能合并做指标复盘**。

3. **金融/贷款品类适配**：YouTube 种草投放中，EVC 代表被动种草完成，该类 EVC 开户/入金用户的客单、留存质量往往优于主动点击人群。对 X Wallet 信贷场景，复盘投产务必纳入 EVC 营收做完整 ROAS 核算，不能只算点击转化的申请/放款成本。

   ⚠️ 合规提醒：涉及 X Cash A.I. Loan 的速度/批核表达，须遵守公司合规红线（禁止"保证批核""人人可借"等），用"平均审核时间约 5 秒"等带数据口径的表述。

## 相关页面
- [[evc-engaged-view-conversion]] — EVC 指标定义、观测口径、MMP 权责边界。
- [[creative-correlation-analysis]] — 量化素材留存指标对 EVC 的驱动规律，支撑本页的素材优化方向。
- [[bayesian-beta-smoothing]] — 小样本素材 EVC 转化率公平评级，支撑淘汰/放量决策。
- [[lookback-window-strategy]] — 归因窗口跨平台对齐原则。
- [[google-ads]] — UAC 投流媒体侧与 RankScore 竞价机制。
