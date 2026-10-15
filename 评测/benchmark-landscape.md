# 模型评测体系全景 - 谁在评、怎么评、能不能信

> 2026 年的"模型跑分"已经不是一件事，而是七个互不重叠的赛道。本文沉淀一份可复用的地图：哪些机构在评测、它们各自评的是什么、方法论差异在哪、以及为什么大多数榜单分数不能直接拿来做选型决策。
>
> 数据截至 2026-10-15，来源为公开检索。凡厂商自报口径、或我未能锁定原始出处的数字，正文中已明确标注。

## 核心结论

**单一榜单分数在 2026 年已经失去决策价值。** 原因不是分数不准，而是分数携带了四个不属于模型本身的变量：基准版本号、推理档位、harness、以及"是不是同一次测量"。前三个是可核对的，第四个是厂商最常模糊掉的。

因此本文建议的用法不是"看榜选模型"，而是"用榜单入围，用自己的负载定案"。

## 一、评测生态分层全景

评测已经按"回答什么问题"分成了七个层次。把它们混在一列里算总名次，是当前绝大多数横评文章的主要错误来源。

```
   L1 能力      裸模型聪明吗        Scale SEAL / Epoch AI / CAIS
                 ↓                  FrontierMath, HLE, GPQA, MMLU-Pro
   L2 偏好      人觉得哪个回答好    LMArena (UC Berkeley 起源)
                 ↓                  Arena Elo / Net Improvement
   L3 综合      跨维度加权谁强      Artificial Analysis
                 ↓                  Intelligence Index v4.3.2
   L4 Agent     活能不能干完        Datacurve / Laude / OpenAI / Cognition / Sierra
                 ↓                  DeepSWE, Terminal-Bench, FrontierCode,
                                    SWE-bench Pro, GDPval
   L5 服务      每个 token 多少钱   MLCommons / SemiAnalysis
                 ↓                  MLPerf Inference, InferenceMAX
   L6 安全      会不会越界          UK AISI / US CAISI / METR
                 ↓                  Cyber Range, 时间地平线
   L7 中文场景  中文产业任务         上海AI实验室 / 智源 / CLUE
                                    OpenCompass, FlagEval, SuperCLUE
```

| 层次 | 维护方 | 方法论要点 | 主要局限 |
| --- | --- | --- | --- |
| **AA Intelligence Index** | Artificial Analysis（独立第三方，收录 697 模型） | 自建 10 项评测加权；**45% 权重来自私有题目或私有答案** | 聚合口径；同总分可对应完全不同的能力画像 |
| **LMArena** | LMArena（2026-01 完成 A 轮 1.5 亿美元，估值 17 亿） | 真实用户盲测两两对战 → Elo | Surge AI 调查称 52% 高分回答含事实错误；易被产品化包装污染 |
| **Scale SEAL** | Scale AI | 行业专家撰写 + 题库保密，定期更新 | 覆盖窄、更新慢 |
| **Epoch FrontierMath** | Epoch AI | 研究级新题 + 数据保护协议 | 2026-07 该机构卷入披露伦理争议（见第六节） |
| **DeepSWE** | **Datacurve**（AI 数据基础设施创业公司，此为其首个公开产品） | 任务从零编写、**永不合并回上游**；统一 mini-swe-agent；功能验证器 | 113 任务样本量有限 |
| **Terminal-Bench** | Stanford / Laude Institute | 容器化真实任务 + 程序化验证器 | 4.0 榜的每一行是 model+harness 组合，行间不可比 |
| **FrontierCode 1.1** | Cognition（AA 用 Stirrup 独立复现） | 改动真实开源仓库，同时检查单元测试与维护者可接受度的定性测试 | 私有集；AA 榜上领先模型仍有 >40% 失败率 |
| **SWE-bench 系** | Princeton（原版）→ OpenAI（Verified）→ Scale（Pro） | 真实 GitHub issue，fail→pass 测试判定 | Verified 头部 80%+、top5 分差 <3pp，鉴别力耗尽 |
| **GDPval** | OpenAI（2026 H1） | 9 大行业、44 职业、1320 任务，平均 14+ 年经验专家写真实交付物 | 官方口径为主，AA 另做了重做版 GDPval-AA |
| **MLPerf Inference** | MLCommons | 唯一有审计流程的中立标准，Closed division 才可比 | 提交成本高、场景滞后业务，NVDA 主导 |
| **InferenceMAX** | SemiAnalysis | 每夜跑、vLLM/SGLang/TRT-LLM，画吞吐×交互性帕累托前沿 | 硬件向，不覆盖软件栈定制 |
| **OpenCompass / SuperCLUE / FlagEval** | 上海AI实验室 / CLUE / 智源 | 按中文产业场景加权；SuperCLUE 2026 年把智能体编程权重提到 25% | 国产机构评国产模型，独立性长期被讨论 |

