---
title: 设备标识符与隐私限制
created: 2026-06-24
updated: 2026-07-05
type: concept
tags: [device-id, privacy, attribution, skan]
sources: [raw/articles/4408847686161-device-identifiers.md, raw/articles/360011890298-ios14-att-skan-faq.md, raw/articles/207447053-appsflyer-attribution-model.md, raw/articles/20260705-appsflyer-sdk-onelink-ios-noidfa-full-notes.md]
confidence: high
---

# 设备标识符与隐私限制

设备标识符是确定性归因的基础。AppsFlyer SDK 会自动收集 AppsFlyer ID 和部分平台标识符；广告主也可根据市场需求额外收集，例如中国 Android 生态中的 OAID。^[raw/articles/4408847686161-device-identifiers.md]

## 常见标识符
- iOS：IDFA、IDFV。IDFA 受 ATT 影响；IDFV 通常可用于同一供应商旗下 App 的交叉推广和重装识别。
- Android：GAID；非 Google Play 服务环境可能涉及 OAID、Android ID、IMEI、Fire ID。
- AppsFlyer ID：AppsFlyer 自身生成的标识符，用于平台内用户链路。

## 对 Google / Meta 的影响
- Google Ads 对接文档明确要求应用必须收集 IDFA / GAID；但在 iOS 14.5+ 上，Google iOS 应用库存并不总能使用 ATT 框架下的 IDFA，因此会结合建模转化、SKAN 和 gbraid 等机制。详见 [[ios-privacy-skan-aem]]。^[raw/articles/115002504686-google-ads-integration.md]
- Meta 在 iOS 上受 ATT、SKAN、AEM 限制，事件可用性和回传延迟会影响优化。AEM 场景下，S2S 事件尤其要关注 IP、IDFV、IDFA 空值/全零值和事件来源范围。^[raw/articles/19228737402129-meta-ios-aem.md]
- 如果设备 ID 缺失，系统会更多依赖概率模型、平台建模或汇总模型，单用户级准确度下降；这会直接影响代理复盘时能否追到具体 campaign/adset/ad 的用户级链路。

## iOS 隐私下的报表边界
- IDFA 可用时，AppsFlyer 传统归因和广告平台归因链路更完整。
- IDFA 不可用时，AppsFlyer 仍可能通过 SKAN、UDL、SRN API、IDFV、平台建模等方式提供部分结果，但颗粒度、时效和可解释性会下降。
- SKAN 和传统归因是互补口径，不应简单相加；同一批 iOS 安装可能在不同报表中以不同归因状态出现。
- 对 X Wallet 这种贷款 App，最终放款通常晚于 SKAN 的早期衡量窗口，因此后端贷款状态要与 AppsFlyer early events 做漏斗校验，而不是期待 SKAN 直接解释全量 ROI。

## 操作建议
1. 让开发确认 AppsFlyer SDK 版本、IDFA/GAID/IDFV 采集状态。
2. iOS 明确 ATT 弹窗策略；不要把 iOS 归因问题简单归咎于代理投放。
3. Android 若存在第三方商店/中国大陆包，再单独规划 OAID 和 multi-store；香港 Google Play 版本优先保证 GAID 和 Referrer。
4. iOS 报表按“传统归因 / SKAN / 平台建模 / 后端实际贷款结果”分层复盘，避免把隐私机制导致的数据缺口当成单一渠道表现。

## IDFA 在点击链路 vs 激活链路的可用性
IDFA 的可用性不仅受 ATT 影响，还取决于链路阶段：点击广告时，Safari/H5/浏览器 JS 无系统权限读取 IDFA，仅媒体 App（抖音/Meta 等）可在跳转 OneLink 时被动传入；若用户拒绝追踪则传入全零无效值。AF SDK 是全链路唯一稳定获取 IDFA 的渠道。无 IDFA 场景下的四层归因匹配（SKAN → af_clickid → IDFV → 概率指纹）详见 [[install-attribution-matching]]。

## 相关页面
- [[attribution-model]]
- [[ios-privacy-skan-aem]]
- [[data-discrepancy-playbook]]
- [[install-attribution-matching]]
