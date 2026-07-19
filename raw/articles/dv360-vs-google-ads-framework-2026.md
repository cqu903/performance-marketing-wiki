---
source_url: docx:DV360与GoogleAds对比及衡量优化框架.docx
ingested: 2026-07-19
sha256: 33e4f54f716bff719a18a25878beaadac733a6a904200ef0866c8e25f55cb8f0
---

# DV360 与 Google Ads 对比、DV360 衡量与优化框架

资料来源：Google 官方帮助中心、Google Marketing Platform 产品文档及行业公开报道　|　撰写日期：2026-07-18

## 一、DV360 与 Google Ads：本质区别

两者都归属 Google 广告生态，但定位、库存、控制粒度完全不同。Google Ads 是"自助式效果投放引擎"，DV360 是"程序化需求方平台（DSP）"，二者不是竞品关系，而是互补关系。

| 维度 | Google Ads | DV360 |
|------|-----------|-------|
| 平台定位 | 自助式广告激活，面向 Search、YouTube 等 Google 自有环境 | Google Marketing Platform 旗下 DSP，面向程序化媒体采买 |
| 库存范围 | Google O&O（Search/YouTube/Play/Gmail/Discover）+ AdMob/GDN 联盟 | O&O + 80 余家第三方 Ad Exchange，覆盖 Display/Video/CTV-OTT/Audio/DOOH |
| 买量方式 | 拍卖出价，系统自动化程度高（UAC/PMax 等黑盒产品） | 程序化 RTB + PMP（私有市场）+ PG（保量直采），交易方式可选 |
| 出价策略 | tCPA / tROAS / Maximize Conversions 等，自动出价为主 | 除自动出价外，支持 Custom Bidding（自定义出价脚本/公式） |
| 受众定向 | 受众信号（Audience Signal）辅助算法学习，不做硬性圈定 | 一方数据、Floodlight 再营销、三方数据、Lookalike 受众（2026 起替代 Audience Expansion） |
| 转化追踪 | Google Ads 自身转化标签，UAC 场景接 MMP | Floodlight（隶属 Campaign Manager 360），支持 1-30 天可配置的 view-through 归因窗口 |
| 控制粒度 | 黑盒为主，可控项少（预算/目标出价/素材/转化事件/受众信号） | 白盒：版位、频控、底价、Deal、Inventory Mode/Content Theme 均可精细配置 |
| 开户门槛 | 自助开户，无消耗门槛 | 通常需 seat/代理开户；建议月度 display+video 预算 ≥2.5 万美元再上，平台费约 10%-15% |
| 典型场景 | App 装机与深度转化、搜索/购物意图承接 | 品牌曝光、Reach 建设、CTV/DOOH、多媒体大促、精细化频控与供应路径优化（SPO） |

结论：买效果、跑 App 增长用 Google Ads；要跨媒体控制、品牌曝光或第三方/CTV/DOOH 库存才上 DV360。成熟出海团队的常见组合是 UAC 打效果盘 + DV360 做大推期品牌覆盖与高端库存保量。

## 二、DV360 衡量体系

### 2.1 转化追踪基建：Floodlight

DV360 的转化衡量核心是 Floodlight 标签（隶属 Campaign Manager 360），覆盖 Web、App（结合 MMP）、CTV（server-side）、线下转化多种场景，并支持可配置的 1-30 天 view-through 归因窗口。App 场景通常仍需接入 AppsFlyer/Adjust 等 MMP 做实际归因判定，因为 DV360 采买第三方库存时不是自归因网络（SAN），这一点和 Google Ads/Meta 不同。

2026 年更新：Floodlight 与 Inventory Availability 报表将从传统离线报表迁移至 Instant Reporting（即时报表），6 月起分阶段上线（首批影响 10% 用户）。建议提前熟悉新报表结构，避免报表口径切换期出现断档。

### 2.2 指标分层