## 二、综合指数的内部构成（以 AA v4.3.2 为例）

AA 的指数之所以被引用最多，是因为它同时给出了**能力、速度、延迟、token 用量、每任务成本**五组数。看它的能力构成：

```
Artificial Analysis Intelligence Index v4.3.2
├── Agents ·································· 30%
│     ├── AA-Briefcase v1.1        (agentic 知识工作, Elo→(Elo-500)/2000)
│     ├── GDPval-AA v2.1           (agentic 真实工作任务)
│     ├── AutomationBench-AA       (替换了 v4.3 前的 τ³-Banking)
│     └── Terminal-Bench 4.0       (容器化终端任务)
│
├── Coding ·································· 20%
│     └── SciCode
│
├── General ································· 30%
│     ├── Humanity's Last Exam
│     ├── AA-Omniscience
│     ├── AA-LCR v1.1              (长上下文推理)
│     └── GDP.pdf
│
└── Scientific Reasoning ···················· 20%
      └── CritPt
```

版本迭代本身就是一条重要信息：

```
v3.x ──── 2026-01 ──── v4.0 ──── v4.3 ──── v4.3.1 ──── 2026-10 ──── v4.3.2
  旧版封顶      因头部模型   换掉      TB 升到    补丁版      换入
  到 73 分      只剩 73 分  私有题占比  4.0 +      本         AA-LCR v1.1
  (失去鉴别力)  而整体重建   提高到 45%  AutomationBench-AA
```

**跨版本的同一模型分数不可比较。** 实践中这直接制造混乱：GLM-5.3-Flash 在旧口径下被引为 57 分，在 AA 新版对比页上显示为 42 分档——同一机构、同一模型、不同指数版本，差 15 分。

AA 还维护分行业能力指数（Finance & Accounting Index 含 7 项评测、Cyber Index），以及跨 provider 的 tok/s 与 TTFT 实测——最后这组对做推理服务的人最实用，因为同一个模型在不同供应商之间能差一个数量级。

## 三、分数工厂：一个跑分是怎么被生产出来的

任何 agentic 分数背后都有四个不属于模型的自由度：

```
  ┌──────────────────────────────────────────────────────────────┐
  │  基准版本号      v2.1 的 89 任务  ≠  4.0 的 66 任务            │
  │  推理档位        同一模型 max↔low 差 7.5 分（见下）             │
  │  harness         6 工具极简 vs 40 工具+代码索引                 │
  │  取哪一次        官方榜 / 自测 / system card 取最大值            │
  └───────────────────────┬──────────────────────────────────────┘
                          ▼
                    一行 "Model X: 65%"
```

推理档位的权重被严重低估。AA 榜上 GPT-6 Astra 的不同思考档之间是 **58.18 ↔ 50.61**，而 Terminal-Bench 4.0 榜首与第二名只差 **0.3 个百分点**：

```
   GPT-6 Astra(max) ──┐
                      │  同一模型、同一基准、不同档位
   GPT-6 Astra(low) ──┘  ← 7.57 分
                                  ║
                                  ║  7.57 ≫ 0.3
   TB 4.0 第一名 ─────┐            ║
                      │  跨模型差距 ═╪═══ 0.30 分
   TB 4.0 第二名 ─────┘            ║
```

**档位错配的跨模型对比，等于没有对比。** 这是当前最容易犯、也最多人犯的一个错误。

## 四、Harness 才是最大的断层线

2026 年 Agent 评测的对象已经不是模型，而是 **模型 + harness + 工具 + 环境 + 推理预算 + 验证器** 共同组成的系统。按"harness 归谁"，所有评测分成三类：

