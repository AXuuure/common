# T1-DORA-five-keys · DORA 软件交付效能指标

- **信源**：DORA 官方指南页，Nathen Harvey 撰写（dora.dev/guides/dora-metrics/）
- **层级**：T1（DORA 研究团队官方发布，State of DevOps Report 的指标定义源头）
- **抓取日期**：2026-10-15
- **⚠️ 时效警示**：DORA 现行模型是**五个指标**，不是四个。MTTR 已被替换。

## 结论层（他们公开说的）

- [实测] 现行五指标分两组：
  - **Throughput（吞吐）**：change lead time（提交到部署的时长）、deployment frequency（部署频率）、failed deployment recovery time（失败部署恢复时长）
  - **Instability（不稳定）**：change fail rate（需立即干预的部署占比）、deployment rework rate（因生产事故引发的计划外部署占比）
- [实测] 核心论断原文："speed and stability are not tradeoffs. In fact, we see that the metrics are correlated for most teams. Top performers do well across all five metrics, and low performers do poorly."
- [实测] 官方引 Dave Farley（《Modern Software Engineering》p.154）："the real trade-off, over long periods of time, is between better software faster and worse software slower."
- [实测] 适用范围原文："best suited for measuring one application or service at a time"——按单个应用/服务度量，不是按组织一刀切。

## 软标签层（决策痕迹）

- [实测] 指标模型经历过公开演变：从 four keys 到五指标，MTTR 被替换为 Failed Deployment Recovery Time，并新增 Deployment Rework Rate。官方设有专页记录这段演变（dora.dev/insights/dora-metrics-history）——**这是官方自己写"我们为什么改指标"的材料，标准的 ADR 式软标签，比指标本身更值得读。**
- [实测] 官方自列的 pitfalls（等于官方承认的失效模式）：
  - 把指标当目标（Goodhart's law：喊"年底前每个应用必须日更"的团队会被 gaming）
  - 想用一个指标统治所有（官方原话：要有"a healthy amount of tension between them"的多个指标）
  - 拿行业当挡箭牌拒绝改进
  - 跨应用横向比较（mobile app vs mainframe 直接比是误导）
  - 指标按团队切分归属（导致 finger-pointing，官方要求 dev/ops/release 共享全部五个）
  - 为采集数据过度投入集成而忽视改进本身（官方明说这种初期投资"might not be worth it"）
- [推断] 换掉 MTTR 的方向性理由：MTTR 的起算点跨团队定义模糊（从发现、确认还是开始修？），FDRT 把口径锁死在"部署失败后的恢复"，可测性优先于全面性。**此条为推断，需抓 history 页面对官方原话验证后才可引用。**

## 降温层（在"蚂蚁系 5 年 + 个人项目"约束下）

- [推断] 面试引用必须用"现行五指标"口径。还在说"DORA 四指标"会直接暴露知识过期——这是"应用现状前置刷新"原则的实证案例，比四指标本身更值得写进面试准备笔记。
- [推断] 个人项目场景不需要完整五指标埋点（官方自己都说过度采集不值得）；但"单应用度量""指标间要有张力""不许跨应用横比"这三条设计思想，可以直接迁移到 Agent 效果评估平台的指标设计——避免把 LLM-as-Judge 的单点分数当北极星指标。
- [推断] 金融级约束下的适配点：change fail rate / rework rate 在强合规环境天然偏高，横比会误判；应改用趋势（自身环比）而非行业分位。

## 缺口层

- [挖不到] dora.dev/insights/dora-metrics-history 本轮未抓取，MTTR→FDRT 的官方原话理由待补。
- [挖不到] 年度 State of DevOps Report PDF 未抓，四→五演变发生在哪一年哪期待补。
- [注意] 页面为活文档，指标定义后续仍可能调整；引用前复查 URL 确认未再改版。
