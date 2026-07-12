---
title: UAC 算法内部解构：RankScore、信号密度与资产混合
created: 2026-07-12
updated: 2026-07-12
type: concept
tags: [google-ads, acquisition, algorithm]
sources: [raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]
confidence: high
---

# UAC 算法内部解构

UAC 与传统 Search/Display 广告有本质区别：它是一个高度自动化的"黑盒"系统。理解底层运作机制才能有效实施 UAC 2.5 策略。^[raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]

## RankScore 核心竞价公式

每次潜在展示机会，Google 实时计算：

$$RankScore = Bid \times p(Conversion) \times E(Value) + QualityScore$$

| 变量 | 含义 | UAC 2.5 中的角色 |
|---|---|---|
| **Bid** | 广告主设定的 tCPA / tROAS | 决定出价上限 |
| **p(Conversion)** | 系统预测用户看到广告后执行目标动作的概率 | **UAC 2.5 核心变量**——代理事件的质量直接决定预测准确度 |
| **E(Value)** | 期望价值，仅在 tROAS 策略生效 | 深层付费/营收事件的期望值 |
| **QualityScore** | 广告质量分 | 受 CTR、素材相关性、版位表现综合影响 |

预测值 × 出价高于竞争对手 → 广告展示。

## 神经网络信号输入

系统通过输入层接收数百万信号（Signals）：

1. **用户上下文**：设备型号、OS 版本、地理位置、时间、网络环境（WiFi/4G/5G）
2. **用户行为画像**：Google 生态（Search、YouTube、Play）中的历史搜索词、观看视频类型、安装应用类型
3. **创意素材特征**：视频色调、节奏、文本语义、图片物体识别结果

隐藏层复杂计算后输出 0 到 1 的概率值。

### 特征工程视角
UAC 2.5 的成功本质是"特征工程"的胜利——选择好的代理事件等于帮助神经网络筛选出能区分"高质量 vs 低质量用户"的关键特征。
- 事件太浅（如"打开应用"）→ 特征过于泛化，无法区分噪音
- 事件太深 → 样本太少，梯度下降无法收敛

## 冷启动的贝叶斯数学解释

冷启动在数学上是**贝叶斯先验概率**问题：

- 新 Campaign 上线时，系统对 p(Conversion) 的估计基于全局平均水平（Global Prior），方差极大
- **UAC 1.0**：安装数据源源不断 → 后验概率（Posterior）迅速收敛 → 方差快速减小 → 自信出价
- **UAC 3.0**：深度事件稀疏 → 后验更新极慢 → 长时间高方差 → 不敢出价（跑不动）或乱出价（成本飙升）
- **UAC 2.5**：引入 ~20% 发生率的代理事件 → 信号密度提升一个数量级 → 大数定律下 3-5 天跨过置信度阈值

这与 [[bayesian-beta-smoothing]] 的 Beta 共轭先验框架完全一致——代理事件策略本质上是在为算法提供更多后验更新样本。

## 资产动态组合（Asset Mixing）

UAC 不再人工拼接"标题+图片"，而是将所有上传资产视为"资产池"：
- 每次竞价时，算法根据当前版位（Placement）和用户特征，动态从池中抽取元素组合成广告单元
- 场景 A（YouTube Shorts）→ 抽取竖屏视频 + 短标题 + "安装"按钮
- 场景 B（Google Search）→ 抽取 App Icon + 长标题 + 描述 + 评分星级

### "素材即定向"（Creative is Targeting）

素材不仅是给用户看的，更是给算法看的：
- UAC 1.0 风格素材（"免费下载、红包"）→ NLP + 图像识别匹配 → 投放给对"诱饵"感兴趣的人 → 大量低质量安装
- UAC 2.5 风格素材（"核心玩法、策略深度、高级功能"）→ 语义匹配 → 展示给搜索相关关键词或观看硬核内容的用户

**结论：素材语义必须与代理事件性质高度一致。** 如果素材强调"免费"但代理事件是"付费"，算法人群与优化目标错配。

### Ad Strength 指标
Google Ads 内部评估资产覆盖完整度（极差→极佳四等级）：
- 文本：5 标题 + 5 描述
- 图片：20 张，覆盖 1.91:1（横）、1:1（方）、4:5（竖）
- 视频：20 个，覆盖 16:9、1:1、9:16
- HTML5：20 个（Playable Ads）

缺任何比例 = 放弃对应版位流量。详见 [[evc-optimization-playbook]] 分版位素材优化。

## 对 RankScore 竞价联动的推导

1. 好 EVC 素材 → 10s 观看率高 → QualityScore 上涨
2. QualityScore 高 → 竞价排名变强 → 同样出价下拿到更多曝光
3. 更多曝光 → 更多 EVC → 算法学习样本更多 → p(Conversion) 预测更准
4. 正循环形成：好素材 → 更高质量分 → 更低成本曝光 → 更多 EVC

这与 [[evc-optimization-playbook]] 中 RankScore 竞价联动段落一致，但本页补充了底层公式推导。

## 相关页面
- [[golden-proxy-event]] — 基于 RankScore 公式选择最优 p(Conversion) 目标事件。
- [[uac]] — UAC 版本总览。
- [[bayesian-beta-smoothing]] — 冷启动贝叶斯框架在素材评级中的应用。
- [[evc-optimization-playbook]] — 素材分版位优化与 Ad Strength。
- [[creative-correlation-analysis]] — 量化素材特征对转化的驱动规律。
