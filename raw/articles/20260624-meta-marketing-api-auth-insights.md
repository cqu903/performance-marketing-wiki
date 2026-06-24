---
source_url: https://developers.facebook.com/docs/marketing-api/get-started/authorization
ingested: 2026-06-24
sha256: 59b0edad6b72579794e32333f6b4ae24a397ef6f6264a304f6200b339babbec7
---
# Meta Marketing API 授权、认证与 Insights 摘录

Source pages:
- https://developers.facebook.com/docs/marketing-api/get-started/authorization
- https://developers.facebook.com/docs/marketing-api/get-started/authentication
- https://developers.facebook.com/docs/marketing-api/insights

Authorization：授权流程用于验证访问 Marketing API 的 users 和 apps，并授予权限。App Dashboard 中可设置 Admin、Developer、Tester 等 app roles。根据用途，可能需要提交 App Review，以获得广告管理相关权限。

Marketing API Access Tier：
- 旧 Ads Management Standard Access 已调整为 Marketing API Access Tier；旧 “Standard Access” 现在是 Limited Access，旧 “Advanced Access” 现在是 Full Access。
- Limited access：添加 Marketing API product 后自动获得；按广告账号重度限流，主要用于开发，不适合面向 live advertisers 的生产应用；可创建 1 个 system user 和 1 个 admin system user；Business Manager / Catalog API 访问有限。
- Full access：通过 App Review 升级；按广告账号轻度限流；可管理无限 ad accounts，前提是取得 `ads_read` 或 `ads_management` 权限；可访问全部 Business Manager / Catalog APIs；可创建 10 个 system users 和 1 个 admin system user。
- Full access 要求：最近 15 天至少成功发起 500 次 Marketing API calls，且最近 500 次 calls 错误率低于 15%。
- 所有 access level 的调用都针对 production data，不存在类似 Google Ads 测试账号访问级别那样的隔离默认安全区。

权限：
- 只读广告报表：请求 `ads_read` + Marketing API Access Tier。
- 读取并管理自己或已授权广告账号：请求 `ads_management` + Marketing API Access Tier。
- 同时拉取部分客户报表、管理另一部分客户广告：请求 `ads_management` 和 `ads_read` + Marketing API Access Tier。
- 如果管理他人的广告账号，需要在 OAuth scope 中请求 `ads_management` 或 `ads_read`，由对方点击 Allow 授权。

Authentication：
- 每次 Marketing API 调用都需要 access token 参数。
- Graph API Explorer 可生成 user access token，并添加 `ads_read` / `ads_management` 等权限。
- Access Token Debugger 可查看 App ID、Expires、Scopes，并把短期 token 扩展为长期 token。
- System user access token 属于 Business Manager 中的 system user，适合无用户交互的 server-to-server 场景，可读写 business data、管理 ad campaigns 和其他广告对象。它不绑定个人，通常不会过期，比长期 user token 更适合长期脚本和服务。
- Token 应安全存储，通过 HTTPS 传输，最小化权限范围，并定期检查有效性；密码变更或权限撤销会导致 token 失效。

Ads Insights API：
- 提供 Meta ads 的 performance data 和 statistics，可自定义请求获取 Meta Ads Manager 中几乎任何 metric。
- 访问前需要 app 和 `ads_read` permission。
- Insights 是 ad account、campaign、ad set、ad 等广告对象的 edge。
- 默认 GET 通常返回过去 30 天的基础指标。
- 请求可通过 parameters、fields、breakdowns 细化，比如 `date_preset=last_7d`、`fields=clicks`、`breakdowns=gender`。
- 如果把 Ads Insights API 数据用于产品方案，需要遵守 Meta Platform Terms 和 Developer Policies for Marketing API。

页面更新时间：authorization 2026-05-05；authentication 2026-06-16；insights 2026-04-27。
