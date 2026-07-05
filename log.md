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
