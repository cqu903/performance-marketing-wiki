---
source_url: https://developers.google.com/google-ads/api/docs/oauth/overview?hl=zh-cn
ingested: 2026-06-24
sha256: 1333020f25594b27cc0491efb89bacd628ae49a0313b3cc7fef520a4838ff2e7
---
# Google Ads API OAuth、developer token 与客户端库摘录

Source pages:
- https://developers.google.com/google-ads/api/docs/oauth/overview?hl=zh-cn
- https://developers.google.com/google-ads/api/docs/api-policy/developer-token?hl=zh-cn
- https://developers.google.com/google-ads/api/docs/client-libs?hl=zh-cn

OAuth 2.0：Google Ads API 使用 OAuth 2.0 做身份验证与授权，使客户端应用能访问用户的 Google Ads 账号，而无需处理或存储用户登录信息。

官方 OAuth 场景建议：
- 已有 Google API OAuth 工作流，只是新增 Google Ads API：复用现有 OAuth 架构，但要确认授权用户或服务账号有目标 Google Ads 账号权限。
- 管理自己已有权限的广告账号，未来新账号通过 MCC 关联或邀请获得权限：优先使用服务账号工作流；如果组织政策不允许服务账号，再用单用户身份验证。
- 构建 SaaS/多租户应用，代表其他用户管理其 Google Ads 账号：应使用多用户身份验证，让用户登录并授权应用访问其账号。

Developer token：
- 每次 API 调用都必须通过 `developer-token` HTTP/gRPC header 发送。
- Google 通常每家公司授予一个 developer token；公司已用过 Google Ads API 时应优先复用现有 token。
- 如果使用第三方工具，通常不需要自己申请 developer token；该工具开发方负责。
- 申请 token 需要 Google Ads 经理账号，且不能是测试经理账号。
- 申请资料中的公司网站和 API 联系邮箱会影响审核；邮箱必须有人监控。
- 自动审核通过时可能获得探索者访问权限，可调用生产账号但有限制；未自动通过时可能只有测试账号访问权限。
- 若要解除探索者/测试限制，需要申请基本或标准访问权限。

客户端库：
- 官方建议 API 新手优先从客户端库开始。
- 官方支持 Java、C#/.NET、PHP、Python、Ruby、Perl；社区库包括 NodeJS `google-ads-api` 和 Go `google-ads-pb`，但官方不测试、不维护。
- v24 最低客户端库版本：Java 43.0.0，.NET 25.3.0，PHP 33.3.0，Python 30.1.0，Ruby 40.0.0，Perl 32.0.0。

通用环境变量：
- `GOOGLE_ADS_CONFIGURATION_FILE_PATH`：配置文件路径。
- OAuth 应用模式：`GOOGLE_ADS_CLIENT_ID`、`GOOGLE_ADS_CLIENT_SECRET`、`GOOGLE_ADS_REFRESH_TOKEN`。
- 服务账号模式：`GOOGLE_ADS_JSON_KEY_FILE_PATH`、`GOOGLE_ADS_IMPERSONATED_EMAIL`。
- Google Ads API：`GOOGLE_ADS_DEVELOPER_TOKEN`、`GOOGLE_ADS_LOGIN_CUSTOMER_ID`、`GOOGLE_ADS_LINKED_CUSTOMER_ID`。

SearchStream 与 Search：
- `GoogleAdsService.SearchStream` 通常用于提取实体，返回行数据流。
- `GoogleAdsService.Search` 适合不稳定网络连接，固定每页 10,000 行，客户端库会自动分页。

最后更新时间：OAuth overview 2026-05-13；developer token 2026-05-13；client libraries 2026-06-17。
