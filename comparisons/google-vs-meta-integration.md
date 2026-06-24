---
title: Google Ads 与 Meta ads 对接对比
created: 2026-06-24
updated: 2026-06-24
type: comparison
tags: [google-ads, meta-ads, integration, event-mapping, discrepancy]
sources: [raw/articles/115002504686-google-ads-integration.md, raw/articles/207033826-meta-ads-integration.md, raw/articles/208338403-lookback-windows.md]
confidence: high
---

# Google Ads 与 Meta ads 对接对比

| 维度 | Google Ads | Meta ads | X Wallet 操作重点 |
|---|---|---|---|
| 平台类型 | SRN | SRN | 不要用普通点击归因逻辑理解全部数据 |
| 必备资产 | Link ID、Google 管理员权限、转化导入 | Meta app/广告账户权限、事件映射、AEM | 资产归广告主持有 |
| 默认点击窗口 | 30 天 | 7 天 | AppsFlyer 内尽量对齐 |
| 默认浏览窗口 | 24 小时 | 24 小时 | iOS Google 展示认领有限制 |
| 基础转化 | first_open | install/app events | 必须先能看到基础事件 |
| 再互动 | session_start 很关键 | 再互动/再归因配置 | 再营销单独看，不混入拉新 |
| iOS 隐私 | SKAN/IDFA 限制；iOS 搜索不归因，展示需设备 ID 授权；gbraid/建模用于部分无设备 ID 场景 | ATT/SKAN/AEM 强相关；AEM 合格性和高级数据共享会影响优化与差异 | iOS 单独建报表口径，传统归因/SKAN/平台建模分开看 |
| 常见差异 | 未导入转化、Link ID/MCC、窗口、成本账号 | AEM、窗口、事件映射、隐私延迟 | 先查配置，再看投放表现 |

## 结论
Google 的治理重点是 Link ID / MCC / 转化导入 / iOS 建模口径；Meta 的治理重点是事件映射 / AEM 合格性 / SKAN 与 iOS 隐私限制。两者共同点是：归因窗口必须对齐，深层事件回传要先测试再用于优化；iOS 数据必须允许延迟和聚合，不应按 Android 的实时用户级链路要求代理解释。

## 相关页面
- [[google-ads]]
- [[meta-ads]]
- [[postback-and-event-mapping]]
- [[lookback-window-strategy]]
- [[ios-privacy-skan-aem]]
