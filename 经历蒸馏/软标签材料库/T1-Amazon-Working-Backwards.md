# T1-Amazon-Working-Backwards · PR-FAQ 机制

- **信源**：AWS Executive Insights《The Secrets to Product Management at AWS》（aws.amazon.com/executive-insights，Andy Jassy 署名引文）
- **层级**：T1（Amazon/AWS 官方出版物）
- **抓取日期**：2026-10-15

## 结论层（他们公开说的）

- [实测] 机制顺序原文："before we ever request a budget, assemble a team, or write a line of code - we write a press release (PR) for what we imagine this final product will accomplish."
- [实测] 单页限制是设计不是传统："Because it must fit on a single page, this is an exercise in simplifying and focusing our big ideas into a compelling and concise narrative."
- [实测] FAQ 的两半结构：
  - 前半是客户问题："What happens when the product breaks?" "Will my data be secure?" "Why would I choose this over another product?"
  - 后半是自家问题："Will this product be profitable?" "Might it cannibalize other products?" "Do we have the resources to make this work?"
- [实测] 官方宣称的机制价值："They force product teams to prioritize what is most important, identify what features to invest in, uncover issues before they occur, and create organizational alignment, all before product development begins."

## 软标签层（决策痕迹）

- [实测] FAQ 刻意不穷尽："We don't include every possible question. We start with the most important first, working until we are comfortable that we've answered enough questions to make decisions quickly"——**问答集是决策工具，不是文档。这一句是 PR-FAQ 最容易被忽略的设计内核。**
- [实测] Vision 与 details 分离："stubborn on vision while flexible on the details"——PR-FAQ 锁死愿景，实现路径保持开放。
- [实测] 官方给的案例是 EC2：团队"launching a more limited version earlier, and then applying an iterative approach to flesh out the rest"，迭代单位叫 "minimum lovable products"（最小可爱产品，注意不是 MVP）。
- [实测] 一切想法按假设处理："treat any idea we have - no matter how experienced we are in the market, how high in the organization the idea generator sits - as a hypothesis."
- [推断] PR-FAQ 的本质是"把假设写成可被否决的文档"：一页纸的篇幅限制 = 强制信息压缩；先写对外叙事 = 强制从客户视角定义问题；两半 FAQ = 强制把"客户要不要"和"我们划不划得来"分开验证。三个强制点都指向同一个目标：在投入资源前制造否决点。

## 降温层（在"蚂蚁系 5 年 + 个人项目"约束下）

- [推断] 对 ChorusTavern 需求文档的直接迁移：v0.13 需求文档本质上就是 PR-FAQ 的变体（先定义角色卡/群聊/记忆的对外叙事，再写实现）。面试讲"需求验证"时，可以主动亮出这层映射——不是"我学过 Amazon 的方法"，是"我的需求文档结构恰好是 PR-FAQ 的同构物，且我知道它同构在哪"。
- [推断] 单页限制在个人项目里没人强制执行，必须自己造约束。写需求文档的典型病是越写越长，PR-FAQ 的价值恰好在"强制一页"这个不舒服的设计上——不舒服才是机制生效的部分。
- [推断] "两半 FAQ"可以改造成个人项目的双栏自检：客户问题半栏 = 用户旅程走查（他已有的 User Journey Walkthrough）；自家问题半栏 = 成本/范围自检（"这个功能我维护得起吗"）。

## 缺口层

- [挖不到] Amazon 内部 PR-FAQ 的真实评审机制：谁有权否决、平均被打回几次、VP 级评审的真实通过率——内部流程数据不公开。
- [注意] workingbackwards.com 是前 Amazon 高管（Colin Bryar / Bill Carr 相关）的第三方站点，其"依赖项前置审批"等细节只能当交叉印证，**不能当 T1 citation**。
- [注意] aboutamazon.co.uk 那篇是 SMB 培训项目宣传稿，公关属性强，同属交叉印证级。
- [挖不到] aboutamazon.com 本体的 Working Backwards 专文本轮未取到（搜索返回均为 AWS 视角），Amazon 零售视角与 AWS 视角的表述差异待补。
