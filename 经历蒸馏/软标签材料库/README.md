# 软标签材料库 · 研发流程方法论（第一批：样板卡）

> 用途：面试叙事 + 方法论对齐的 T1 信源挖掘底稿。
> 核心方法论：不是收集结论，是挖"软标签"——决策痕迹、淘汰史、约束条件。
> 建立日期：2026-10-15

## 三档标注规范

| 标注 | 含义 | 面试可用性 |
|------|------|-----------|
| [实测] | 官方原文可直接抄录，URL 已记录在卡内 | 可直接引用 |
| [推断] | 从官方材料推导，逻辑链写在卡内 | 可说"我的判断是"，不能说"官方说" |
| [挖不到] | 信源未取到，或不存在公开版本 | 禁止引用；面试前必须补或删 |

## 卡片四层结构

- **结论层**：他们公开说了什么（可引用的一句话锚点）
- **软标签层**：决策痕迹 / 淘汰的备选 / 当时的约束条件（暗知识）
- **降温层**：在"蚂蚁系 5 年 + 个人项目/小团队"约束下成不成立
- **缺口层**：这份材料缺什么、哪里可能被公关过滤

## 索引

| 卡片 | 信源 | 层级 | 状态 |
|------|------|------|------|
| T1-DORA-five-keys.md | dora.dev 官方指南（Nathen Harvey） | T1 | 已取原文 |
| T1-Amazon-Working-Backwards.md | AWS Executive Insights《The Secrets to Product Management at AWS》 | T1 | 已取原文 |
| T1-Google-SRE-postmortem.md | sre.google《SRE Book》第 14 章 | T1 | 网络不可达，仅挖掘计划，无内容 |

## 下一批挖掘候选（按优先级）

1. DORA metrics history 页（补 MTTR→FDRT 换指标的官方理由）
2. sre.google SRE Book postmortem culture 章节（换网络环境后补）
3. Uber Engineering 官方博客：PRD vs prototype 的关系
4. Spotify engineering culture 官方页（Think It / Build It / Ship It / Tweak It）
5. 腾讯《互联网产品研发流程概论》（吴涛，需求文档编写 9 步）
6. 字节青训营敏捷流程官方文档
7. aboutamazon.com Working Backwards 专文（Amazon 零售视角，区别于 AWS 视角）
8. SRE Book 第 3/4 章：Embracing Risk / SLO（error budget 的原始出处）
