---
source_url: https://developers.google.com/google-ads/api/docs/get-started/introduction?hl=zh-cn
ingested: 2026-06-24
sha256: b7ab97e4a0b25807cf8884f39000edc69b928c0a97977ddb16f1c158b96f35bd
---
# Google Ads API 简介与快速入门摘录

Source pages:
- https://developers.google.com/google-ads/api/docs/get-started/introduction?hl=zh-cn
- https://developers.google.com/google-ads/api/docs/get-started/make-first-call?hl=zh-cn

Google Ads API 是 Google Ads 的程序化接口，适合管理大型或复杂的 Google Ads 账号和广告系列。典型用途包括自动账号管理、自定义报告、基于产品目录的广告管理、智能出价策略管理。

Google 官方给出的选型边界：
- 开发者要构建自己的软件产品或深度集成，且能管理服务器、数据库等软件基础设施：使用 Google Ads API。
- 能写代码但不想维护基础设施，或非开发者只想自动执行 Google Ads 操作：使用 Google Ads Scripts。
- 数据分析师只想下载 Google Ads 报告进一步分析：使用 BigQuery Data Transfer Service 的 Google Ads 转移。
- MMM 开发：官方建议使用 MMM Data Platform，而不是把 Google Ads API 当成 MMM 数据抽取的理想方案。
- 不想写代码做批量管理：使用自动规则、批量上传或 Google Ads Editor。

首次调用涉及的核心概念：
- 开发者令牌 developer token：22 位字母数字字符串，用于向 Google Ads API 标识应用，每次 API 调用都需要。
- API 访问权限级别：决定每天可调用次数以及可调用生产环境还是测试环境。
- Google Ads 经理账号 MCC：用于管理客户账号，也用于获取开发者令牌。
- Google Ads 客户账号 customer account：实际被 API 操作的广告账号；Customer ID 是 10 位数字，调用时去掉连字符。
- OAuth 2.0：所有 Google API 使用的授权协议；Google Ads API 调用需要 OAuth 凭据。
- Google Cloud 项目：启用 Google Ads API、管理 OAuth 2.0 凭据的基础。
- 服务账号与 JSON 密钥：服务账号属于应用而非个人用户；JSON key 用于生成 OAuth 2.0 凭据。

官方快速入门路径：
1. 在 Google Ads 经理账号的 API Center 申请或查找 developer token。
2. 在 Google Cloud 项目中启用 Google Ads API。
3. 创建服务账号和 JSON key；保存为 credentials.json 或客户端库配置指向的路径。
4. 确认要调用的 Google Ads 客户账号，以及 developer token 的访问权限级别。
5. 如果 token 只有测试账号访问权限，只能调用 Google Ads 测试账号，不能调用生产账号。
6. 给服务账号授予目标 Google Ads 账号访问权限；需要客户账号管理员权限。
7. 下载客户端库或使用 REST/curl 发起首次调用。

官方首次调用示例主要读取 campaign：通过 GoogleAdsService.SearchStream 运行 GAQL 查询 `SELECT campaign.id, campaign.name FROM campaign ORDER BY campaign.id`。

最后更新时间：introduction 页面 2026-05-17；quickstart 页面 2026-06-17。