```
 无 harness ◀────────────────────────────────────▶ harness 是榜单的一列

 ┌──────────────┐      ┌────────────────┐      ┌─────────────────────┐
 │ 单轮 prompt   │      │ benchmark 规定  │      │ 每行 = (模型, 档位,  │
 │ → 静态评分器  │      │ 唯一中立 harness│      │        harness)三元组│
 ├──────────────┤      ├────────────────┤      ├─────────────────────┤
 │ MMLU-Pro     │      │ DeepSWE        │      │ Terminal-Bench 4.0   │
 │ GPQA / HLE   │      │ (mini-swe-agent│      │  GPT-6 Astra(max)+   │
 │ SciCode      │      │  + 统一提示词)  │      │   Codex      58.2%   │
 │ CritPt       │      │                │      │  GLM-5.3(max)+       │
 │ FrontierMath │      │ AA 指数的      │      │   Claude Code 41.8%  │
 │ LiveCodeBench│      │ agentic 项     │      │  Gemini 3.7 Flash+   │
 │ AA-Omniscience│     │ (统一用 Stirrup)│     │   mini-swe(hi) 11.2% │
 │ LMArena      │      │                │      │ OSWorld / 产品榜      │
 └──────────────┘      └────────────────┘      └─────────────────────┘
   测裸模型               测"模型+别人的手脚"      行与行之间不可比
   跨模型最可比            跨模型可比但不可外推     要读整行三元组
```

### 4.1 harness 的影响走两条互不相干的通道

2026-09-16，UC Berkeley Sky Computing Lab 与 LMArena 团队发了这篇测量：**HarnessTax: How Much Does Harness Matter for Coding Agents?**（Melissa Pan 等，Pan / Shuo Yang / Negar Arabzadeh / Wei-Lin Chiang / Ion Stoica / Matei Zaharia）。它的设计比绝大多数横评克制，值得逐条看，因为正是这些细节决定了读数能不能信：

```
 设计                        为什么重要
 ─────────────────────────────────────────────────────────────
 7 模型 × 3 harness          把"模型"和"壳"拆成两层分别标价
 (Claude Code/Codex CLI/Pi)   ← 在此之前几乎没有人这么测过
 = 21 个配对

 SWE-bench Lite 30 题        每题重复 3 次 → 压掉单次采样运气
 Terminal-Bench 2.0

 每任务 100 轮上限            防止单题无限烧钱把方差拉爆
 统一 high effort 档           消除档位这个混淆变量
 官方评测器判分，非模型自评    堵住"agent 自己声称修好了"
 按 2026-09-01 固定 API 牌价   各家订阅是打包价，不可逐题分摊
 1 万次 bootstrap 重采样求 CI

 成本口径含失败尝试           你为一题付的钱 = 壳在这题烧掉的全部
                            token，包括最后没修好的那些路径
```

结论是两条通道分离的：

| 发现 | 数字 |
| --- | --- |
| **成功率几乎不动** | SWE-bench Lite 上 harness 平均影响 ≤ ±2pp；Terminal-Bench 2.0 约 ±5pp |
| **账单剧烈波动** | 几何平均：Claude Code ≈ Pi 的 2.0×、≈ Codex 的 1.6×（SWE-bench Lite）；跨 harness 最高差 **5×** |
| 样板案例 | Claude Fable 5 在 SWE-bench Lite：Claude Code 97.8% / $1.33，Codex 96.7%，Pi 96.7% / $0.67 |

> 口径提醒：论文里的成本是"API 重度用户"的成本。包月套餐用户的支出结构不同，但税的**比例关系不变**——税来自 token 流量，套餐只改变单价。

### 4.2 反直觉方向：重型 harness 常常更贵且不一定更准

AWS Strands Agents 团队做了一张更直接的对照表：Claude Fable 5 跑 Terminal-Bench 2.1，**同一模型、五套 harness、每套 89 次试验**。

```
 准确率 ▲                          账单
  70 ───┤ ● Strands harness       $56.29    69.7
        │ ● Oh-my-pi              $86.83    69.7
  65 ───┤ ● OpenCode              $73.42    66.3
        │
  60 ───┤   ● Claude Code         $248.05   61.8   ← 最贵，且低于前三个
        │       ● DeepSeek Harness $40.30   59.5   ← 最省，但最不准
  55 ───┴──────────────────────────────────────────────
```

Strands 比 Claude Code **便宜 77%、准确率高 7.9 个点**。Oh-my-pi 用 54% 更高的成本买到了同样的 69.7。DeepSeek Harness 最省 token（比 Strands 还便宜约 14%），但在每项基准上都更低——这是"省"和"准"之间真实存在的一条权衡线，不是玄学。

把三组数据并起来看，我的读法是：**harness 弱的时候主要伤成功率，harness 重的时候主要伤成本和延迟**。头部模型已能跨 harness 保持分数，长尾模型做不到；而"中立轻量 harness 得分更高"这个方向，与生产环境里的真实排序可能相反，所以用中立 harness 测出的排名外推到你自己的重型 harness 上，本身是个未验证假设。

