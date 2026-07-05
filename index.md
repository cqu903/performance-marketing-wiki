# Wiki Index

> Content catalog. Every wiki page listed under its type with a one-line summary.
> Last updated: 2026-07-05 | Total pages: 21

## Entities
- [[appsflyer]] — AppsFlyer 在归因、SDK 事件、渠道对接和数据治理中的角色。
- [[google-ads]] — Google Ads SRN 对接、Link ID、转化导入、事件回传和 MCC 治理。
- [[meta-ads]] — Meta ads SRN 对接、事件映射、AEM/iOS 隐私、CAPI 服务器端上报和数据差异重点。

## Concepts
- [[agency-and-mcc-governance]] — 代理、Google MCC、Link ID、成本连接和数据权限的治理原则。
- [[attribution-model]] — AppsFlyer 归因模型、确定性/概率归因、SRN 和再营销基础。
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

## Queries
- [[xwallet-implementation-checklist]] — 面向 X Wallet 的 AppsFlyer + Meta CAPI 配置落地清单。
