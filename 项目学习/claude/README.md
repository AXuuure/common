# Claude Code 架构白皮书（本地副本）

来源：[在线文档站](https://ccb.agent-aura.top/) ｜ 源仓库：[claude-code-best/claude-code](https://github.com/claude-code-best/claude-code)

共 64 篇，每篇一个 markdown 文件；目录层级与站点章节一致（13 个章节组 + 附录）。

## 目录

### 开始 / 介绍

1. [什么是 Claude Code - Terminal Native Agentic Coding System](introduction/what-is-claude-code.md)
2. [为什么写这份白皮书 - Claude Code 逆向工程分析](introduction/why-this-whitepaper.md)
3. [架构全景 - Claude Code 五层架构详解](introduction/architecture-overview.md)

### 对话是如何运转的

4. [Agentic Loop：AI 自主循环的核心机制](conversation/the-loop.md)
5. [流式响应机制 - Claude Code 打字机效果原理](conversation/streaming.md)
6. [多轮对话管理 - QueryEngine 会话编排与持久化](conversation/multi-turn.md)

### 工具：AI 的双手

7. [工具系统设计 - AI 如何从说到做](tools/what-are-tools.md)
8. [文件操作工具 - 三大工具的源码级解剖](tools/file-operations.md)
9. [命令执行工具 - BashTool 安全设计与实现](tools/shell-execution.md)
10. [搜索与导航工具 - 代码库精准定位](tools/search-and-navigation.md)
11. [任务管理系统 - TodoWrite 与 Tasks 双轨架构](tools/task-management.md)

### 上下文工程

12. [System Prompt 动态组装 - AI 工作记忆构建](context/system-prompt.md)
13. [项目记忆系统 - 文件级跨对话记忆架构](context/project-memory.md)
14. [上下文压缩 - Compaction 三层策略与边界机制](context/compaction.md)
15. [Token 预算管理 - 上下文窗口动态计算](context/token-budget.md)

### 多 Agent 协作

16. [子 Agent 机制 - 权限、流程、同步/异步与 Fork](agent/sub-agents.md)
17. [Worktree 隔离 - Git Worktree 实现文件级隔离](agent/worktree-isolation.md)
18. [协调者与蜂群模式：多 Agent 编排机制](agent/coordinator-and-swarm.md)

### 可扩展性

19. [MCP 协议 - 连接管理、工具发现与执行链路](extensibility/mcp-protocol.md)
20. [Hooks 生命周期钩子 - 执行引擎与拦截协议](extensibility/hooks.md)
21. [Skills 技能系统 - Prompt 即能力的架构哲学](extensibility/skills.md)
22. [自定义 Agent - 从 Markdown 到运行时的完整链路](extensibility/custom-agents.md)

### 安全与权限

23. [AI 安全至关重要 - Claude Code 安全设计哲学](safety/why-safety-matters.md)
24. [权限模型 - Allow/Ask/Deny 三级权限体系](safety/permission-model.md)
25. [沙箱机制 - 权限系统之外的第二道防线](safety/sandbox.md)
26. [计划模式 - Plan Mode 先看后做的安全机制](safety/plan-mode.md)
27. [Auto Mode - AI 分类器驱动的自主执行模式](safety/auto-mode.md)
28. [工信部预警 Claude Code 后门 - 5 分钟切到 CCB 兼容](safety/miit-warning-response.md)

### 揭秘：隐藏功能与内部机制

29. [三层门禁系统 - 功能可见性控制架构](internals/three-tier-gating.md)
30. [88 个 Feature Flags - 构建时特性门控全解](internals/feature-flags.md)
31. [GrowthBook A/B 测试体系 - 运行时功能发布](internals/growthbook-ab-testing.md)
32. [GrowthBook 适配器 - 自定义 Feature Flag 服务器接入](internals/growthbook-adapter.md)
33. [自定义 Sentry 错误上报配置](internals/sentry-setup.md)
34. [未公开功能巡礼 - 8 个隐藏功能深度解析](internals/hidden-features.md)
35. [Ant 特权世界 - Anthropic 员工专属功能](internals/ant-only-world.md)
36. [Session transcript persistence](internals/session-transcript-persistence.md)
37. [Debug 模式](features/debug-mode.md)
38. [Buddy 宠物系统](features/buddy.md)

### 隐藏功能详解 / Agent 与协作

39. [Coordinator mode](features/coordinator-mode.md)
40. [Fork subagent](features/fork-subagent.md)
41. [Daemon](features/daemon.md)
42. [Teammem](features/teammem.md)

### 隐藏功能详解 / 运行模式

43. [Kairos](features/kairos.md)
44. [Voice mode](features/voice-mode.md)
45. [Bridge mode](features/bridge-mode.md)
46. [Remote control self hosting](features/remote-control-self-hosting.md)
47. [Proactive](features/proactive.md)
48. [Ultraplan](features/ultraplan.md)

### 隐藏功能详解 / 工具增强

49. [Mcp skills](features/mcp-skills.md)
50. [Tree sitter bash](features/tree-sitter-bash.md)
51. [Bash classifier](features/bash-classifier.md)
52. [Web browser tool](features/web-browser-tool.md)
53. [Experimental skill search](features/experimental-skill-search.md)

### 隐藏功能详解 / 上下文与自动化

54. [Token budget](features/token-budget.md)
55. [Context collapse](features/context-collapse.md)
56. [WORKFLOW_SCRIPTS — 确定性多 agent 工作流编排](features/workflow-scripts.md)
57. [Auto dream](features/auto-dream.md)
58. [Tier3 stubs](features/tier3-stubs.md)

### 基础设施与依赖

59. [Auto updater](auto-updater.md)
60. [Lsp integration](lsp-integration.md)
61. [External dependencies](external-dependencies.md)
62. [Telemetry remote config audit](telemetry-remote-config-audit.md)

### 附录（站点可访问，未列入导航）

63. [All features guide](features/all-features-guide.md)
64. [Acp link](features/acp-link.md)

## 转换说明

- 正文取自文档站“页面 URL + `.md`”端点，即站点实际发布的渲染后 markdown，不是抓取 HTML。
- MDX 组件已转为纯 markdown：Note/Tip/Warning 转引用块；CardGroup/Card 转列表；Steps/Step 转编号小标题；AccordionGroup/Accordion 转加粗小标题；Frame+img 转图片语法。
- 章节互链改为本地相对路径；指向 src、packages 的链接改为 GitHub 绝对链接；3 张架构图存于 images 目录。
- 每页顶部由 Mintlify 注入的索引提示块已移除。
