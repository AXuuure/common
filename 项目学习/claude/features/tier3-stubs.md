# Tier3 stubs

# Tier 3 — 纯 Stub / N/A 低优先级 Feature 概览

> 本文档汇总所有 Tier 3 feature。这些功能要么是纯 Stub（所有函数返回空值），
> 要么是 Anthropic 内部基础设施（N/A），要么是引用量极低的辅助功能。

## 概览

| Feature | 引用 | 状态 | 类别 | 简要说明 |
| - | - | - | - | - |
| CHICAGO\_MCP | 16 | 已实现 | 工具 | Computer Use + Chrome MCP 控制（build 默认启用） |
| MONITOR\_TOOL | 13 | 已实现 | 工具 | 后台监控工具，持续监视 shell 输出（build 默认启用） |
| BG\_SESSIONS | 11 | 部分实现 | 会话管理 | 后台会话注册/清理已实现，任务摘要是 stub（dev 默认启用） |
| SHOT\_STATS | 10 | 已实现 | 统计 | API 调用统计面板（build 默认启用） |
| EXTRACT\_MEMORIES | 7 | 已实现 | 记忆 | 自动记忆提取（build 默认启用，受 GrowthBook 门控） |
| TEMPLATES | 6 | 部分实现 | 项目管理 | 项目/提示模板系统（dev 默认启用） |
| LODESTONE | 6 | 已实现 | 深度链接 | URL 协议处理器（build 默认启用） |

## 单引用 Feature（40+ 个）

以下 feature 各只有 1 处引用，多为内部标记或实验性功能：

UNATTENDED\_RETRY, ULTRATHINK, TORCH, SLOW\_OPERATION\_LOGGING, SKILL\_IMPROVEMENT,
SELF\_HOSTED\_RUNNER, RUN\_SKILL\_GENERATOR, PERFETTO\_TRACING, NATIVE\_CLIENT\_ATTESTATION,
KAIROS\_DREAM（见 kairos.md）, IS\_LIBC\_MUSL, IS\_LIBC\_GLIBC, DUMP\_SYSTEM\_PROMPT,
COMPACTION\_REMINDERS, CCR\_REMOTE\_SETUP, BYOC\_ENVIRONMENT\_RUNNER, BUILTIN\_EXPLORE\_PLAN\_AGENTS,
BUILDING\_CLAUDE\_APPS, ANTI\_DISTILLATION\_CC, AGENT\_TRIGGERS, ABLATION\_BASELINE

## 优先级说明

这些 feature 被列为 Tier 3 的原因：

1. **已实现但影响范围小**（CHICAGO\_MCP, LODESTONE, SHOT\_STATS, EXTRACT\_MEMORIES, MONITOR\_TOOL）：已在 build/dev 默认启用，主要作为其他功能的基础设施
2. **部分实现**（BG\_SESSIONS, TEMPLATES）：核心注册已实现，但部分功能如任务摘要仍是 stub
3. **辅助功能**（STREAMLINED\_OUTPUT, HOOK\_PROMPTS）：影响范围小
4. **CCR 系列**：依赖远程控制基础设施，需要 BRIDGE\_MODE 先完善

如需深入了解某个 Tier 3 feature，可以在代码库中搜索 `feature('FEATURE_NAME')` 查看具体使用场景。


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.