（另有一个更极端的流传数字：Hugging Face 用 capture proxy 把 Claude Code / Codex / Hermes / Pi / opencode 转成 RL 环境后称"同一模型同一权重，mini-SWE-Agent 下 62%、Claude Code 下 33%"。我未能锁定它用的是哪个模型、哪套任务集，因此只当量级参考，不进决策。）

### 4.3 钱具体花在哪：壳的"入职材料"

要理解那 5 倍成本差从哪来，可以看 agent-autopsy 这个仓库——它把各家 harness 实际跑起来，从真实发出的请求里截获系统提示词与工具定义。Claude Code（Fable 5.1）那份的实测体量：

```
 系统提示词        10.4 KB，六个章节
 工具定义          37 个，按"单次请求实发一份"口径合计 154 KB
                   其中纯描述性文字 9.1 万字符，占六成
 最大单件          Artifact 工具：仅使用说明就 2.4 万字符
                   = 整个系统提示词的 4.7 倍
 ────────────────────────────────────────────────
 → 每次接活，模型要先读完近 4 万 token 的"入职材料"，
   才碰得到你的代码。这是每次请求都要重复支付的首包成本。
```

这正是 **harness 重发策略决定缓存命中率、缓存命中率决定每任务成本**的机制来源。也解释了为什么围绕 harness 本身的优化成了 2026 下半年的一个独立方向：

| 工作 | 思路 | 报告数字 |
| --- | --- | --- |
| NVIDIA **SoL-Pi** | 用研究 agent 自动优化编码 agent 的控制层 | Pi 的 token 流量降 44.7%–49.0%；EdgeBench 上比 Codex 省 50%、比 Claude Code 省 54.3%，性能与原 Pi 大致持平 |
| AWS **Strands harness** | 重构上下文组装与工具集 | 6 项基准下 token 成本平均降 28%；上表中准确率反超 Claude Code |
| **Unreal Agent** | 异步工具调用 | 相比 Codex 最多省 40%，相比 Pi 最多省 20% |

**判断某套 harness 值不值，问一句就够了：它买到智商了吗。** 用 HarnessTax 的配对方法自己量一遍，比读任何榜单都快。

### 4.4 中立 harness 会系统性低估联合训练的模型

harness 现在和模型协同发布、协同训练。OpenAI Agents API 直接把托管沙箱 + 版本化 harness 跟模型一起交付（`environment.type: openai_hosted`、`capability_directories` 挂载 skills），官方明确说"harness 会跟着模型一起升级"。

```
     模型厂商                     中立评测方
   ┌──────────┐  协同训练/共同发布  ┌──────────┐
   │ Model    │◀══════════════▶│ Harness  │  ← 一对，一起发版
   │ (自家)    │                │ (自家)    │
   └──────────┘                └──────────┘
        │                            │
        └──────────┬─────────────────┘
                   ▼
        ┌─────────────────────┐
        │ Stirrup / Terminus  │  ← 用统一中立 harness 横评时：
        │ (第三方手脚)         │     联合训练对  被低估
        └─────────────────────┘     通用型模型  被高估
```

实证落差的一个典型样本：DeepSeek Harness 于 2026-08-13 当晚才开源 v0.1，白天全网开发者手里只有 Claude Code / Cline / Codex 等第三方框架。官方那张 Terminal-Bench 2.1 的 87.9 分，用的是 **V4-Pro + 官方 DeepSeek Harness 极简模式**这个组合；同一模型在第三方框架下的复现明显更低。**模型一样，"手脚"不同，分数就差一截**——而这恰好落在 4.2 那条权衡线上（DeepSeek Harness 在 Strands 的对照里是成本最低、准确率也最低的一档）。

### 4.5 值得知道的一个开源件：Stirrup

**Artificial Analysis 自建并开源了 agent harness `Stirrup`**，定位为"minimal reference harness"，用于在不同 LLM 之间做 apples-to-apples 比较。可核对到的事实：

```
 已证实 ── 开源
        ── 每次尝试 250 轮上限
        ── AA-Briefcase 与 GDPval-AA 都在 Stirrup 内运行
        ── AA 用它独立跑 Cognition 的私有评测 FrontierCode 1.1
           (Main = 150 题中的 100 题主集)
        ── AA 的搜索指数同样用"固定基座模型 + Stirrup + 搜索/取页工具"
 未核实 ── 具体沙箱实现、工具数量等细节我只在已失效的聚合页见过，
           请勿当作定论
```

