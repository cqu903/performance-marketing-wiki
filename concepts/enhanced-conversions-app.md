---
title: 增强型转化与混合变现数据工程
created: 2026-07-12
updated: 2026-07-12
type: concept
tags: [google-ads, data-engineering, privacy, acquisition]
sources: [raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]
confidence: high
---

# 增强型转化与混合变现数据工程

UAC 2.5 对数据准确性、实时性和隐私合规要求极高。随着 iOS IDFA 失效和 Android 隐私沙盒推进，丢失设备 ID 已成定局，Google 全面转向转化建模（Conversion Modeling）。本文记录两个关键技术基础设施。^[raw/articles/20260712-uac-2.5-deep-technical-strategic-whitepaper.md]

## 增强型转化（Enhanced Conversions）

### 原理
当用户在应用内触发 UAC 2.5 事件时：
1. App 捕获用户第一方数据（邮箱、手机号）
2. 本地 SHA-256 哈希加密
3. 通过 Google Ads API 或 Firebase SDK 发送哈希值给 Google
4. Google 与登录 Google 账号（YouTube、Gmail、Search）的用户哈希匹配
5. 找回因 Cookie 缺失或设备 ID 限制而丢失的转化

### 效果
据 Google 内部数据，平均提升 10%-15% 的转化率，直接改善 UAC 2.5 的 ROAS 表现。

Google 官方文档（[9888656](https://support.google.com/google-ads/answer/9888656)）区分两种类型：
- **Enhanced Conversions for Web**：追踪网站上的转化（如 W2A 落地页行为）
- **Enhanced Conversions for Leads**：追踪网站线索产生的离线交易（如 Web 注册后的 App 内付费）

### X Wallet 信贷场景适配
- W2A 链路中 H5 落地页采集用户手机号/邮箱 → SHA-256 哈希 → Enhanced Conversions 补全 iOS 黑盒归因
- 与 [[meta-capi]] Meta CAPI 的用户特征信号补充逻辑完全一致——CAPI 补 Meta 侧，Enhanced Conversions 补 Google 侧
- 与 [[ios-privacy-skan-aem]] 中 CAPI 补充 iOS 归因损耗的思路互补

## 混合变现（Hybrid Monetization）数据流

对于依赖"IAP（内购）+ 广告"双重变现的应用（如休闲游戏），UAC 2.5 必须具备全局价值视角。

### 传统误区
只回传 IAP 金额给 Google → 算法认为"不付费但看广告"的用户价值为 0 → 停止购买这类用户。

### 最佳实践架构

```
广告变现平台（AdMob/Max/IronSource）
    ↓ 开启 ILAR（Impression-Level Ad Revenue）
    ↓ 每个广告展示打精确美元价值标签
归因/分析层（Firebase/MMP）
    ↓ 接收 ILAR 数据，聚合到 User ID 级别
    ↓ UserValue = Σ IAP + Σ AdRevenue
Google Ads 回传
    ↓ 方案 A（推荐）：Firebase 关联 Google Ads → 导入 tROAS(Hybrid) 转化目标
    ↓ 方案 B：MMP 聚合 Total Revenue 回传 Google Ads
```

通过这种方式，UAC 2.5 既可优化付费行为，也可优化高频看广告行为，最大化总收益。

### ILAR 参考
- Android: [developers.google.com/admob/android/impression-level-ad-revenue](https://developers.google.com/admob/android/impression-level-ad-revenue)
- Unity: [developers.google.com/admob/unity/impression-level-ad-revenue](https://developers.google.com/admob/unity/impression-level-ad-revenue)
- Firebase 混合变现教程: [firebase.google.com/docs/tutorials/optimize-hybrid-monetization](https://firebase.google.com/docs/tutorials/optimize-hybrid-monetization)

### X Wallet 适用性
X Wallet 是纯 IAP 模式（贷款利息收入），无广告变现，混合变现数据流**暂不适用**。但如果未来 App 内加入广告展示（如金融资讯 Feed 中的广告位），此架构可复用。

## MMP 与 Google 归因博弈

### 归因逻辑差异
- **MMP 逻辑**：Last-Click 归因（最后一次点击渠道获得归因）
- **Google 逻辑**：Data-Driven Attribution (DDA) + Self-Attribution Network (SAN)，只要用户在 Google 生态有过交互就向 MMP 申领归因

### UAC 2.5 实战原则
不要试图在财务对账层面强行抹平差异：
- **财务结算**：以 MMP 数据为准
- **算法优化**：必须完全信任并回传 Google 自身的数据

关键操作：MMP 后台 Postback 配置选择 **"All Media Sources"**（回传所有转化）发送给 Google Ads。Google 自行过滤未参与的转化，但这样最大化其数据视野——否则会丢失 30%-40% 的辅助转化（Assisted Conversions）和浏览转化（View-Through Conversions），人为造成数据稀疏。

这与 [[postback-and-event-mapping]] 和 [[event-source-scope-and-learning]] 中"冷启动阶段回传所有媒体源"的原则一致，但本页强调的动机不同：这里是为了给 Google 算法提供最大数据视野，而非为了 Meta AEM 合格性。

## 相关页面
- [[golden-proxy-event]] — Enhanced Conversions 服务于代理事件的转化建模。
- [[ios-privacy-skan-aem]] — IDFA 失效后 Google iOS 的建模转化与 gbraid。
- [[meta-capi]] — Meta CAPI 与 Google Enhanced Conversions 是对等的第一方数据补充方案。
- [[postback-and-event-mapping]] — MMP Postback "All Media Sources" 配置。
- [[event-source-scope-and-learning]] — 回传范围对算法学习的影响。
- [[data-discrepancy-playbook]] — Google DDA+SAN 与 MMP Last-Click 的差异排查。
