# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete

## [2026-06-24] create | AppsFlyer attribution wiki initialized
- Domain: AppsFlyer attribution, Google Ads / Meta ads event postbacks, X Wallet implementation.
- Created schema, index, log, raw source archive, entity/concept/comparison/query pages.

## [2026-06-24] ingest | AppsFlyer Help Center section 6551113099153 plus Google Ads and Meta ads integration docs
- Source section: https://support.appsflyer.com/hc/zh-cn/sections/6551113099153
- Added related sections for user goal: Meta ads and Google Ads AppsFlyer integrations.
- Raw articles captured: 44 files under raw/articles/.
- Wiki pages created: 13.
- Key synthesis pages: [[xwallet-implementation-checklist]], [[postback-and-event-mapping]], [[lookback-window-strategy]], [[data-discrepancy-playbook]].

## [2026-06-24] ingest | X Wallet AppsFlyer event source scope discussion
- Raw source created: raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md
- New concept page created: [[event-source-scope-and-learning]]
- Updated pages: [[postback-and-event-mapping]], [[xwallet-event-taxonomy]], [[ios-privacy-skan-aem]], index.md
- Extracted decision: selected mid-funnel events can use all media sources during cold start; deep sensitive events should be governed separately and may be narrowed to this-partner-only.

## [2026-06-24] update | iOS privacy, SKAN, ATT, AEM and ad platform impact
- Expanded [[ios-privacy-skan-aem]] with ATT, SKAN, Meta AEM, Google iOS attribution limits, AppsFlyer reporting impact, platform optimization impact, and X Wallet operating guidance.
- Updated pages: [[device-identifiers-and-privacy]], [[meta-ads]], [[google-ads]], [[data-discrepancy-playbook]], [[postback-and-event-mapping]], [[xwallet-implementation-checklist]], [[google-vs-meta-integration]], index.md.
- Repo visibility changed to public: https://github.com/cqu903/appsflyer-attribution-wiki

## [2026-06-24] ingest | Google Ads API get started documentation
- Source pages: Google Ads API introduction, quickstart, OAuth overview, developer token, client libraries.
- Raw articles created: raw/articles/20260624-google-ads-api-get-started.md, raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md.
- New concept page created: [[google-ads-api]].
- Updated pages: [[google-ads]], [[agency-and-mcc-governance]], index.md.
- Key synthesis: Google Ads API is the programmatic account/reporting/operation layer, distinct from AppsFlyer Google Ads integration as attribution/event postback layer.

## [2026-06-24] ingest | Meta Marketing API documentation
- Source pages: Marketing API overview/get started, authorization, authentication, Ads Insights API.
- Raw articles created: raw/articles/20260624-meta-marketing-api-overview-get-started.md, raw/articles/20260624-meta-marketing-api-auth-insights.md.
- New concept page created: [[meta-marketing-api]].
- Updated pages: [[meta-ads]], [[agency-and-mcc-governance]], index.md.
- Key synthesis: Meta Marketing API is the programmatic ad account/insights/operation layer, distinct from AppsFlyer Meta integration as attribution/event postback/AEM/SKAN layer.

## [2026-07-05] ingest | AppsFlyer SDK、OneLink、iOS 无 IDFA 归因全流程笔记
- Raw source: raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md
- New concept pages: [[onelink-click-tracking]], [[install-attribution-matching]]
- Updated pages: [[attribution-model]], [[ios-privacy-skan-aem]], [[device-identifiers-and-privacy]], index.md
- Key synthesis: OneLink 点击上报时机（广告点击 vs H5 二次按钮触点区分）；直跳商店 vs H5 落地页归因差异；商店不参与归因线索存储（全靠点击触点 + SDK 上报双向匹配）；点击阶段网页无权读取 IDFA（仅媒体 App 可传入）；ATT 拒绝后 AF 四层归因（SKAN → af_clickid 精准匹配 → IDFV → 概率指纹），af_clickid 匹配是唯一不依赖 IDFA 的精准归因路径。

## [2026-07-05] ingest | Meta CAPI + AppsFlyer 归因业务复盘（信贷 W2A 专项）
- Raw source: raw/articles/20260705-meta-capi-w2a-credit-lending-review.md
- New concept pages: [[meta-capi]], [[w2a-data-flow]]
- New comparison page: [[af-vs-meta-capi]]
- Updated pages: [[meta-ads]], [[postback-and-event-mapping]], [[xwallet-event-taxonomy]], [[event-source-scope-and-learning]], [[ios-privacy-skan-aem]], [[xwallet-implementation-checklist]], index.md
- Key synthesis: 已有 AF 归因仍必须对接 Meta CAPI——AF 是全局归因总账/渠道治理/反作弊，CAPI 是广告投放优化/EMQ 提升/深层转化回传，二者不可替代；信贷 W2A 标准化三层分流（H5 网页层 Pixel+CAPI+AF Web-S2S → App 层 AF SDK+postback 兜底 → 风控后端 CAPI+AF S2S 双向同步）；后端事件（授信/放款）严禁二选一上报；CAPI 携带加密手机号/fbclid/fbp 多维信号提升 EMQ 是 AF postback 无法实现的能力；iOS 无 IDFA 场景需 af_clickid 精准匹配 + CAPI 用户特征信号双向弥补。

