---
title: SKAdNetwork (SKAN)
created: 2026-07-12
updated: 2026-07-12
type: entity
tags: [skan, privacy, attribution, ios]
sources: [raw/articles/360011890298-ios14-att-skan-faq.md, raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md, raw/articles/20260712-ios-w2a-vs-android-a2a-uac-strategy.md]
confidence: high
---

# SKAdNetwork (SKAN)

SKAdNetwork（简称 SKAN）是 Apple 提供的隐私保护归因框架。广告平台通过 SKAN 接口向 Apple 注册广告活动签名，Apple 在用户安装后延迟匿名回传聚合转化数据给广告平台，再由广告平台转发给 [[appsflyer]] 等归因平台。SKAN 回传仅含广告网络 ID、活动 ID 和转化值（CV），不含用户级设备标识。

## 核心特征

- **聚合而非用户级**：回传无 IDFA/IDFV 等设备标识，无法做用户级明细分析。
- **延迟回传**：SKAN 4.0 窗口 1 通常 24-48 小时；窗口 2-3 可延迟 24-144 小时；Google 的 SKAN 原始数据有时需等待之后约 13 天的文件才完整。延迟是机制设计的一部分，不是平台"慢"。
- **短衡量窗口**：只覆盖安装后较短时间内的行为（D0 或最多 24-48h），对贷款 App 这种长决策链路天然不完整。
- **6bit 转化值**：SKAN 转化值仅 0-63 共 6bit，只能编码有限事件层级。贷款 App 应围绕早期质量信号设计 CV（注册完成、申请开始、资料提交），不能只盯最终放款。

## 与 AppsFlyer 传统归因的关系

SKAN 和 AppsFlyer 传统归因是**互补口径，不能简单相加**。同一批 iOS 安装可能在一个口径显示自然量、另一个口径显示非自然量。详见 [[ios-privacy-skan-aem]]。

SKAN 数据进入 AppsFlyer 的 SKAN 面板/API，传统归因数据进入常规面板/API；同一业务问题经常需要同时看两套数据。

## iOS 无 IDFA 四层归因中的角色

在 ATT 拒绝追踪场景下，SKAN 是 AppsFlyer 四层归因匹配的**最高优先级**。SKAN → af_clickid 精准匹配 → IDFV 弱关联 → 概率指纹兜底。SKAN 优点是 100% 合规、无误匹配；缺点是延迟高、仅活动层级、无法细分素材、晚转化丢失。详见 [[install-attribution-matching]]。

## 对 UAC 投放的影响

SKAN 6bit + ATT 限制导致 iOS 原生 A2A 信号残缺——只能 D0、24-48h 窗口、IDFA 缺失。这是 iOS 投放策略偏向 W2A 的根因之一。详见 [[ios-w2a-vs-android-a2a]]。

## X Wallet 操作要点

1. 确认 AppsFlyer SDK 支持当前 SKAN 版本；老 SDK 可能不支持 SKAN 4，甚至影响 App Store 审核。
2. SKAN 转化值设计：前 24-48 小时内可触发且能代表质量的事件优先放入 CV（注册完成、申请开始、资料提交、初审通过），放款成功量低且滞后，不适合作为唯一早期信号。
3. 报表节奏：iOS 不看单日实时结论。SKAN/深层事件复盘看 7-14 天。
4. 排查异常归因到某渠道时，SKAN 仅活动层级，无法细分到素材。

## 相关页面
- [[ios-privacy-skan-aem]] — SKAN 与 ATT/AEM/Google 建模的完整 iOS 隐私全景。
- [[install-attribution-matching]] — SKAN 在四层归因优先级中的位置。
- [[ios-w2a-vs-android-a2a]] — SKAN 约束推导 iOS W2A 优先策略。
- [[device-identifiers-and-privacy]] — IDFA/IDFV 等标识符与 SKAN 的关系。
- [[data-discrepancy-playbook]] — SKAN 数据与传统归因差异排查。
- [[att]] — AppTrackingTransparency 框架，决定 IDFA 可用性。
