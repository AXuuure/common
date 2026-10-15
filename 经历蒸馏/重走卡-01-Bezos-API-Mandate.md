# 重走卡 01 · Bezos 的 API Mandate（2002）

- **决策点**：Jeff Bezos，2002 年，向全公司（约 150 名工程师规模）下达 API mandate，强制所有团队通过服务接口通信
- **原始信源**：[转述-未验证] 文本最早可溯源的公开记录是 Steve Yegge（Amazon 前员工，1998–2005）2011 年 10 月的 Google+ 长帖，Yegge 自述原文 "went something along these lines"；Bezos 本人及 Amazon 官方从未发布原文，也从未正式否认
- **引用口径**：面试/对外材料引用时必须带"据广泛转述"字样，不能说"Bezos 在备忘录里写道"

## 当时的信息集（只有这些）

- Amazon 是典型的单体架构：内容、客服、物流等所有零售功能挤在一个代码库里
- 团队扩张很快，团队间协作靠人际沟通：开会、对齐、排期
- 每开一个新业务，都要和一堆团队重新谈判一轮
- AWS 还不存在（2006 年才上线）；"microservices"这个词还没被造出来（2011 年前后才流行）
- SOA 概念正在被大企业热炒，但业界普遍认为是"大公司的重活"，小团队不碰
- 同期 Google 内部正相反：平台团队各自为政，团队间点对点集成越缠越死（Yegge 的帖子就是在骂这个）
- [过滤提示] 以下是你**不可能**知道的：AWS 会成功、微服务会成为主流、这份备忘录会成为技术史典故

## 第一问（先写答案，再往下看）

> 你是 Bezos。公司要快速进入新市场，但每进一个市场，新团队都要和几十个老团队开会、排期、等接口。你手里有 CEO 的全部权力，但改变不了人的协作习惯。**你会下什么决策？**

必须写两样，缺一样算没答：① **你的决策**（输出层）② **你的推理链**——你是怎么从上面的信息走到这个决策的，考虑过什么、排除了什么（思维链层）。写完再往下看。

## 对照：专家的实际决策

据转述， mandate 全文约六条，核心三条：

1. **接口是唯一合法通信方式**："There will be no other form of interprocess communication allowed: no direct linking, no direct reads of another team's data store, no shared-memory model, no back-doors whatsoever."
2. **接口从第一天起按可外部化设计**："All service interfaces, without exception, must be designed from the ground up to be externalizable... the team must plan and design to be able to expose the interface to developers in the outside world."
3. **技术栈自由，契约强制**："It doesn't matter what technology they use. HTTP, Corba, Pubsub, custom protocols — doesn't matter. Bezos doesn't care."

（另有 "Anyone who doesn't do this will be fired. Thank you; have a nice day!"——转述者注明第七条是玩笑。）

## 对照：专家的思维链（重建）

mandate 文本本身不含推理过程，以下是从公开材料重建的，逐条标注可信度：

- [实测] 上下文锚点：CTO Werner Vogels 事后回顾，当时的痛是 "iterative development on a monolithic code base that included everything from content to customer service apps to the logistics of shipping packages"——单体痛苦是真实的，不是假想敌。
- [推断] 三条 mandate 的排序不是随意的，是三步结构：**先禁死退路**（no back-doors，消除例外的可能）→ **再抬高标准**（externalizable，把内部协调转成产品质量问题）→ **最后放开执行**（技术栈自由，收窄决策范围避免 mandate 沦为选型战争）。多数人写规范只做第一步，所以规范沦为倡议。
- [知识补全-待验证] Bezos 长期决策框架（regret minimization 等）在他其他公开材料里有自述，但**没有任何材料记录 mandate 当时他具体怎么想的**——这一条面试不可引，只能当理解背景。

## 机制归因（你的方案可能缺什么）

多数人面对这个处境的答案是"建 API 规范""推动团队解耦""搞平台化"。对照 mandate，这些答案缺的是：

- **缺不可谈判性**：没有 "No exceptions" 和执行力背书的规范 = 倡议。mandate 把架构决策变成组织纪律，而不是技术建议。
- **缺 externalizable 这一记绝杀**：这是整份文件最容易被跳过的一条。它把内部政治问题（"求别的团队排期"）转化为产品质量问题（"接口要能服务外部开发者"）——**接口的验收标准从"同事觉得方便"变成"陌生人能不打电话就用起来"**。不设计这一条，前面的强制令只会制造出大量难用的内部接口。
- **把 "Bezos doesn't care" 当段子，会漏掉它的机制**：刻意收窄决策范围——只锁接口契约，不锁实现选型。这避免了 mandate 沦为技术选型战争，也让各团队没有抗拒的着力点。
- **缺系统-激励对齐的视角**：接口一旦 externalizable，"被更多团队复用"就从行政要求变成团队的自身利益。机制制造激励，而不是靠文化号召。

## 迁移题（你当前处境的等价决策点）

**你的 ChorusTavern 已经有 ChatProvider/MemoryProvider 接口——你以为自己已经做了这个决策。对照三条 mandate，逐条自检：**

1. "no other form of communication" → 你的"业务层不直接 import 中间件"纪律，有没有 no exceptions？偷偷 import 过一次的地方就是未来的技术债源头。去翻代码，找出所有例外。
2. "designed to be externalizable" → 你的接口文档，是写给"不了解内部实现的第三方"看的，还是只写给三个月后的自己看？这是**差一个档次**的分界线：可替换 ≠ 可外部化。
3. "Bezos doesn't care" → 你的接口契约里，有没有把 LangGraph/Spring AI 的具体用法漏进业务层？漏了，就是锁了实现，没锁契约。

**面试叙事用途**：被问"你的半自研架构怎么解耦的"，不要背"高内聚低耦合"。讲这段重走——"我知道这个决策的原始形态是 Bezos 2002 年的 mandate，它的核心不是接口，是 externalizable 和 no exceptions；我对照自检后发现我的接口只做到了可替换，差在……"——这个回答比任何架构词都有信息量。

## 偏差记录（跑完填写，按三层归因）

- 信息集差距：我决策时忽略了什么当时可见的信息：____
- 推理链差距：我的推理路径缺了什么环节（预期答案：不可谈判性 / externalizable / 激励对齐，占几样）：____
- 结论差距：结论本身差在哪：____
- 这次暴露的系统性偏差：____（跑够多张后回看聚类）