## [2026-07-12] ingest | UAC 全系投流量化分析实操手册
- Raw source: raw/articles/20260712-uac-quantitative-analysis-handbook.md（本地文件摄入，sha256 校验）
- New concept pages: [[evc-engaged-view-conversion]], [[creative-correlation-analysis]], [[bayesian-beta-smoothing]]
- Updated pages: [[google-ads]]（追加 3 个相关页面反向链接），index.md（21→24 pages）
- Key synthesis: EVC 是 UAC 2.5 优质流量信号，MMP 负责跨渠道口径统一/LTV 分层/反作弊，但不负责 10s 触发阈值（Google 原生规则）；皮尔逊（线性）+斯皮尔曼（单调非线性）双相关系数可量化素材/出价/人群特征对转化的驱动规律，区分「高流量垃圾素材」与「低流量优质素材」；贝叶斯 Beta 共轭先验（后验期望 α+k/(α+β+n)）是全 UAC 小样本转化率通用修正工具，转化样本 <100 时禁用原始指标；两套方法联动构成「大样本挖特征 / 小样本平滑评级」的双模型工作流，并可优化 RankScore 竞价的质量分与出价精度。

## [2026-07-12] ingest | EVC 观测、优化实操与 MMP 归属解答
- Raw source: raw/articles/20260712-evc-observation-optimization-mmp-scope.md（对话整理稿，sha256 校验）
- New concept page: [[evc-optimization-playbook]]
- Updated pages: [[evc-engaged-view-conversion]]（补三层观测操作细节、EVC vs VT 不可合并、MMP 四项职责 + 两项不属于），index.md（24→25 pages）
- Key synthesis: EVC 观测分三层（Google Ads 原生看板拉 EVC 转化/CPA/ROAS → MMP 三类归因分流校验 + LTV 分层 → BI 二次建模）；素材优化按版位拆——Shorts 强前 3 秒 + 降 CTA 弹窗撑满 10s，In-Stream 第 7~9 秒剧情拐点拉留存，AdMob 激励视频基本无 EVC 不纳入；tROAS 必须并入 EVC 营收；EVC vs 普通 VT 不可合并复盘；归因窗口安装 30 天 / 深度付费 60 天，不可盲目压缩；MMP 权责「部分纳入」——跨渠道口径/分层/反作弊/回传归 MMP，10s 触发阈值与 YouTube 原始埋点归 Google 原生。

## [2026-07-12] create | 从 concepts 实体抽取：8 个高引用实体建 entity 页
- 扫描全部 19 个 concept 页 + 3 个已有 entity 页 + 1 个 comparison 页，提取被多次引用但未独立建页的命名实体。
- 按 centrality（被引用页数）排序，优先创建 8 个 entity 页：
  - [[skadnetwork]] — SKAdNetwork/SKAN（9 页引用，最高频）
  - [[idfa]] — Apple 广告标识符（6 页引用）
  - [[att]] — AppTrackingTransparency（5 页引用）
  - [[aem]] — Aggregated Event Measurement（5 页引用）
  - [[uac]] — Universal App Campaign（5 页引用）
  - [[onelink]] — AppsFlyer 归因链接产品（5 页引用）
  - [[meta-pixel]] — Meta 前端 JS 埋点（3+ 页引用）
  - [[af-smartscript]] — AppsFlyer H5 网页脚本（3 页引用）
- 更新 index.md（26→34 pages），entity 区块从 3 条扩展到 11 条。
- 每个新 entity 页均带 frontmatter、出站 wikilinks（≥2），与已有 concept 页双向交叉引用。

## [2026-07-12] ingest | iOS W2A vs Android 原生 A2A UAC 投放策略
- Raw source: raw/articles/20260712-ios-w2a-vs-android-a2a-uac-strategy.md（对话整理稿，sha256 校验）
- New comparison page: [[ios-w2a-vs-android-a2a]]
- Updated pages: [[w2a-data-flow]]、[[ios-privacy-skan-aem]]（追加反向链接），index.md（25→26 pages）
- Key synthesis: SKAN 6bit + ATT 限制导致 iOS 原生 A2A 信号残缺（只能 D0、24-48h 窗口、IDFA 缺失），W2A 用 H5 前置转化信号（下载按钮点击、Web 注册）做对冲；Android 无隐私限制，原生 A2A 归因完整、tROAS 稳定，W2A 反而增损耗；UAC Campaign 强制分平台天然支持两套独立策略（iOS W2A + 浅层优化目标 / Android A2A + 深度事件 tROAS）；边界：iOS D0 付费产品（超休闲）可原生 A2A，安卓品牌种草可 W2A，其余按主策略。