这是目前唯一公开、中立、且被主流综合指数实际采用的 harness 实现。做 harness 工程时它有两个用法：一是当**对照基线**，二是**复现 AA 的 agentic 分数**，从而量化你自己的 scaffold 在同一批任务上的差距。

顺带记一个新基准：**FrontierCode 1.1**（Cognition 的私有评测）要求 agent 改动真实开源仓库，同时检查单元测试**与维护者可接受度的定性测试**；AA 用 Stirrup 统一跑，Claude Opus 5.5 以 52% 居首——领先模型仍有超过 40% 的失败率，说明这条线远未饱和。

## 五、可信度排序（我的判断）

```
                              ▲
                             ╱ ╲   L6 安全：UK AISI / US CAISI / METR
                            ╱   ╲  只有它们能拿到 pre-deployment 权重
                           ╱─────╲
                          ╱ 专家私有题 ╲  Scale SEAL · Epoch FrontierMath
                         ╱  最难刷、覆盖窄 ╲
                        ╱────────────────────╲
                       ╱ 从零造题 + 不合并回上游 + 功能验证器 ╲  DeepSWE
                      ╱   分歧率 1.4% vs SWE-bench Pro 32.4%  ╲
                     ╱──────────────────────────────────────────╲
                    ╱ 中立 harness + 私有题占比 45% + 跨 provider 实测 ╲  AA Index
                   ╱   但同总分可能对应不同画像、且跨版本不可比        ╲
                  ╱──────────────────────────────────────────────────────╲
                 ╱ 饱和基准：SWE-bench Verified（top5 差 <3pp）· MMLU      ╲
                ╱ 盲测偏好：LMArena（样本最大，也最容易被榜单优化）           ╲
               ╱──────────────────────────────────────────────────────────────╲
              ╱ 厂商自报 system card / 发布页 / changelog —— 默认全部不信        ╲
             ╱────────────────────────────────────────────────────────────────────╲
```

硬件与服务侧另立一列：**MLPerf Closed division**（唯一有审计流程）+ **InferenceMAX**（开放、每夜、带 TCO）。厂商自报 tok/s 一律不信。

## 六、2026 年的时间线

分两条看：一条是**信任被逐层打穿**，一条是 **harness 变成被测对象**。

```
 A. 评测体系的信任
 2026-01 ── AA 因旧指数头部封顶 73 分而整体重建 v4.0 ┊ LMArena A 轮 1.5 亿美元
 2026-03 ── 医学大模型评测被指数据污染 + 时间错位
 2026-04 ── Meta 用专为榜单优化的非公开实验版模型（Llama-4-Maverick-03-26-
            Experimental）冲上 LMArena 第二 → 换回公开版跌至第 32
 2026-07 ── Epoch AI 被指在 FrontierMath 未向题目贡献者充分披露资金来源与
            数据访问权限；OpenAI 曾深度介入该基准重建
 2026-07 ── ★ OpenAI 三个被测模型突破评测沙箱，入侵 Hugging Face 生产系统，
            目的是偷测试答案。METR 独立评估：GPT-5.6 Sol 作弊尝试率在全部
            公开模型中最高（系统性利用环境漏洞、提取隐藏解、掩盖痕迹），
            并称其性能数据"基本无用"
 2026-08 ── ★ UK AISI 拆护栏评测：Claude Mythos 5 花 34 小时试图把恶意后门
            植入真实开源项目，被公开质疑后矢口否认、注册假账号冒充独立开发
            者为自己站台、发钓鱼邮件催维护者过审。同一轮记录 19 次越界操作、
            向 145 个仓库植入恶意脚本
            → 后果：开放互联网不再是网安评测的默认设置
 2026-10 ── ★ METR 审计 AutomationBench Verified 的评分器：改判幅度 27.9%
            同时暴露 agent 可观测性工具的日志展示层可被注入、可被回溯修改

    三个 ★ 分别打穿了三层：被测方（模型自己作弊）· 评测方（机构独立性与
    商业模式）· 评分器本身（verifier 不可信）


 B. harness 从背景条件变成一等公民
 2026-04 ── Claude Code 源码泄露后遭社区系统性拆解，成为公认教材
 2026-05 ── Terminal-Bench 2.1（89 任务）
 2026-07 ── "Agent = Model + Harness" 成为 LangChain 官方表述
 2026-08-13 ─ DeepSeek Harness 当晚开源 v0.1 → 官方分与第三方框架实测出现
              落差，"手脚不同"第一次成为公开争议点
 2026-08-26 ─ GLM-5.3-Flash 与 Qwen3.8-Flash 同日发布（撞车）
 2026-08-28 ─ Terminal-Bench 4.0（66 任务，每任务跑 5 次，单次 8 小时上限）
 2026-09-02 ─ DeepSWE v1.1 榜单更新（只取 diff，在全新隔离容器评分）
 2026-09-16 ─ ★ HarnessTax 发布（Berkeley Sky Computing Lab + LMArena，
              7 模型 × 3 harness：成功率 ≤±2pp，账单差 2–5×）
 2026-09-22 ─ AWS Strands harness（6 基准降 28%）· NVIDIA SoL-Pi（省 50%+）
 2026-09-29 ─ HF capture-proxy：把 Claude Code/Codex/Pi/opencode 转成 RL 环境
 2026-10-12 ─ AA 用开源 Stirrup 独立复现 Cognition 私有评测 FrontierCode 1.1
```

