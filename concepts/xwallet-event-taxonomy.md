---
title: X Wallet 事件字典建议
created: 2026-06-24
updated: 2026-07-05
type: concept
tags: [xwallet, event-mapping, postback, data-quality, acquisition]
sources: [raw/articles/115002504686-google-ads-integration.md, raw/articles/4410480904081-meta-ads-in-app-event-mapping.md, raw/articles/207447163-link-structure-and-parameters.md, raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md, raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]
confidence: medium
---

# X Wallet 事件字典建议

这是基于贷款 App 投放优化的建议事件层级，不是 AppsFlyer 官方事件表。正式落地前要和产品、后端、合规、广告账户优化目标一起确认。

## 推荐事件漏斗
| 阶段 | 建议事件名 | 用途 | 是否建议回传 Google/Meta |
|---|---|---|---|
| 安装/打开 | first_open / af_app_opened | 基础归因、再互动 | 必须 |
| 注册完成 | af_complete_registration | 早期优化信号 | 建议 |
| 申请开始 | xw_loan_apply_start | 区分低意向注册 | 建议 |
| 资料提交 | xw_loan_submit | 核心质量事件 | 强烈建议 |
| KYC 完成 | xw_kyc_completed | 合规/身份质量 | 视量级建议 |
| 审批通过 | xw_loan_approved | 高质量事件 | 量够则建议 |
| 放款成功 | xw_disbursement_success | 终局价值 | 建议用于价值分析，不一定作为唯一优化目标 |
| 还款成功 | xw_repayment_success | 长周期质量 | 更适合 BI/LTV，不一定回传投放平台 |

## 事件来源范围建议
| 事件层级 | Google Ads | Meta Ads | 说明 |
|---|---|---|---|
| first_open / install | 必须导入/回传 | 必须回传 | 基础归因和安装转化可见性 |
| app_open / session_start | 做再营销时回传 | 做再营销时回传 | 再互动、受众和衡量 |
| 注册完成 | 所有媒体源，包括自然量 | 所有媒体源，包括自然量 | 高频、低敏、适合早期学习 |
| 申请开始 | 所有媒体源，包括自然量 | 所有媒体源，包括自然量 | 中层意向信号，帮助模型学习 |
| 申请提交 | 初期所有媒体源，稳定后评估是否收紧 | 初期所有媒体源，尤其 iOS/AEM 阶段；稳定后评估 | 核心质量事件，需监控后续质量 |
| 审批通过 / 授信成功 | 谨慎测试；可考虑仅本渠道或不带金额 | 谨慎测试；可考虑仅本渠道或不带金额 | 高质量但低频、敏感 |
| 放款成功 / 金额 | 不默认全媒体源全量推 | 不默认全媒体源全量推 | 终局价值事件，涉及敏感商业和合规边界 |

详见 [[event-source-scope-and-learning]]。^[raw/articles/20260624-xwallet-af-postback-source-scope-discussion.md]

## 参数建议
- 金额：申请金额、获批额度、放款金额（注意隐私与平台政策）。
- 产品类型：短期/分期、期限、利率档位（避免传个人敏感信息）。
- 风险分层：只传粗粒度标签，不传可识别个人身份的信息。
- 币种：HKD。

## 回传策略
- 初期以注册完成 + 申请提交作为主要优化信号，保证事件量。
- 稳定后测试审批通过/放款成功作为价值信号，但不要因量太低让算法失去学习能力。
- Google / Meta 的事件命名和 AppsFlyer 内部事件要维护映射表，防止代理随意改名。

## 后端事件上报规则

信贷核心后置转化（提交申请、风控审核、授信、放款、还款）必须双向同步上报 Meta CAPI 和 AF S2S，严禁二选一。报 Meta CAPI 服务投放优化（矫正模型、降 CPA、分层再营销、提升 EMQ）；报 AF S2S 服务业务总账（绑定业务 ID、核算放款 ROI、财务对账、反作弊）。详见 [[w2a-data-flow]] 和 [[af-vs-meta-capi]]。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

## 相关页面
- [[postback-and-event-mapping]]
- [[event-source-scope-and-learning]]
- [[xwallet-implementation-checklist]]
- [[data-discrepancy-playbook]]
- [[meta-capi]]
- [[w2a-data-flow]]
- [[af-vs-meta-capi]]