## [2026-07-13] ingest | Deep Link / Deferred Deep Link 技术限制排查解析
- Raw source: raw/articles/20260713-deeplink-deferred-deeplink-technical-limitations.md（本地文件摄入，sha256: 42095ea2…）
- New concept page: [[deeplink-deferred-deeplink-limitations]]
- Updated pages: [[install-attribution-matching]]、[[lookback-window-strategy]]、[[onelink]]（追加反向链接），index.md（38→39 pages）
- Key synthesis: DDL 三项核心技术限制——① 热更新（OTA）在首次启动时打断"参数获取→路由跳转"串行链路，实例重建销毁已请求参数（React Native `Linking.getInitialURL()` 返回 null 为典型症状）；② 点击→首次启动间隔超过 15 分钟归因窗口，服务端主动销毁会话参数（DDL 概率匹配准确性随间隔指数下降）；③ 点击与首次启动 IP 不一致导致指纹哈希不匹配（4G/Wi-Fi 切换、VPN、共享 Wi-Fi、iOS Private Relay 均可触发）。三层缓解方案：确定性参数存储（服务端存参数+确定性 ID 取回）、多标识匹配（优先 Install Referrer/iOS Pasteboard 降 IP+UA 权重）、客户端本地补偿（参数加密存本地热更新后重读）。关键区分：DDL 15 分钟窗口 ≠ MMP 归因回溯窗口（Google 30 天/Meta 7 天），前者决定参数能否还原，后者决定触点能否归因。

## [2026-07-13] query | Deferred Deep Link 技术限制查询
- 用户问询 DDL 技术限制，已由专题页 [[deeplink-deferred-deeplink-limitations]] 完整覆盖，未新建 query 页（无新增综合）。

## [2026-07-19] update | Wiki 重命名：appsflyer-attribution-wiki → performance-marketing-wiki
- 动因：wiki 实际范围早已超出 AppsFlyer 归因，覆盖完整效果营销（投放平台/代理/衡量/优化/政策），改名匹配实际领域。
- GitHub repo 重命名：cqu903/appsflyer-attribution-wiki → cqu903/performance-marketing-wiki（gh repo rename）。
- 本地目录：~/Documents/appsflyer-attribution-wiki → ~/Documents/performance-marketing-wiki。
- git remote set-url origin 已更新。
- 删除独立的 ~/Documents/ad-ops-wiki（仅有 1 份代理体系散文件，已并入本 wiki）。
- SCHEMA.md 更新：Domain 改为"效果营销"，Tag Taxonomy 扩展（新增 dv360/tiktok/youtube/programmatic/bidding/targeting/creative/frequency-capping/brand-safety/spo/pmp/lift-testing/view-through/agency-tier/rebate/policy-change 等）。

## [2026-07-19] ingest | DV360 与 Google Ads 对比、DV360 衡量与优化框架
- 来源：用户上传 docx（~/Downloads/DV360与GoogleAds对比及衡量优化框架.docx）
- Raw source: raw/articles/dv360-vs-google-ads-framework-2026.md（sha256: 33e4f54f…）
- New wiki pages (3):
  - [[dv360]] (entity) — DV360 平台实体
  - [[dv360-vs-google-ads]] (comparison) — 全维度对比 + X Wallet 适用性结论
  - [[dv360-optimization-framework]] (concept) — Floodlight 衡量体系、六类优化杠杆、2026 政策变更速览
- Key synthesis: DV360 是 DSP、Google Ads 是效果引擎，互补非竞品；DV360 4 个独有杠杆（库存筛选/SPO/跨媒体频控/Custom Bidding）+ 3 层衡量（可见度/触达/增量）；post-view 不能忽视（只看 last-click 系统性低估 DV360）；X Wallet 月预算 HKD 50 万以 App 效果为主，DV360 开户门槛 ≥2.5 万美元/月+10-15% 平台费，短期性价比不高，除非进入品牌建设/CTV/DOOH 阶段。
- 2026 硬性变更：Optimized Targeting 不再支持 Fixed Bidding（2025-03）、Floodlight 报表迁移 Instant Reporting（2026-06）、Audience Expansion→Lookalike（2026-06-15）、创意轮播弃用（2026-08-12）、Content Labels→Inventory Modes（2026-08，未完成映射将 unrestricted serving）。

## [2026-07-19] ingest | 效果广告代理体系：中国分级 vs 海外官方认证（从 ad-ops-wiki 迁入）
- 来源：原 ~/Documents/ad-ops-wiki/agency-tier-system.md（该目录已删除）
- Raw source: raw/articles/agency-tier-system-raw.md（sha256: 5370e875…）
- New wiki page: [[agency-tier-system]] (concept) — 中国一/二/三级代理 vs Google/Meta/TikTok 官方合作伙伴体系、返点机制、X Wallet 通过代理 vs 直投取舍。
- 与 [[agency-and-mcc-governance]] 互补：前者商业关系层级，后者账户权限治理。
- index.md（39→43 pages）