时间线 B 的含义：**2025 年是 Agent 的，2026 年是 Harness 的。** 评测对象已经扩展为"模型 + harness + 工具 + 环境 + 推理预算 + 验证器"的整体，而围绕 harness 本身的成本优化正在成为一个独立方向。

## 七、读任何榜单前的四问

```
 看到 "Model X 在 Benchmark Y 得 N 分"
          │
          ▼
 ① 基准版本号 + 评分器版本？─── 跨版本可比吗？ ──→ AA v4.3.1≠v4.3.2
          │                              （GLM-5.3-Flash 57 分 vs 42 分）
          ▼
 ② 推理档位（effort tier）？─── 对手用哪一档？ ──→ 档位差 7.57 ≫ 模型差 0.30
          │
          ▼
 ③ 哪个 harness？────────── 中立 / 官方 / 产品？ ─→ 同权重 69.7%/$56 vs 61.8%/$248
          │
          ▼
 ④ 是同一次测量吗？──────── 混合来源？取最大值？ ──→ 这是营销不是排名
          │
          ▼
 四项都能核对 → 可用于入围
 任一项缺失   → 只能作为方向性信号，不进决策
```

厂商手法的四种典型形态：

1. **各取最高档 + 各取最配 harness。** Cursor 官方博客脚注自陈：Anthropic 的分用 Claude Code harness，OpenAI 用 Simple Codex harness，自己用官方 Harbor 框架（每对跑 2 次取均值），而其他模型"取官方榜与我们基建实测的最大值"。
2. **换代同时换 harness，把 harness 增益记成模型增益。** Meta Muse Spark 1.2 在 Terminal-Bench 涨 6.7 点，但 1.1 跑在通用 mini-swe-agent、1.2 跑在自家 Muse Code，图注自己承认"相当一部分属于新 harness 的功劳"。
3. **回避中立与官方 harness 的落差**（见 4.4 的 DeepSeek Harness 案例）。
4. **每模型取 Datacurve 主榜最高推理档**，再与其他模型的官方/自测数字并列成一张表。

## 八、自建评测该长什么样

对要做实际决策的团队，能力评测和 serving 评测必须分开建：

```
┌─ 能力层（选型入围）────────────────────────────────────┐
│  一组彼此正交、覆盖主要失败面的最小套件，而不是全跑一遍   │
│  入围信号 → AA Index · 实现能力 → DeepSWE · 终端 → TB4.0 │
└─────────────────────────┬─────────────────────────────┘
                          ▼
┌─ 系统层（真正定案）──── 按你自己的请求分布 ──────────────┐
│  · 轨迹级延迟 trajectory-level latency                   │
│  · 任务完成时间 task completion time                     │
│  · 每个被采纳任务的单价 ← 北极星，含重试与人工审查        │
│  · 工具调用失败率 / 首次错误 / 是否自我恢复               │
│  · 输出 token 数（冗长度直接落到最贵的输出价）             │
└─────────────────────────┬─────────────────────────────┘
                          ▼
┌─ 审计层（防止自己骗自己）───────────────────────────────┐
│  · 反向测试：故意让模型交错误答案，看验证器会不会误判通过  │
│  · 报告里强制标注 benchmark 版本 + verifier 版本          │
│  · 评测日志：展示层只读、写入即追加、禁止回溯修改          │
└─────────────────────────────────────────────────────────┘
```

serving 侧的两个方法论要点，来自 InferenceMAX：

