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
