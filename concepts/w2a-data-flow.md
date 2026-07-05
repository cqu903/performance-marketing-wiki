---
title: 信贷 W2A 标准化数据流
created: 2026-07-05
updated: 2026-07-05
type: concept
tags: [meta-ads, integration, event-mapping, xwallet, data-quality]
sources: [raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]
confidence: high
---

# 信贷 W2A 标准化数据流

信贷 W2A（Web to App）投放链路的标准化数据流设计。核心原则：每一层转化事件按用途分流上报，Meta CAPI 和 AppsFlyer 双向并行，不做二选一。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

投放链路：Meta 广告点击 → AF OneLink 归因链接 → 自有 H5 落地页 → 点击 CTA 下载按钮 → 应用商店 → App 下载激活。中间嵌套网页层，割裂广告-安装-后端转化全链路，极易出现归因断层和信号丢失。

## 三层数据流

### 第一层：H5 网页层

| 上报通道 | 事件 | 目的 |
|---|---|---|
| Meta Pixel | 页面浏览、CTA 按钮点击、简单填表 | 实时浅层行为，投喂 Meta 做人群触发和再营销 |
| Meta CAPI | 网页申请提交 | 确定性网页转化，弥补 Pixel 前端丢失 |
| AF Web-S2S | H5 行为/申请 | 补全 AF 网页层数据空白（App-SDK 原生无法采集 H5 行为） |

### 第二层：App 层

| 上报通道 | 事件 | 目的 |
|---|---|---|
| AF SDK | 激活（first_open）、注册 | 基础归因、安装转化、事件回传 |
| AF → Meta Postback | 激活、注册等 | 兜底回传 Meta，不作为主力优化信号通道 |

AF 向 Meta 的 postback 中转链路（业务→AF→Meta）存在两层转发延迟，队列易丢事件；且 AF 仅传递设备 ID，无法上传加密手机号、fbp、fbclid 等高精准标识，Meta 匹配分数 EMQ 偏低。iOS 关闭 IDFA 场景下归因匹配损耗极大。详见 [[af-vs-meta-capi]]。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

### 第三层：风控后端

| 上报通道 | 事件 | 目的 |
|---|---|---|
| Meta CAPI | 授信、放款 | 投喂高价值转化信号，矫正广告模型，降低 CPA；搭建分层再营销人群 |
| AF S2S | 授信、放款 | 绑定用户业务 ID，关联前置广告归因来源；核算各渠道真实放款 ROI 和单放款成本 |

后端事件双向同步是信贷重中之重。授信、放款数据仅存储在信贷风控后端，前端 H5 和 App 均拿不到。缺失任意一方上报都会引发业务问题：不报 Meta → 广告模型缺信号、CPA 升高；不报 AF → 总账 ROI 失真、无法甄别劣质渠道。^[raw/articles/20260705-meta-capi-w2a-credit-lending-review.md]

## 一句话原则

- 对接 Meta CAPI：为了投好广告、降成本。
- 同步上报 AF：为了算清账目、评估渠道。
- 两者不可替代；W2A 信贷投放，缺一数据失真、投放亏损。

## iOS 无 IDFA 场景的补充

W2A 链路 iOS 归因兜底方案：H5 部署 AF 网页脚本缓存 af_clickid 做精准归因（参见 [[install-attribution-matching]] 第二层匹配）+ Meta CAPI 补充用户特征信号（加密手机号、fbclid/fbp）提升 EMQ。双向弥补 SKAN 精度不足和 IDFA 关闭导致的大规模归因丢失。详见 [[ios-privacy-skan-aem]]。

## 相关页面
- [[meta-capi]]
- [[af-vs-meta-capi]]
- [[postback-and-event-mapping]]
- [[xwallet-event-taxonomy]]
- [[onelink-click-tracking]]
- [[ios-privacy-skan-aem]]