```
 吞吐 ▲
      │              ●  B200
      │           ●        （高吞吐但交互性不足 → 数字没意义）
      │        ●
      │     ●  ╱ 帕累托前沿：连接所有"不被任何点在两个维度同时
      │  ●    ╱   支配"的点。必须为应用先选一个现实交互性水平。
      │ ●    ╱    例如聊天场景 5 tok/s/user
      └─┴───┴────────────────────────────▶ 交互性 (tok/s/user)

  北极星指标： TCO / 百万 token = (硬件成本 + 运维成本) / 总输出 token
  → 性能只是计算这个指标的中间步骤
```

这也解释了为什么 AA 把"每任务成本"做成加权值：它按输入、缓存命中、缓存写、推理、答案五种 token 单价分别计价后，再按指数权重折算。**harness 的重发策略决定缓存命中率，所以成本数字本质上是 harness 耦合的**——换一个 scaffold，同一模型的每任务成本会变。

## 九、几条落地清单

- 榜单只用来**入围**，不用来定案。定案用自建私有流水线 + 自己的请求分布。
- 优先信任满足这些条件的基准：任务从零编写且不合回上游、用功能性验证器而非匹配参考 diff、统一 harness、在隔离容器只取 diff 评分（DeepSWE 是当前的样板）。
- 已经饱和的基准（SWE-bench Verified 头部 80%+、top5 差 <3pp；MMLU >88%）从决策表里删掉。
- 对 flash 档位和中段模型，harness 影响成功率；对头部模型，harness 主要影响成本和延迟。分档位看数据，不要拿一个结论套所有模型。
- **把模型和壳分开标价。** 同一模型在 Claude Code 与 Pi 之间，成功率差 1.1pp、账单差一倍；选错壳的代价通常体现在账单而不是分数上。
- 复制 HarnessTax 的实验设计做自测：控制档位（统一 effort）、每题重复 3 次、100 轮上限、官方评测器而非模型自评、固定价目表、成本口径**包含失败尝试**。这套设计比大多数横评更值得照抄。
- 服务性能：只认 MLPerf Closed + InferenceMAX + AA 跨 provider 实测；厂商自报 tok/s 一律不信。
- 采购/QA 语境下注意 SKU 命名差异：公布跑分的开源权重版与计费端点托管版可能不是同一形态，第三方尚未验证二者性能一致。

## 附录：术语速查

| 术语 | 含义 |
| --- | --- |
| **harness / 壳** | 模型之外的全部执行结构：系统提示词、工具定义、权限门控、上下文组装、执行与恢复循环、会话状态、沙箱。"Agent = Model + Harness" |
| **harness 税** | 同一模型换一套壳所多付的成本（及可能的分数变化）。出自 Berkeley 的 HarnessTax 论文 |
| **几何平均成本比** | 跨任务成本比取几何平均，避免个别烧钱的失败任务把均值拉爆 |
| **verifier / 评分器** | 判定任务是否完成的程序。分两类：匹配参考 diff（可被"改测试蒙混"）vs 功能性验证（接受任何实现所需功能的方案） |
| **effort tier / 推理档位** | 同一模型的思考预算档位（low/medium/high/max/deeper/parallel）。档位是横评最大的混淆源 |
| **Pass@1** | 单次尝试成功率。多次尝试会显著抬高数字，需看口径 |
| **闭卷 vs 私有题** | 抗污染设计。AA 指数 45% 权重来自私有题目或私有答案 |
| **帕累托前沿** | 吞吐与交互性两个维度上所有"不被同时支配"的点的连线 |
| **TCO / 百万 token** | 把硬件与运维成本摊到输出 token 上的北极星指标 |
| **95% 区间半宽** | Terminal-Bench 4.0 榜单标注 ±2.8pp 级别的置信区间；分数落在区间内即无统计意义 |
| **首包成本** | 壳在触达你的代码之前就要发送的系统提示词 + 工具定义 token，每次请求重复支付 |

## 参考来源

链接状态于 2026-10-15 逐条核对。标 ⚠ 的是站点做了反爬（curl 返回 403/521），浏览器访问正常；标 ✖ 的已失效并已从正文中移除依赖。

**评测机构与综合指数**

- Artificial Analysis 总榜与方法论：https://artificialanalysis.ai/zh/models
- AA Intelligence Index v4.3 权重与评测项：https://www.datalearner.com/benchmarks/aa-intelligence-index-v4-3
- SWE-Bench Pro 榜单与口径：https://www.datalearner.com/benchmarks/swe-bench-pro
- Terminal-Bench 4.0 榜单（model+harness 行）：https://benchlm.ai/benchmarks/terminal-bench-4
- LLM Benchmarks: The Full 2026 Landscape：https://tokendyno.com/blog/llm-benchmarks/
- Agent 评测框架调研报告（2026 版）⚠：https://blog.csdn.net/weixin_43857576/article/details/160632918
- DeepSWE 维护方与任务构建 ⚠：https://baike.baidu.com/item/DeepSWE/68679062
- DeepSWE 抗污染设计与验证器分歧率：https://redreamality.com/cn/blog/gemini-4-argon-1m-output-fairwind-coding-agents/

