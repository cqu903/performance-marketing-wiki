---
title: Google Ads API
author: Hermes Agent
created: 2026-06-24
updated: 2026-06-24
type: concept
tags: [google-ads, integration, permissions, data-quality]
sources: [raw/articles/20260624-google-ads-api-get-started.md, raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md]
confidence: high
---

# Google Ads API

Google Ads API 是 Google Ads 的程序化接口，用于管理大型或复杂的 Google Ads 账号和广告系列。它适合需要自建软件、自动账号管理、自定义报告、产品目录驱动广告管理、智能出价策略管理的场景；如果只是轻量自动化或批量操作，Google 官方更建议先看 Google Ads Scripts、BigQuery Data Transfer、自动规则、批量上传或 Google Ads Editor。^[raw/articles/20260624-google-ads-api-get-started.md]

对 X Wallet 来说，Google Ads API 与 [[google-ads]] 的 AppsFlyer 对接不是一回事：AppsFlyer 对接解决归因、Link ID、转化导入和事件回传；Google Ads API 解决广告账号、广告系列、报告、预算、素材、转化资源等对象的程序化管理。两者最终会在 [[postback-and-event-mapping]]、[[agency-and-mcc-governance]] 和数据治理上汇合。

## 什么时候该用 Google Ads API

| 场景 | 适合度 | 说明 |
| --- | --- | --- |
| 自动拉取 campaign/ad group/keyword/asset/report 数据 | 高 | API 是标准路径，适合内部 BI、归因复盘、代理监督。 |
| 自动创建或调整广告系列、预算、出价策略 | 高 | 需要严格权限、审计和灰度；不要直接让 agent 无保护地改生产账号。 |
| 只想下载报表到 BigQuery | 中 | 官方建议优先看 BigQuery Data Transfer Service。 |
| 不想维护服务器和数据库 | 低 | Google Ads Scripts 更轻。 |
| MMM 数据抽取 | 低 | 官方建议 MMM Data Platform，而不是把 Google Ads API 当首选。 |

## 首次接入依赖

1. Google Ads 经理账号（MCC）：用于申请 developer token，也用于管理客户账号。
2. Developer token：22 位字母数字字符串，每次调用都要通过 `developer-token` header 发送。
3. API 访问权限级别：决定调用额度和是否能调生产账号；测试账号访问权限不能调用生产账号。
4. Google Cloud 项目：启用 Google Ads API，管理 OAuth 凭据。
5. OAuth 2.0 凭据：常见是服务账号 + JSON key，也可以按场景使用单用户或多用户 OAuth 工作流。
6. 目标 Google Ads Customer ID：10 位数字，API 调用时去掉连字符。
7. 目标账号权限：授权用户或服务账号必须被授予目标 Google Ads 账号访问权限。

## Developer token 决策

Google 通常每家公司授予一个 developer token。X Wallet 如果只是使用第三方工具，不需要自己申请 token；如果要构建内部投放自动化、报表抽取或 agent 操作系统，就应先确认公司是否已经有 token，优先复用。申请新 token 时，MCC 不能是测试经理账号，公司网站和 API 联系邮箱会影响审核，邮箱必须有人监控。^[raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md]

自动审核通过可能获得探索者访问权限，可以调生产账号但有限制；未自动通过可能只有测试账号访问权限，只能调测试账号。要解除限制，需要申请基本或标准访问权限。

## OAuth 工作流选择

| 使用方式 | 官方建议 | X Wallet 解读 |
| --- | --- | --- |
| 管理自己已有权限的广告账号 | 服务账号工作流；组织不允许时用单用户 OAuth | 内部自动化首选。服务账号权限应最小化，并记录在资产表。 |
| 已有 Google API OAuth 架构 | 复用现有 OAuth 架构，补充 Google Ads API scope 和账号权限 | 适合已有内部数据平台。 |
| SaaS / 多租户，代表其他用户管理账号 | 多用户身份验证，让用户登录并授权 | 只有对外产品化时才需要。 |

## 客户端库与调用方式

官方建议新手优先使用客户端库。官方支持 Java、C#/.NET、PHP、Python、Ruby、Perl；社区有 NodeJS `google-ads-api` 和 Go `google-ads-pb`，但 Google 不测试、不维护。v24 最低客户端库版本：Java 43.0.0，.NET 25.3.0，PHP 33.3.0，Python 30.1.0，Ruby 40.0.0，Perl 32.0.0。^[raw/articles/20260624-google-ads-api-oauth-token-client-libraries.md]

通用配置环境变量包括：`GOOGLE_ADS_CONFIGURATION_FILE_PATH`、`GOOGLE_ADS_JSON_KEY_FILE_PATH`、`GOOGLE_ADS_DEVELOPER_TOKEN`、`GOOGLE_ADS_LOGIN_CUSTOMER_ID`、`GOOGLE_ADS_LINKED_CUSTOMER_ID`。生产环境不要把这些凭据提交进仓库。

## 报告读取基本模式

官方首次调用示例用 `GoogleAdsService.SearchStream` 跑 GAQL：

`SELECT campaign.id, campaign.name FROM campaign ORDER BY campaign.id`

`SearchStream` 通常用于提取实体并返回行数据流；`Search` 适合网络不稳定场景，固定每页 10,000 行，客户端库会自动分页。后续如果要建设 X Wallet 投放数据管道，应先从只读报告开始，确认账号权限、MCC 层级、币种、时区、campaign 命名和 AppsFlyer 口径，再考虑写操作。

## X Wallet 落地顺序

1. 在 [[agency-and-mcc-governance]] 的资产表中补充 developer token owner、MCC、Customer ID、Cloud Project、OAuth/service account owner。
2. 先用只读权限跑 campaign/report 拉取，和 Google Ads UI、AppsFlyer 报表做抽样核对。
3. 把转化 action、AppsFlyer Link ID、事件映射关系整理进 [[postback-and-event-mapping]] 的验收清单。
4. 写操作必须分阶段上线：测试账号 → 单个低风险账号 → 小预算 campaign → 扩大范围。
5. 所有 agent 自动改预算、出价、素材、状态的动作都应有审计日志和人工确认阈值。

## 相关页面

- [[google-ads]]
- [[agency-and-mcc-governance]]
- [[postback-and-event-mapping]]
- [[data-discrepancy-playbook]]