| 层级 | 核心指标 | 衡量方式 |
|------|---------|---------|
| 投放健康 | Win rate、Pacing、CPM | 平台原生报表 |
| 曝光质量 | Active View 可见度、无效流量（IVT）率 | 内置 Active View + 第三方验证（IAS/DoubleVerify） |
| 触达 | Unique Reach、跨媒体频次分布 | Reach 报表；2026 年起新增 Reach Overlap 维度 |
| 品牌 | 认知/记忆/购买意向提升 | Brand Lift 调研 |
| 效果 | CPA、ROAS、post-view vs post-click 转化占比 | Floodlight / MMP |
| 增量 | 相对自然转化的提升幅度 | Conversion Lift、Geo 实验 |

**关键认知：post-view 转化不能忽视**
程序化买量中 post-view（浏览后转化）占比往往很高，只看 last-click 会系统性低估 DV360 的贡献、高估搜索渠道的效果。建议两条腿走路：报表层面启用 Data-Driven Attribution（DDA），验证层面对大预算的 CTV/品牌盘做 Lift 实验，用增量数据说话而不是单纯依赖归因模型的数字。

技术路径：deep-dive 分析可通过 Floodlight raw log 数据经 BigQuery Data Transfer 导出，自建归因模型或跨渠道联合分析，不受 DV360 界面报表维度限制。

## 三、DV360 优化杠杆（按优先级）

### 3.1 出价

- **起步**：Fixed Bid 测市场真实成交价，摸清底价水位；
- **进阶**：转 tCPA / tROAS 自动出价，让系统按目标自动调整；
- **成熟**：上 Custom Bidding——用 Goal Builder 或自定义脚本，按"可见度 × 转化概率""客单价""品牌忠诚度权重"等自定义变量给每次展示定价，这是 DV360 相对 Google Ads 的独有杀器。

2026 年硬性变化：自 2025 年 3 月起，Optimized Targeting（优化定向）已不再支持 Fixed Bidding 的 line item——必须切到自动出价或 Custom Bidding 才能开启受众智能扩展。仍在用固定出价 + 优化定向组合的账户需要尽快迁移。

### 3.2 库存与供应路径优化（SPO）

- 砍掉低可见度（<50%）、高无效流量的 Exchange/网站；
- 对效果稳定的媒体谈 PMP（私有市场）或 PG（保量直采）锁量锁价；
- 2026 年起：非 PG 类 Deal 的手动创建已下线，必须通过 Deal Sync API 创建——技术团队需要提前把手动建 Deal 的工作流迁移为 API 对接。

### 3.3 频控

- 跨 Exchange 统一频次上限（如 3-5 次/周），程序化买量最大的浪费就是同一用户被多个交易所重复触达；
- 结合 Reach Overlap 报表识别跨 Campaign 的重复触达，及时收紧。

### 3.4 受众

优先级：一方数据（CRM / Floodlight 再营销）> Google 受众 > 三方数据；

- **Optimized Targeting（优化定向）**：机器学习驱动的受众扩展，基于历史表现、Floodlight 转化信号和上下文预测高转化用户，逻辑上和 UAC 的"受众信号"类似——都是"提示"而非"圈定"；
- **Lookalike 受众（2026 新）**：2026 年 6 月 15 日起，Audience Expansion（受众扩展）已被 Lookalike 受众取代（先适用于 YouTube Video reach / Video view 系列）。老设置不会自动迁移，需要手动重新配置。新工具相比旧版有本质提升：

| 维度 | Audience Expansion（旧，已弃用） | Lookalike 受众（新） |
|------|--------------------------------|--------------------|
| 扩展规模上限 | 1% | Narrow 2.5% / Balanced 5% / Broad 10% |
| 种子排除 | 默认不排除已在名单内用户 | 默认自动排除种子名单，聚焦拓新 |
| 复用性 | 单次生效，绑定单个 Campaign | 可保存并跨 Campaign 复用 |

### 3.5 素材与创意

