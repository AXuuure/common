# T1-Google-SRE-postmortem · Blameless Postmortem 文化（挖掘计划卡）

- **信源**：《Site Reliability Engineering》(O'Reilly, 2016) 第 14 章 "Postmortem Culture: Learning from Failure"，官方在线版 sre.google/sre-book/postmortem-culture/
- **层级**：T1（Google SRE 团队官方著作）
- **抓取日期**：2026-10-15
- **状态**：⚠️ **原文未取到，本卡不含任何内容，只列挖掘计划。**

## 为什么这张卡是空的

抓取尝试全部失败，记录在案：

| 尝试 | 通道 | 结果 |
|------|------|------|
| sre.google 直连 | WebFetch ×2 | fetch failed |
| sre.google 直连 | curl（浏览器 UA） | 超时（exit 28） |
| GitHub 官方源仓库 | GitHub API | 仓库不存在（官方未开源正文，搜到的 e-book 生成器均为第三方） |
| Wayback Machine | WebFetch + availability API | 不通 |

**决定：不写卡，不拿训练记忆冒充原文。** 这张卡本身是三档标注规范的反面示范——信源不可达时，正确动作是留空 + 记录路径，而不是凭记忆补一段"看起来对"的内容。面试材料里凭记忆写的 SRE 引文，是穿帮高危项。

## 挖掘计划（网络恢复后执行）

按信息价值从高到低挖：

1. **定义锚点**：blameless postmortem 的官方定义句（面试引用就引这一句，引用前必须回原文核对措辞）
2. **设计理由**：为什么 blameless 优于 blameful——找"指责导致隐瞒事故"的论证链
3. **Counterexamples 节**：官方自己列的"何时不该 blameless"（如重复犯同一错误、明知故犯的场景）——这是软标签层，比定义更有信息量
4. **历史演变**：Google 早期复盘实践如何演进到 blameless（如有）
5. **附录 D**：官方 postmortem 模板的字段结构（"事后检讨范例"）

## 降温层（待原文到位后补）

- [待补] blameless 的个人项目适配：一个人开发没有"追责"对象，blameless 的核心价值（让人敢报事故）失效；但"聚焦系统而非人"的复盘提问方式仍可迁移到 Agent 项目的 bad case 分析——把"我为什么犯蠢"改写成"系统在哪一步给了错误信号"。

## 缺口层

- 全部内容待取。可替代通道：本地 O'Reilly 电子书（如已购）、公司内网图书馆、换网络环境后重试 sre.google。
- 若三条路都不通，面试叙事中涉及 blameless postmortem 的部分整体降级为"我了解这个概念但未回读原文"，或改引 Google Cloud 博客上可达的官方文章替代。
