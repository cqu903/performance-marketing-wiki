# Wiki Index

> Content catalog. Every wiki page listed under its type with a one-line summary.
> Last updated: 2026-07-12 | Total pages: 38

## Entities
- [[af-smartscript]] — AppsFlyer H5 网页脚本：缓存 af_clickid 实现 iOS 无 IDFA 精准归因。
- [[aem]] — Meta Aggregated Event Measurement：iOS 14+ 聚合事件衡量方案与合格性要求。
- [[appsflyer]] — AppsFlyer 在归因、SDK 事件、渠道对接和数据治理中的角色。
- [[att]] — Apple AppTrackingTransparency：iOS 14.5+ IDFA 授权框架，iOS 隐私归因根因。
- [[google-ads]] — Google Ads SRN 对接、Link ID、转化导入、事件回传和 MCC 治理。
- [[idfa]] — Apple 广告标识符：点击/激活链路可用性、ATT 影响与无 IDFA 归因替代。
- [[meta-ads]] — Meta ads SRN 对接、事件映射、AEM/iOS 隐私、CAPI 服务器端上报和数据差异重点。
- [[meta-pixel]] — Meta 前端 JS 埋点：W2A 网页层浅层行为采集，与 CAPI 双轨上报。
- [[onelink]] — AppsFlyer 归因链接产品：点击触点上报、直跳商店 vs H5 链路差异。
- [[skadnetwork]] — Apple SKAN 隐私保护归因：聚合回传、延迟窗口、6bit 转化值与四层归因。
- [[uac]] — Google Universal App Campaign：自动化投放、分平台隔离、EVC 与量化分析工具。

## Concepts
- [[agency-and-mcc-governance]] — 代理、Google MCC、Link ID、成本连接和数据权限的治理原则。
- [[attribution-model]] — AppsFlyer 归因模型、确定性/概率归因、SRN 和再营销基础。
- [[bayesian-beta-smoothing]] — 贝叶斯 Beta 共轭先验：全 UAC 小样本转化率（EVC/CPI/ROAS）通用修正方法。
- [[creative-correlation-analysis]] — 皮尔逊+斯皮尔曼双相关系数：素材/出价/人群特征与转化的量化归因工具。
- [[enhanced-conversions-app]] — Google 增强型转化（SHA-256 第一方数据匹配）与混合变现（ILAR+IAP）数据工程。
- [[evc-engaged-view-conversion]] — Google UAC 2.5 互动浏览转化指标：定义、MMP 权责边界与数据观测口径。
- [[evc-optimization-playbook]] — EVC 分版位素材优化、tROAS 出价、归因窗口校准与避坑清单。
- [[golden-proxy-event]] — UAC 2.5 黄金代理事件筛选三步法：相关性→时效性→覆盖率，金融科技最佳实践。
- [[ios-w2a-vs-android-a2a]] — iOS W2A vs Android 原生 A2A：UAC 投放平台差异化策略论证。
- [[uac-algorithm-internals]] — UAC 算法内部解构：RankScore 公式、神经网络信号、冷启动贝叶斯解释、资产动态组合。
- [[uac-bidding-and-operations]] — UAC 三阶段出价迁移（Max→tCPA→tROAS）、预算/窗口配置、检查清单与故障排查。
- [[data-discrepancy-playbook]] — Google/Meta/AppsFlyer 数据差异排查顺序与常见原因。
- [[device-identifiers-and-privacy]] — IDFA、IDFV、GAID、OAID 等设备标识符与隐私限制。
- [[event-source-scope-and-learning]] — AppsFlyer 事件来源范围如何影响 Google/Meta 学习、客群匹配和 X Wallet 回传策略。
- [[google-ads-api]] — Google Ads API 的适用场景、developer token、OAuth、客户端库和 X Wallet 自动化落地边界。
- [[install-attribution-matching]] — 商店无参数时 AF 的点击-安装匹配机制、IDFA 在点击/激活链路的可用性、无 IDFA 四层归因匹配。
- [[ios-privacy-skan-aem]] — iOS ATT、SKAN、Meta AEM、Google iOS 隐私建模、CAPI 补充归因损耗对 AppsFlyer 归因和 X Wallet 应对措施的影响。
- [[lookback-window-strategy]] — Google/Meta 回溯窗口建议与 X Wallet 实操策略。
- [[meta-capi]] — Meta Conversions API 服务器端转化上报通道：Pixel+CAPI 双轨、EMQ 提升、信贷后端深度转化回传。
- [[meta-marketing-api]] — Meta Marketing API 的广告对象结构、权限、Access Tier、token、Insights 和 X Wallet 自动化边界。
- [[onelink-click-tracking]] — OneLink 点击上报时机、广告点击 vs H5 二次按钮区分、直跳商店 vs H5 落地页归因差异。
- [[postback-and-event-mapping]] — 关键事件回传、Google 转化导入、Meta 事件映射、后端事件 AF postback vs CAPI 原生上报和验收。
- [[w2a-data-flow]] — 信贷 W2A 标准化三层分流数据流：H5 网页层/App 层/风控后端，Meta CAPI 与 AF 双向并行。
- [[xwallet-event-taxonomy]] — X Wallet 贷款 App 推荐事件字典、回传优先级和后端事件双向同步规则。

## Comparisons
- [[af-vs-meta-capi]] — AppsFlyer 与 Meta CAPI 能力对比：为什么已有 AF 归因仍必须对接 CAPI，后端事件双向同步规则。
- [[google-vs-meta-integration]] — Google Ads 与 Meta ads 在 AppsFlyer 中的对接差异和配置重点。
- [[ios-w2a-vs-android-a2a]] — iOS W2A vs Android 原生 A2A：UAC 投放平台差异化策略。

## Queries
- [[xwallet-implementation-checklist]] — 面向 X Wallet 的 AppsFlyer + Meta CAPI 配置落地清单。