- 素材尺寸全覆盖 + DCO（动态创意优化），逻辑与 UAC 的资产覆盖率异曲同工；
- 预算放在 IO（Insertion Order）层用自动分配，让系统在多个 line item 间自动调仓。

2026 年 8 月起变化：创意轮播设置（Creative Rotation）弃用，改为按出价目标自动匹配创意——Max Conversions 目标会优先展示预测转化最高的创意，Max Clicks 优先点击率最高的创意，Fixed Bidding/Maximize Viewable Impressions/成本类出价仍保持均匀轮播。依赖固定轮播做 A/B 测试的团队需要调整测试方法论。

### 3.6 品牌安全（Brand Safety）——2026 年 8 月硬性截止

传统的 Digital Content Labels（内容标签）和敏感类目排除将于 2026 年 8 月弃用，由更精细的 Inventory Modes（库存模式）和 Content Themes（内容主题）取代；广告主层面的类目排除（如新闻、政治）同步由 Content Themes 替代。

Google 官方明确警告：未在 8 月前完成迁移映射的账户，投放将变为不受限制（unrestricted serving）——这是硬性截止日期，不是软性建议。有品牌安全排除策略的团队应尽快完成映射。

## 四、优化操作节奏

| 阶段 | 动作 |
|------|------|
| 第 1-2 周 | Fixed Bid 探底 + 全库存跑量收集数据，同时确认 Floodlight/MMP 回传链路无误 |
| 第 3 周起 | 按可见度/CPA 数据砍低效库存，收紧跨 Exchange 频控 |
| 第 5 周起 | 切换 tCPA/tROAS 自动出价或上线 Custom Bidding；配置 Optimized Targeting 或 Lookalike 受众做拓量 |
| 稳定期 | 每月做一次 Lift 实验校准归因；持续做 SPO，谈判 PMP/PG 锁定优质库存 |

改动纪律与 Google Ads/UAC 一致：一次一个变量，避免多因素同时调整导致无法定位效果变化的真实原因。

## 五、与 Google Ads / UAC 的本质区别小结

UAC 是"喂对信号，让黑盒自己跑"——可控项只有预算、目标出价、素材、转化事件、受众信号五个维度，衡量体系相对简单（CPA/ROAS/学习期状态）。

DV360 是"自己当算法"——衡量上多了可见度、触达（Reach）、增量（Lift）三层指标，优化上多了库存筛选、供应路径优化、跨媒体频控、Custom Bidding 四个 UAC 完全不存在的杠杆，同时对数据基建（Floodlight/BigQuery）和运维投入的要求也高得多。

## 六、2026 年平台变更速览（影响衡量与优化的关键节点）

| 时间 | 变更 | 对衡量/优化的影响 |
|------|------|-----------------|
| 2025 年 3 月起 | Optimized Targeting 不再支持 Fixed Bidding line item | 固定出价账户需切自动/自定义出价才能继续用受众扩展 |
| 2026 年 6 月 | Floodlight/Inventory 报表迁移至 Instant Reporting（分阶段） | 报表结构切换期需交叉验证数据口径 |
| 2026 年 6 月 15 日 | Audience Expansion 弃用，Lookalike 受众取代 | 需手动重新配置受众扩展设置，不会自动迁移 |
| 2026 年 8 月 12 日 | 创意轮播设置弃用，按出价目标自动匹配创意 | A/B 测试方法论需调整 |
| 2026 年 8 月 | 内容标签/类目排除弃用，Inventory Modes/Content Themes 取代 | 品牌安全策略必须完成映射，否则投放不受限制 |

## 参考资料

- Google Ads Help — Custom bidding overview：support.google.com/displayvideo/answer/9723477
- Google Ads Help — Using Floodlight data：support.google.com/displayvideo/answer/11969760
- PPC Land — "Google is overhauling DV360: 12 changes coming by August 2026"（2026-06-10）
- Improvado — "DV360 vs Google Ads 2026: Platform Comparison Guide"
- Fivestones — "Google Ads vs Display & Video 360 in 2026: What's the Difference?"
