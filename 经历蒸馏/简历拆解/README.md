# 年薪百万、美团 L9：阿酥在coding 简历拆解

> 用途：把一份"20 岁双非本科拿美团北斗计划 L9 技术专家 offer"的 Agent 方向简历，拆成可对照学习的知识点。
> 来源：飞书文章《年薪百万、美团L9"背后：拆解阿酥在coding简历里的 Agent 技术》（作者：可冰乐）＋ 简历原图两页。
> 简历作者：阿酥在coding ｜ GitHub: [LofiSu](https://github.com/LofiSu)
> 建立日期：2026-10-15

## 目录

1. [简历逐段解析](resume-breakdown.md) — 基本信息、五段经历时间线、每段的技术链路与量化指标、这份简历在卖什么
2. [DeerFlow 技术拆解笔记](deerflow-article-notes.md) — 文章对简历第一段项目（字节开源 Long-Horizon Agent Harness）的机制讲解，1.0 与 2.0 分层

## 原始材料

| 文件 | 内容 |
|------|------|
| [imgs/resume-page1.png](imgs/resume-page1.png) | 简历第 1 页原图（1092×2048）：基本信息、DeerFlow、抖音 AI 基建 AI4SE |
| [imgs/resume-page2.png](imgs/resume-page2.png) | 简历第 2 页原图（1206×1628）：抖音电商、直播中台、B 站、开源与创业项目 |

## 一句话结论

简历真正撑住 L9 的不是"会用 Agent 框架"，而是三件套叠加：**会搭 harness**（DeerFlow 2.0）、**会造训练与评测数据管线**（AgentScaling / Agent Tracer / Rubric / Failure Onset）、**有真实用户与商业指标**（两个付费转化率 38%／42% 的独立产品）。文章只拆了第一件，后两件才是简历里更贵的部分。

## 已知缺口

- 原文档内嵌 3 张飞书表格（沙箱作用对照、Context 整理机制、11 层 Middleware 职责表）未取到单元格原文，正文以段落形式转述其内容。补取需对 lark-cli 追加授权 `sheets:spreadsheet:read`。
- 简历中"哔哩哔哩 OGV 技术部"一段只有标题行，无展开内容。
- 文章观点（如"不推荐从 0 到 1 自己搭 Agent"）归属作者可冰乐，非简历作者，引用时需区分。