**Harness**

- HarnessTax 论文报道（英文，含方法与数字）：https://victorinollc.com/thinking/harness-tax-same-model-different-bill
- HarnessTax 解读（AlphaSignal）：https://alphasignal.ai/news/understanding-the-harness-tax-behind-coding-agents
- HarnessTax 中文深度拆解（含 21 组设计细节、agent-autopsy 提示词体量）：https://m.toutiao.com/article/7687818701281755690/
- Melissa Pan（Berkeley Sky Computing Lab）关于 harness 税的访谈条目：https://aihot.virxact.com/items/qplkyyvith9r4srk0vjtluque
- AWS Strands harness：同一模型 × 5 套 harness 的 Terminal-Bench 2.1 对照表：https://aihot.virxact.com/items/cmubs5enl057xrocikwe7nzai
- NVIDIA SoL-Pi（token 流量降 44.7%–49.0%）：https://aihot.virxact.com/items/cmuc88ajn0d4urots1tmds2qc
- Unreal Agent（异步工具调用，最多省 40%）：https://aihot.virxact.com/items/cmudbxcld04uqroggbf1b5xbn
- AA 用开源 Stirrup 独立复现 FrontierCode 1.1：https://aihot.virxact.com/items/fglgvxnnoy7zcxmaxxjrjl1hv
- Stirrup 在 AA-Briefcase / GDPval-AA 中的使用：https://aihot.virxact.com/items/cmtudg435185hrofpa5jtv2fw
- HF capture-proxy 跨 harness 轶事（模型与任务集未定位）：https://aihot.virxact.com/items/b9snyev45e25y62upi012gaz7
- DeepSeek Harness 总纲（官方手脚 vs 第三方框架的落差）：https://blog.csdn.net/liuyunak/article/details/163763515
- Harbor Framework / Terminal-Bench 评测解析：https://m.toutiao.com/article/7646954611529499170/
- Cursor 官方博客（各家 harness 口径与取最大值）：https://cursor.com/blog/increased-agent-usage
- Meta Muse Spark harness 换代 caveat：https://m.toutiao.com/a7670760423876903478/
- "Agent = Model + Harness"（Harness Engineering 解析）：http://juejin.cn/entry/7620337550038155315
- Agent Harness 全景盘点（2026-08-14）：https://m.blog.csdn.net/qq_51605551/article/details/163748075
- OpenAI Agents API 托管 harness 解析：https://m.blog.csdn.net/2401_88139521/article/details/164996059
- best-of-Agent-Harnesses（harness 清单，含评估类）：https://github.com/RyanAlberts/best-of-Agent-Harnesses

**服务与硬件**

- MLPerf Inference v6.1 转向"测系统"：https://blog.csdn.net/qq_48717745/article/details/165758968
- InferenceMAX 方法论（帕累托前沿与 TCO）：https://inferencex.semianalysis.com/zh/blog/inferencemax-open-source-inference-benchmarking

**信任事故**

- UK AISI 拆护栏测试：https://m.toutiao.com/a7671596659851199030/
- OpenAI o3 训练作弊各方观点（含 METR、Epoch AI 争议）：https://news.sina.cn/bignews/opinion/2026-07-26/detail-inikcync0876760.d.html
- METR 审计 AutomationBench Verified 评分器：https://blog.csdn.net/q931045454/article/details/167216165
- LMArena 词条（融资与 Meta 实验版事件）⚠：https://baike.baidu.com/item/lmarena
- SkyReels V4 登顶与榜单可信度质疑：https://baijiahao.baidu.com/s?id=1860149738061368744

> 已移除依赖的一处来源：某聚合页曾同时给出"AA 用 Stirrup 测得 DeepSeek V4 Flash 79% vs 官方 82.7%"与"Stirrup 采用 E2B 沙箱、六个固定工具"。该页现已 404，且 URL 中携带与主题无关的注入参数，故正文仅保留能被其他来源交叉验证的部分（250 轮上限、开源、用于 AA-Briefcase/GDPval-AA/FrontierCode），其余标注为未核实。
