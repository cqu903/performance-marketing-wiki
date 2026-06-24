---
source_url: https://developers.facebook.com/docs/marketing-apis/
ingested: 2026-06-24
sha256: e7f6be604f11d05f9ec048d6d8b5cd876030a0c825e5f216c5861a01a22d04d6
---
# Meta Marketing API 概览与起步摘录

Source pages:
- https://developers.facebook.com/docs/marketing-apis/
- https://developers.facebook.com/docs/marketing-api/overview
- https://developers.facebook.com/docs/marketing-api/get-started

Meta Marketing API 是一组 Graph API endpoints 和相关功能，用于跨 Meta technologies 投放广告，包括 Facebook、Instagram、Messenger、WhatsApp。它可用于广告创建、广告管理和效果分析。

核心能力：
- 程序化创建 campaigns、ad sets、ads 和 creatives。
- 更新、暂停或删除广告。
- 保持 campaign 与业务目标一致。
- 读取 insights 和 analytics，用数据驱动优化。

广告结构：
- Campaign：广告账号内最高层结构，代表单一 objective。Campaign objective 会约束该 campaign 下广告的合法配置。
- Ad set：一组广告，用于配置预算、投放周期、targeting、billing、optimization goal、bid 等。通常每个目标 audience 建一个 ad set，以控制预算、投放时段和指标。
- Ad creative：广告的视觉和素材元素。Creative 创建后不能修改；广告账号有 creative library 用于复用。
- Ad：包含展示广告所需的信息，包括 ad creative。一个 ad set 内可以创建多个 ads，以测试不同图片、链接、视频、文案或 placements。

起步要求：
- Active ad account：需要一个活跃广告账号才能跑 campaign、管理 billing、跟踪花费和监控表现。
- Ad account number：可在 Meta Ads Manager 的 ad account settings 中找到。
- Meta Developer account：需要注册 Meta Developer。
- Meta App：需要在 App Dashboard 创建 app。
- Authorization：验证访问 Marketing API 的用户和 app，并授予相应权限。
- Authentication：每次 Marketing API 调用都需要 access token。

相关 APIs：
- Conversions API：把服务器上的营销数据连接到 Meta 系统，用于优化广告定位、降低 CPA、衡量结果。
- Catalog API：创建商品 catalog，用于广告、Facebook/Instagram shop 等。
- Business Management API：维护 business 在 Facebook、Instagram、Messenger、WhatsApp 上的自然和付费资产。
- Commerce API：连接商家基础设施和 Meta commerce 工具。

页面更新时间：overview/get-started 2026-06-16。
