---
title: AppsFlyer SmartScript
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [appsflyer, attribution, integration]
sources: [raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]
confidence: high
---

# AppsFlyer SmartScript

AF SmartScript 是 [[appsflyer]] 提供的 H5 网页脚本，部署在 W2A 链路的落地页上。它的核心作用是读取 [[onelink|OneLink]] URL 中的归因参数（特别是 af_clickid），缓存到 localStorage，供 App 首开后 SDK 读取回传，完成精准归因匹配。

## 解决的问题

ATT 拒绝追踪后，IDFA 全零无效，传统设备 ID 精准匹配失效。AF SmartScript 缓存的 af_clickid 是 ATT 拒绝场景下**唯一不依赖 IDFA 的精准归因路径**（四层归因中的第二层）。详见 [[install-attribution-matching]]。

## 工作流程

1. 用户点击广告 → OneLink 生成全局唯一 af_clickid。
2. AF 重定向至 H5 页面，af_clickid 拼接在 URL 参数中。
3. SmartScript 读取 URL 中的 af_clickid，存入浏览器 localStorage。
4. 用户在 H5 点击下载按钮跳转商店（商店 URL 干净，不携带归因参数）。
5. App 安装后首开，AF SDK 读取 localStorage 中缓存的 af_clickid 随 install 上报。
6. AF 后台通过 af_clickid 精准命中对应点击记录，准确率 100%，不受 IDFA 影响。

## 可选能力：动态生成新 OneLink

SmartScript 还可选动态生成新 OneLink 跳转商店，这会在广告点击之外新增第二条 H5 按钮点击触点（参见 [[onelink-click-tracking]] 场景 2）。末次点击规则下，H5 按钮触点时间更晚会抢占归因。

## 失效场景

- 纯浏览器打开链接（短信外链、Safari 直接打开），无 H5 SmartScript 部署。
- H5 未正确部署或 SmartScript 缓存失败。
- 用户清除浏览器缓存后 App 才首开。

## SmartScript 不能获取 IDFA

即便部署 SmartScript，H5 网页依旧无法获取本机 IDFA——Safari/H5/浏览器 JS 完全没有系统权限读取 IDFA。SmartScript 仅能读取 URL 渠道参数和缓存 af_clickid。

## X Wallet 操作要点

1. iOS 投放强烈建议 H5 落地页部署 AF SmartScript，启用第二层 af_clickid 精准匹配。
2. 需要开发验证 SmartScript 正确缓存 af_clickid、SDK 正确读取回传——这是 iOS 归因准确度的隐藏依赖，常被忽略。
3. 对香港 iOS 流量尤其关键（ATT opt-in 率偏低的环境）。

## 相关页面
- [[install-attribution-matching]] — SmartScript 支撑的第二层 af_clickid 精准匹配。
- [[onelink]] — SmartScript 读取的 OneLink 归因链接。
- [[onelink-click-tracking]] — SmartScript 动态生成 OneLink 产生的双触点场景。
- [[ios-privacy-skan-aem]] — SmartScript 作为 iOS 无 IDFA 归因兜底方案之一。
- [[w2a-data-flow]] — SmartScript 在 W2A 三层数据流第一层的角色。
