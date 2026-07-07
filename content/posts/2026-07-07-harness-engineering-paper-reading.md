---
title: "Harness Engineering for Self-Improvement 精读"
date: 2026-07-07
draft: false
tags: ["论文精读", "Agent", "Harness", "AI趋势", "AI自进化", "持续学习", "Coding"]
categories: ["paper-reading"]
summary: "Lilian Weng（Thinking Machines Lab 联合创始人、前 OpenAI 研究副总裁）在这篇万字综述中系统梳理了「Harness 工程」——围绕基础模型的运行时系统——作为通往递归自我改进（RSI）现实路径的核心命题。文章从 RSI 的思想起源讲起，把 Harness 定义为决定模型如何思考、规划、调用工具、管理上下文、评估结果的系统层，并梳理了三大设计模式（工作流自动化、文件系统持久记忆、子代理并行）、四大优化方向（上下文工程、工作流设计、自我改进、进化搜索）以及与模型权重的联合优化，最后坦诚列出七大瓶颈。本精读将这篇综述放在 RSI→Harness 的研究脉络中定位，提炼其方法论骨架与可迁移的普适灵感。"
---

> **文章链接**：[Harness Engineering for Self-Improvement — Lil'Log](https://lilianweng.github.io/posts/2026-07-04-harness/)
> **发表时间**：2026 年 7 月
> **作者**：Lilian Weng（Lil'Log 博客作者）
> **作者背景**：Thinking Machines Lab 联合创始人（2025.1 至今）；曾任 OpenAI 研究副总裁（Safety）、Head of Safety Systems、Head of Applied AI Research；早期从事社交网络信息扩散、机器人灵巧操作（Rubik's Cube 机器人手）、嵌入表示等研究
> **文章性质**：综述博客（非会议论文），引用 35 篇文献，含 6 个基准测试附录

---

## 一、论文背景：从"超智能机器"到"模型外面的那一层"

### 1.1 一个延续六十年的梦想：递归自我改进（RSI）

要理解这篇文章，先得理解它试图回答的那个老问题：**机器能不能让自己变聪明？**

这个想法的最早形态可以追溯到 **I. J. Good 在 1965 年**提出的"超智能机器（ultraintelligent machine）"——一台在所有智力活动上超越人类的机器，并且能够设计出比自己更聪明的机器。**Eliezer Yudkowsky 在 2008 年**给这个反馈循环起了一个今天更常用的名字：**递归自我改进（Recursive Self-Improvement, RSI）**——AI 利用自己当前的智能，去改进"产生这份智能的那个认知机制"。

把这句绕口的话翻译成画面：

> 一个学生通过学习变聪明了，然后用更聪明的脑袋去研究"怎么学习"这件事本身，于是下一轮学得更快、更深——如此循环，每一轮的"学法"都比上一轮更好。

这就是 RSI 的核心：**改进的对象不是知识，而是"产生知识的能力"**。

### 1.2 RSI 在现代 AI 里的三种现实形态

过去讲 RSI，常常联想到"模型自己重写自己的权重"——这是最戏剧化、也最遥远的一种。搜索结果与本文都指出，现实中的 RSI 路径其实有三个层次：

| 层次 | 改进对象 | 机制 | 可观测性 |
|------|---------|------|---------|
| **第一层：脚手架层改进** | 围绕模型的"外壳系统"（harness/脚手架） | 更好的编排 → 更好的任务分解 → 更好地调用已有认知 | 已经在编码代理上被观察到 |
| **第二层：研发层改进** | 训练流程 / 部署系统 | AI 帮助设计架构、调参、跑实验，压缩研发周期 | 受算力、数据、组织约束，较慢 |
| **第三层：模型内部自修改** | 模型权重 / 认知架构 | 经典"foom"设想，模型理解自身后重写自身 | 尚无现实路径 |

Lilian Weng 在文章开头明确表态：**"近期的 RSI 路径，不太可能从模型直接重写自身权重开始。"** 她把目光聚焦在第一层——也就是本文的主角 **Harness**。

### 1.3 关键概念：Harness 是什么？

一句话定义（来自原文）：

> **A harness is the system surrounding a base model that orchestrates execution and decides how the model thinks and plans, calls tools and acts, perceives and manages context, stores artifacts, and evaluates results.**

翻译过来：Harness 是**围绕基础模型的那一层系统**，它负责编排执行，并决定模型**怎么思考、怎么规划、怎么调用工具、怎么行动、怎么感知、怎么管理上下文、怎么存储工件、怎么评估结果**。

用一个类比来建立直觉：

- **模型（Model）** 像一个**能力很强但什么都看不见的大脑**——它只能接收文本、输出文本。
- **Harness** 像这个大脑外面的**一整套"身体 + 办公环境"**：眼睛（读文件）、手（调工具、跑代码）、记忆本（文件系统）、同事（子代理）、KPI 考核（评估器）。

大脑再聪明，如果没有身体和环境，它也做不出任何"现实世界"的事。**Harness 就是把"只会说话的大脑"变成"能干活的 Agent"的那一层。**

为什么这层如此重要？Lilian Weng 给出了一个关键判断：

> "the layer between the raw model and the real-world context seems to be **as important as the model's raw intelligence** (i.e. the evals right after pretraining)."

也就是说：**模型刚预训练完的"裸智商"，和它真正在现实任务中表现出的能力之间，隔着整整一个 Harness 层；这一层的重要性，和模型本身的智商同等量级。**

这一点和业界共识高度一致——MongoDB 的技术博客把这套逻辑类比成 2015 年 Google 那张著名的"ML 技术债"图：在真实系统里，模型（ML 代码）只是中心一个小黑块，周围数据收集、特征工程、监控、serving 等一大圈才是工程主体。今天的 Agent 系统正在重演这个故事：**LLM 调用只占几行代码，围绕它的 Harness 才是真正的工程量。**

### 1.4 Harness 和早期"Agent 框架"的区别

熟悉 Agent 的读者会问：这和之前大家说的"agent = LLM + 记忆 + 工具 + 规划 + 行动"有什么不同？原文给出了明确递进：

早期 Agent 框架定义：`Agent = LLM + memory + tools + planning + action`

Harness 工程在此基础上**额外包含**：
- **工作流设计**（如 loop engineering）
- **评估**（evaluation）
- **权限控制**（permission controls）
- **持久状态管理**（persistent state management）

也就是说，Harness 已经不再是"几条 prompt 模板"，而是更接近**运行时（runtime）和软件系统设计**：研究"模型如何观察、行动、记忆、自我检查、自我改进"。**Harness 和操作系统有强烈的类比关系**——像 OS 一样，它应该封装复杂逻辑，同时保持对外接口简单。

至此，本文的研究问题自然浮出水面：**既然 Harness 如此重要，那"改进 Harness"本身能不能成为 RSI 的现实入口？如果能，应该怎么改、改到什么程度、又有哪些坑？** 这就是整篇文章要回答的问题。

---

## 二、论文定位与关联工作：Harness 研究的谱系图

这篇文章不是某一篇具体方法的论文，而是**对"Harness 工程作为一个研究方向"的系统性综述与宣言**。要理解它的定位，需要看清它处于哪条研究脉络上，以及它把哪些工作组织进了同一个框架。

### 2.1 谱系一：从"自我改进"思想到代码化的自我改进器

| 工作 | 时间 | 核心思想 | 与本文关系 |
|------|------|---------|-----------|
| **I. J. Good** 超智能机器 | 1965 | 提出能设计更好机器的机器 | RSI 的思想源头 |
| **Yudkowsky** RSI 术语 | 2008 | 正式命名"递归自我改进" | 给本文核心概念命名 |
| **Schmidhuber** Gödel Machine | 2003 | 理论上"可证明有益"的自修改机 | DGM 的理论祖先 |
| **STOP**（Zelikman et al.） | 2023 | 用代码实现"改进器改进自己"的递归脚手架 | 本文重点案例：自我改进 Harness 的早期雏形 |

**STOP（Self-Taught Optimizer）** 是这条线上最关键的早期实验：它用一个"种子改进器" $I_0$ 去改进任意解 $s$，然后用这个改进器去改进改进器自己：

$$I_t = I_{t-1}(\hat{u}, I_{t-1}; M)$$

它的警告性发现尤为重要：**STOP 在 GPT-4 上能持续改进，但在 GPT-3.5、Mixtral 等弱模型上反而退化**——这揭示了一个普适规律：**"递归结构本身不够，基础模型必须足够强，才能改进这个机制。"** 这条结论贯穿全文。

### 2.2 谱系二：上下文工程（Context Engineering）的演化

这是本文最密集的一条方法线，也是 Harness 优化的"前沿战场"：

| 工作 | 机构 | 核心创新 | 优化对象 |
|------|------|---------|---------|
| **Prompt 工程** | 各界 | 写好单条指令 | instruction prompts |
| **ACE**（Zhang et al.） | Stanford + SambaNova + UC Berkeley | 把上下文当作"不断演进的剧本"（evolving playbook），用 Generator-Reflector-Curator 三角色架构做增量更新 | structured context |
| **MCE**（Ye et al.） | 北京大学 | 把"怎么管理上下文的机制"也变成可学习对象，双层优化、技能交叉进化 | 上下文工程技能本身 |
| **Meta-Harness**（Lee et al.） | Stanford IRIS Lab | 直接优化"决定存什么/检索什么/呈现什么"的那段代码 | harness code |

这条线的核心演进逻辑是**优化对象不断"上移"**，原文给出了一条精辟的进阶链：

> instruction **prompts** → structured context → workflow → harness code → optimizer code

也就是说：随着模型变强，我们优化的目标从"一句话怎么说"，一路上升到"整个系统怎么跑"。

**ACE** 的关键设计是**拒绝重写整个 prompt 块**——因为反复重写会丢失细节（context collapse）和偏向简洁（brevity bias）。它只输出结构化的 `(identifier, description)` 增量条目，确定性合并。这是一个很有迁移价值的设计哲学：**有界更新优于无界重写**。

**MCE** 进一步把 ACE 里"手工写死的更新规则"也变成可进化对象——一个 skill $s$ 定义了上下文函数 $c_s = (\rho_s, F_s)$，meta-agent 通过"agentic crossover（智能体交叉）"在技能历史上搜索更好的技能。

**Meta-Harness** 把优化推到更深：优化对象是"决定和优化应该存什么/检索什么/呈现给模型的代码"。它的命名含义就是——**优化 harness 的 harness**。

### 2.3 谱系三：工作流设计的自动化搜索

| 工作 | 机构 | 方法 | 核心区别 |
|------|------|------|---------|
| **AI Scientist**（Lu et al.） | Sakana AI 等，发表于 Nature | 手工 pipeline：想法→代码→实验→分析→写稿→评审 | 人类设计工作流 |
| **ScientistOne**（Meng et al.） | — | 以"可验证性"为核心，每条声明都要 Chain-of-Evidence 可追溯 | 强调可验证工作流 |
| **Autodata**（Kulikov et al.） | — | 主代理管理 Challenger/弱解/强解/裁判，合成"恰好合适"难度的数据 | 数据合成工作流 |
| **ADAS**（Hu, Lu, Clune） | UBC + CIFAR | 把 agent 设计本身形式化为优化问题，meta-agent 用代码编程新 agent | 自动发现工作流（ICLR 2025） |
| **AFlow**（Zhang et al.） | DeepWisdom + HKUST(GZ) + 人大等 | 把工作流表示为图，节点是 LLM 调用，用 MCTS 优化 | 图搜索工作流（ICLR 2025 Oral） |

这条线的关键判断是原文一句话：**"工作流的设计空间是巨大的，自然地可以把工作流设计看成一个搜索问题。"** ADAS 和 AFlow 分别用"代码归档 + meta-agent"和"MCTS 图搜索"给出了两种自动化答案。

### 2.4 谱系四：进化搜索（Evolutionary Search）

| 工作 | 机构 | 核心机制 | 代表成果 |
|------|------|---------|---------|
| **Promptbreeder** | — | 进化 prompts，变异算子本身也进化 | prompt 自指改进 |
| **GEPA** | — | 反思 + 进化搜索 | prompt 优化 |
| **AlphaEvolve** | Google DeepMind | 冻结 LLM 进化候选程序池，EVOLVE-BLOCK 标记可编辑区 | 4×4 复矩阵乘法 56 年来首次改进 Strassen；数据中心调度回收 0.7% 全球算力 |
| **ShinkaEvolve** | — | 三组件提高采样效率：父代采样、嵌入去重、meta-scratchpad | 进化效率改进 |
| **ThetaEvolve** | — | 进化搜索 + RL + in-context learning | test-time 学习 |
| **Darwin Gödel Machine (DGM)** | Jenny Zhang, Shengran Hu, Cong Lu, Robert Lange, **Jeff Clune** | 明确针对可编辑 harness-code 仓库的进化，LLM 代理可改自己的 harness | SWE-bench 20%→50%，Polyglot 14.2%→30.7%（ICLR 2026） |
| **Hyperagents** | 同组 | meta-agent 控制如何修改任务代理 | DGM 的后续 |

**DGM 是这条线最贴合本文主题的工作**：它让一个 Claude 3.5 Sonnet 驱动的编码代理，用 `bash` 和 `editor` 两个最朴素的工具，**修改自己的 harness 代码仓库**，并在 SWE-bench 上做基准评估，只有性能足够高的版本才被保留进"代理池"。它体现了本文一个核心信念：**一旦 harness 设计变成一个可执行的搜索空间，一个强编码代理就能利用人类工程师使用的同一个设计空间。**

### 2.5 谱系五：自我改进的 Harness（Self-Improving Harness）

| 工作 | 机构 | 机制 | 关键区别 |
|------|------|------|---------|
| **Self-Harness**（Zhang et al.） | **上海人工智能实验室**（Shanghai AI Lab） | propose-evaluate-accept 循环：弱点挖掘→harness 提案→回归验证 | 代理改进**自己**的 harness，不依赖更强外部代理 |
| **SIA**（Hebbar et al.） | — | Meta-Agent + Task-Agent + Feedback-Agent，联合改 harness 或权重 | 早期尝试 harness+权重联合优化 |

**Self-Harness** 的定位特别值得注意：它明确区分了三种范式——人类工程化 harness、**用更强的外部代理改进弱代理的 harness（Meta-Harness）**、**代理自己改进自己的 harness（Self-Harness）**。它在 Terminal-Bench-2 上让 MiniMax M2.5、Qwen3.5-35B-A3B、GLM-5 三个不同家族的模型各自学到**模型特异性**的 harness 指令——说明**同一个 harness 套不同模型效果不同，最佳 harness 是因模型而异的**。

### 2.6 本文在这张谱系图里的位置

把上面五条线合起来看，Lilian Weng 的这篇综述做的是一件"地图绘制"工作：

- **它不是某一类方法的提出者**，而是把分散在 prompt 工程、上下文工程、工作流搜索、进化计算、自我改进等不同社区的工作，**统一收纳到"Harness 工程"这个框架下**。
- **它的核心论点**是：这些看似不同的工作，本质上都在回答同一个问题——**"如何系统性地改进模型外面那一层"**，而这正是 RSI 最现实的近期入口。
- **它的差异化贡献**在于：明确给出了 Harness 的形式化定义、三大设计模式、四大优化方向、与权重联合优化的展望，以及一份诚实的"七大瓶颈"清单——为这个正在成形的领域画了一张可操作的研究地图。

---

## 三、问题定义：Harness 工程要解决的本质问题

### 3.1 从具体场景到抽象问题

文章面对的具体场景很丰富：编码代理（Claude Code、Codex）、科研自动化（AI Scientist、ScientistOne）、数据合成（Autodata）、算法发现（AlphaEvolve）……但如果剥掉场景外衣，**所有这些工作都在解同一个抽象问题**。

### 3.2 抽象问题：把"系统怎么跑"变成可优化的对象

建立一个类比来抓住本质——**Harness 工程 ≈ 把"软件系统"当成一个可被搜索/学习的程序来优化**：

| 深度学习训练 | Harness 工程 |
|------------|-------------|
| 模型参数 $\theta$ | harness 的代码、prompts、工作流、上下文管理策略 |
| 训练数据 | 任务集合 + 执行轨迹（成功/失败） |
| 损失函数 $\mathcal{L}$ | 任务评估函数 $J$（基准分数、验证器通过率等） |
| 梯度下降 | 进化搜索 / meta-agent 编程 / 反思迭代 |
| 训练循环 | 自我改进循环（propose→evaluate→accept） |
| 泛化能力 | held-out 任务上的表现（防止过拟合评估器） |

形式化地说，Harness 工程要解决的问题是：

> **给定**：一个固定（或缓慢变化）的基础模型 $M$、一个任务分布 $\mathcal{T}$、一个评估函数 $J$、一个可编辑的 harness 设计空间 $\mathcal{H}$。
>
> **求**：一个 harness $h^* \in \mathcal{H}$，使得在 $h^*$ 包装下的 $M$ 在 $\mathcal{T}$ 上的期望得分最大化，即
> $$h^* = \arg\max_{h \in \mathcal{H}} \mathbb{E}_{\tau \sim \mathcal{T}}[J(M, h, \tau)]$$
>
> **约束**：评估要可信（避免 reward hacking）、改进不能引入回归（held-in 和 held-out 都不能退化）、长期可持续（不过度工程化）。

### 3.3 这个抽象的精妙之处

这个定义有三个值得品味的点：

1. **它把"模型智商"和"系统能力"解耦了**。同一个 $M$，套不同的 $h$，得分可以差几倍——这就把研究焦点从"炼更大的模型"转移到了"设计更好的外壳"。

2. **它把 $h$ 显式地放进搜索空间**。$h$ 不再是工程师拍脑袋写的固定脚本，而是一个可以被 meta-agent / 进化算法 / MCTS 优化的对象。这是从"手工作坊"到"系统化搜索"的范式跃迁——和深度学习历史上"手工特征→学习特征"的转折同构。

3. **它内置了安全约束**。评估器和权限控制必须位于进化循环**之外**——否则系统会过拟合自己的评分。这个约束不是事后补丁，而是问题定义的一部分。

### 3.4 优化对象的进阶链

原文给出了一条贯穿全文的关键判断：**优化对象在不断"上移"**：

> instruction **prompts** → structured context → workflow → harness code → optimizer code

这条链的含义是：模型越强，我们越有能力去优化更复杂、更通用的目标。最开始的 prompt 工程优化的是"一句话"；上下文工程优化的是"给模型看什么"；工作流设计优化的是"任务怎么分解串联"；harness 代码优化的是"整个系统怎么跑"；而 optimizer code 优化的是"用来优化系统的那个系统本身"——这就摸到了 RSI 的边缘。

---

## 四、问题解法：设计模式、优化方法与联合优化

Lilian Weng 把"Harness 怎么改"拆成了三大设计模式（基础工程）+ 四大优化方向（如何自动化改进）+ 与权重的联合优化。我们逐一拆解，并补全必要参考信息。

### 4.1 三大设计模式（Harness Design Patterns）

这是"Harness 应该长什么样"的基础工程经验。

#### 模式一：工作流自动化（Workflow Automation）

**核心思想**：给模型一个可以在其中操作、测试、迭代的工作流，是自动化的关键。

一个通用的目标导向循环：

```
plan → execute → observe/test → improve → execute again（直到目标达成）
```

过程中可以主动向用户请求澄清。原文以 Karpathy 的 **autoresearch** 仓库作为清晰范例，并展示了简化的 Codex agent loop：代理调用工具 → 工具响应 → 影响模型下一次生成。关键在于：**模型分析自己的轨迹和失败，然后通过 agent runtime（而非静态 prompt 模板）迭代推进**。

**类比**：这就像给一个员工一套"PDCA（计划-执行-检查-改进）"的标准作业流程，而不是每次都临时告诉他怎么做。

#### 模式二：文件系统作为持久记忆（File System as Persistent Memory）

**核心洞察**：长时间跨度的代理，反复出现的模式是"对丰富的状态和工件做简单的文件控制"。

关键原则：
- Harness **不应该**在 context 里携带整个工作流和所有日志；
- 应该在**文件中**保持持久状态；
- 在长跨度 rollout 中，工件（实验日志、代码 diff、论文摘要、错误追踪、历史轨迹）通常**远超模型上下文窗口**。

原文有一句值得记住的判断：

> "Learning how to read, write, and edit the file system (commonly via `bash` commands) is a foundation skill for LLMs."

**类比**：这就像一个研究员不会把所有资料都背在脑子里，而是写进笔记本、存进文件夹——需要时再翻出来。**文件系统就是 Agent 的"外置大脑"。**

#### 模式三：子代理与后端作业（Sub-agent and Backend Jobs）

**核心功能**：Harness 可以生成多个子代理并行执行，并监控后端作业。

应用场景：搜索多个假设、并发跑实验、委托隔离子任务而不污染主上下文。父代理需要一个小的"进程管理器"：启动作业、检查日志、取消失败运行、合并结果回主线。

关键设计选择——**让并行性显式且可检查**：

> "The key design choice is to make parallelism **explicit and inspectable**."

如果子代理输出只存在于临时聊天上下文里，很快会过时和隐藏；如果存为文件、日志、状态记录，模型就能在中断后恢复并推理自己的执行历史。

### 4.2 案例研究：编码代理 Harness 的工具分层

主流编码代理（Claude Code、Codex、OpenCode、Cursor）的核心接口已稳定，工具可分七组：

| 工具组 | 代表工具 |
|-------|---------|
| 文件系统 | `glob`, `grep`, `ls`, `read`, `read_many`, `write`, `edit`, `multi_edit`, `apply_patch` |
| Shell 执行 | `bash`, `PowerShell` |
| IO | `lsp`, `git_status`, `git_diff`, `git_commit` |
| 外部上下文 | MCP tools, Skills |
| Web 搜索 | `web_search`, `web_fetch`, 浏览器工具 |
| 工件 | 读文档/图片，生成 HTML/图片 |
| 后端进程 | `CronCreate`, `CronDelete`, `CronList` |
| 代理委派 | `spawn_agent`, `resume_agent`, `wait_agent`, `list_agents`, `close_agent`, `interrupt_agent` |

这张表的价值在于：它给出了一个**可复用的 Harness 工具分层模板**——任何想搭编码类 Agent 的人，都可以照着这张表查漏补缺。

### 4.3 Harness 层 vs 核心智能：一个重要预判

作者承认很难预测 RSI 未来有多大程度依赖 harness 工程，但给出了一个清晰的近期路径预判：

1. **Harness 工程将向 meta-methodology 演进**——即改进"获得更好答案的机制"，而不仅是改进答案本身。系统本身成为优化目标，更少启发式、更多通用机制。
2. **成熟的 harness 反过来启用 auto-research**——用于模型自我改进循环；更智能的模型防止 harness 过度工程化，保持系统可持续。

最终趋势（原文）：

> "Eventually it is possible that many harness improvements will be **internalized** into core model behavior, but the interface with external context and tools should remain."

这和 prompt engineering 的历史同构：随着 instruction tuning 和模型推理变强，手动 prompt 技巧变得不那么核心——**但"指定目标、约束、上下文和评估"的需求从未消失**。Harness 也会走同样的路：很多技巧会被模型内化，但与外部世界交互的接口层会一直存在。

### 4.4 四大优化方向（Harness Optimization）

这是"如何自动化地改进 Harness"的方法地图。

#### 方向一：上下文工程（Context Engineering）

**问题**：把所有工具响应和模型生成都追加到上下文里，随着代理工作跨度变长会迅速失控。

**ACE（Agentic Context Engineering）** 把上下文当成"不断演进的剧本"而非不断增长的 prompt，三个组件：
- **Generator**：生成任务轨迹；
- **Reflector**：从成功/失败中提炼洞察；
- **Curator**：用增量分项条目更新结构化上下文。

为防止上下文坍塌和简洁偏差，Curator **不重写完整 prompt 块**，而是输出 `(identifier, description)` 形式的结构化条目，确定性合并，定期精炼去重。

**MCE（Meta Context Engineering）** 把"怎么管理上下文（机制）"和"上下文里放什么（内容）"分离。一个 skill $s$ 定义上下文函数 $c_s = (\rho_s, F_s)$，双层优化：

```
Inner: c_s* = arg max_{c_s} J_train(c_s; s)
Outer: s*   = arg max_{s ∈ S}  J_val(c_s*)
```

meta-agent 对历史技能做 **agentic crossover** 创造新 skill；base-agent 执行 skill 并从 rollout 反馈学习。**MCE 相比 ACE 在五个领域平均再提升 16.9%**。

**Meta-Harness** 优化对象是"决定和优化应该存什么/检索什么/呈现给模型的代码"。它的"Meta-"含义就是"优化 harness 的 harness"。在 TerminalBench-2 上从两个极强 harness（Terminus-KIRA、Terminus-2）初始化搜索。

#### 方向二：工作流设计（Workflow Design）

**手工设计的框架**：AI Scientist（idea→code→experiment→analysis→manuscript→review 的 pipeline）、ScientistOne（以可验证性为核心，每条声明可追溯）、Autodata（Challenger/弱解/强解/裁判，合成"恰好合适"难度的数据）。

原文对 Autodata 有一个重要局限点评：**它合成的任务只用于微调弱求解器，而非强求解器；如果循环不能迭代改进强模型，就更像是"通过生成 prompt 分布做间接蒸馏"，RSI 特征较弱。** 这是一个很敏锐的判断——区分"真自改进"和"伪装成自改进的蒸馏"。

**自动化工作流设计**：ADAS 把 agent 设计形式化为优化问题，meta-agent 用代码编程新 agent；AFlow 把工作流表示为图（节点=LLM 调用，边=代码逻辑），用 **MCTS（蒙特卡洛树搜索）** 优化。

#### 方向三：自我改进的 Harness（Self-Improving Harness）

这是最贴近 RSI 主题的一节，原文的关键判断是：

> "**✨code✨** is a **universal language** for defining programs and systems."

Harness 就是编程 prompts、tool calls、subagents、control flow、memory、workflow logic 如何协同工作的代码。如果 LLM 能优化执行代理的代码，它可以访问比手写 prompts **大得多的设计空间**。

**STOP** 是递归脚手架改进的早期例子（见 2.1 节），它的递归更新 $I_t = I_{t-1}(\hat{u}, I_{t-1}; M)$ 体现了"改进改进器本身"的思想。

**Self-Harness** 依赖 LLM 代理通过 propose-evaluate-accept 循环改进自己的 harness，三阶段：

1. **Weakness Mining（弱点挖掘）**：把失败聚类为基于验证器的失败模式，需要包含终端验证器级别原因、相关代理行为的因果状态、轨迹暴露的抽象代理机制。
2. **Harness Proposal（提案）**：同一模型作为提议者，提供有界提案上下文（当前 harness 可编辑表面、验证器失败模式、应保留的通过行为、先前尝试摘要）。编辑应优先处理可处理的复发性错误模式，候选应独特且多样。
3. **Proposal Validation（验证）**：通过 held-in $D_{in}$（测弱点是否解决）和 held-out $D_{out}$（检查是否引入新问题）回归测试，**只有两个集合都无回归才接受**。

实验显示：Self-Harness 让 MiniMax M2.5（40.5%→61.9%）、Qwen3.5-35B-A3B（23.8%→38.1%）、GLM-5（42.9%→57.1%）在 held-out 通过率上显著提升，且学到的是**模型特异性**的指令。

**安全担忧**（原文）：

> "if a program is allowed to edit the OS system, abstraction boundaries are broken. The editable surface needs to be properly designed and the permission control and security layers need to live **outside this loop**."

所有关于 reward hacking 的挑战仍然存在。

#### 方向四：进化搜索（Evolutionary Search）

**适用条件**：搜索空间广阔或形状怪异 + 难以直接用梯度优化但容易评估。

**Promptbreeder / GEPA**：在 prompt 空间做进化。**AlphaEvolve** 是这条线的旗舰——它存储候选程序池，提示冻结 LLM 生成改进 diff，用 `# EVOLVE-BLOCK-START/END` 显式标记可编辑区。消融实验证实了进化过程、上下文、meta-prompts、全文件进化、更强 LLM 的价值。

**DGM（Darwin Gödel Machine）** 明确针对**可编辑 harness-code 仓库**的进化：从池中按"性能正比、子代数量反比"的概率选父代，父代检查自己的基准评估日志、提出改进、用 `bash` 和 `editor` 两个工具生成新版本，只有性能足够高才回池。SWE-bench Verified 20%→50%，Polyglot 14.2%→30.7%。

**适用与局限**：进化搜索适合"候选解可自动评估、适应度易量化"的领域（矩阵乘法、GPU kernel、算法竞赛、数据中心调度）；在"评估慢、模糊、主要靠启发式"的领域则困难。

### 4.5 与模型权重的联合优化（Joint Optimization with Model Weights）

Harness 进化只改非参数系统。要实现完全的自我改进，模型可以同时**更新自身权重**——通过训练流程改进或测试时持续学习。

**SIA（Self-Improving AI）** 是早期尝试：Meta-Agent 提初始 harness、Task-Agent 执行、Feedback-Agent 决定更新 harness 还是权重。但原文指出其实验有混淆因素——Task-Agent 用的模型（gpt-oss-120b）比 Meta/Feedback-Agent（Claude Sonnet 4.6）弱得多，基线太弱，无法干净交叉引用，训练稳定性和 Goodhart 效应仍是开放挑战。

---

## 五、必要知识反推：写出这篇综述必须掌握什么

假设让一个完全没有背景的人来写这篇综述，他至少必须掌握以下知识，并在关键节点上把它们融合。

### 5.1 领域知识层：理解 RSI 与 Harness 的本质

- **RSI 的思想史**：必须读过 Good（1965）、Yudkowsky（2008），才能在开篇准确地把"递归自我改进"定位成一个有六十年历史的老问题，而不是 2026 年的新发明。不理解这一点，会把整篇文章写成"Agent 框架综述"而非"RSI 现实路径综述"。
- **Harness vs Model 的边界**：必须能清晰区分"模型权重内的智能"和"模型外那一层的智能"。这是全文最核心的概念切割。MongoDB 那张"LLM 是最小部分"的图、Credal 对 harness vs runtime 的区分，都是这条认知的支撑。
- **Agent 框架的演化**：必须了解从 ReAct 到 Claude Code/Codex 的演化，才能说出"harness 在早期 agent 定义基础上额外包含了工作流设计、评估、权限控制、持久状态管理"这句话——否则无法定义 Harness 的边界。

### 5.2 方法论知识层：掌握四条优化路线

- **上下文工程谱系**：必须吃透 Prompt→ACE→MCE→Meta-Harness 这条"优化对象上移"的链，才能把杂乱的方法组织成有序进阶。不读懂 ACE 的"增量更新防坍塌"和 MCE 的"机制与内容分离"，就无法解释为什么这是进步。
- **进化计算与搜索**：必须懂进化搜索、MCTS、Gödel Machine 的理论背景，才能把 ADAS/AFlow/AlphaEvolve/DGM 放进同一个"把设计空间当搜索问题"的框架。尤其是 DGM 与 Schmidhuber 的 Gödel Machine 的理论血缘，不点出就丢了灵魂。
- **自我改进的数学结构**：必须能写出 STOP 的递归更新式 $I_t = I_{t-1}(\hat{u}, I_{t-1}; M)$ 和 meta-utility $\hat{u}(I)$，才能讲清"改进改进器本身"这个区别于普通优化的关键结构。
- **RL 与 reward hacking**：必须理解奖励黑客的机制，才能在"七大瓶颈"里把 reward hacking 讲得有分量，并给出"评估器和权限控制必须在循环之外"的解决方案。

### 5.3 工程知识层：系统实现与评估

- **编码代理的真实工具栈**：必须熟悉 Claude Code、Codex 这类产品级 Agent 的工具分层（文件系统、shell、IO、外部上下文、web、工件、后端进程、代理委派），才能给出那张可复用的工具表。MongoDB 博客提到 Claude Code 源码约 51.2 万行 TypeScript，模型交互只占很小一部分——这种工程实感是写出"harness 是工程主体"判断的前提。
- **基准测试生态**：必须了解 PaperBench、CORE-Bench、ScienceAgentBench、RE-Bench、MLE-bench、KernelBench 这六个基准的设计与最佳结果，才能在附录里给出"有用的基准"清单，并在正文用 RE-Bench 的"2 小时 AI 赢、8/32 小时人类赢"说明评估的时间尺度问题。
- **失败模式的一手观察**：必须读过 Trehan & Chopra（2026）的六种失败模式实验，才能写出"自动研究的失败模式"——偏向训练数据默认值、执行压力下实现漂移、记忆退化、过度乐观、领域智能不足、科学品味薄弱。这些是凭空编不出来的。

### 5.4 知识融合的关键节点

这篇综述的创造性，不在于单项知识，而在于几个融合节点：

1. **"Harness = 操作系统"的类比融合**：把软件工程的 OS 设计哲学、MLOps 的技术债图、Agent 框架的演化史，在"Harness 是运行时系统"这个节点上合一——这才让"配置和工具接口会逐渐标准化"的预判站得住。

2. **"优化对象上移链"的提炼**：把 prompt、context、workflow、harness code、optimizer code 这五个看似不同层次的优化，统一成一条进阶链——这需要同时懂 prompt 工程、上下文工程、工作流搜索、元学习才能看出来。

3. **"评估器必须在循环之外"的安全约束融合**：把 RL 的 reward hacking 教训、Self-Harness 的 held-in/held-out 双回归测试、进化搜索的适应度过拟合，在"安全边界设计"这个节点上合一——这让"安全"不是口号，而是问题定义的一部分。

4. **"内化但接口永存"的历史类比融合**：把 prompt engineering 被 instruction tuning 内化的历史，投射到 harness engineering 的未来——这个类比需要同时懂 prompt 工程史和模型能力演化趋势。

---

## 六、通用性灵感：可以迁移到其他领域的普适原理

以下每条灵感都来自论文的具体机制或实验证据，并给出可推广的场景。

### 灵感一：把"外壳系统"当作一等优化对象

**核心思想**：在任何"核心引擎 + 外壳系统"的结构里，外壳系统的工程质量往往和引擎本身同等重要，且外壳是更容易、更现实的优化入口。

**论文证据**：Lilian Weng 的核心判断——"模型与真实世界之间的那一层，和模型的裸智商同等重要"；MongoDB 引用的 Claude Code 51.2 万行 TypeScript 中模型交互只占很小一部分；DGM 通过只改 harness 就把 SWE-bench 从 20% 提到 50%。

**推广场景**：
- **数据库系统**：查询优化器（引擎）vs 索引/缓存/连接池配置（外壳）——外壳调优往往收益更大且风险更低。
- **编译器**：中间表示（引擎）vs pass 调度/内联阈值/向量化策略（外壳）。
- **人机交互设计**：底层能力（引擎）vs 交互流程/信息架构/反馈机制（外壳）。
- **组织管理**：个人能力（引擎）vs 流程/工具/协作机制（外壳）——改流程往往比换人更高效。

### 灵感二：有界增量更新优于无界整体重写

**核心思想**：在需要迭代维护"活文档/活配置/活策略"时，做局部、结构化、增量的更新，比反复重写整体更稳定——因为重写会丢失细节（context collapse）和偏向简洁（brevity bias）。

**论文证据**：ACE 的 Curator 不重写完整 prompt 块，只输出 `(identifier, description)` 增量条目并确定性合并；Self-Harness 要求提案"minimal（最小化）"且通过双回归测试才接受。

**推广场景**：
- **知识库/文档维护**：增量修订条款优于定期整体重写。
- **法律/合规规则更新**：补丁式修订优于推倒重来，保证可追溯。
- **个人笔记/第二大脑**：原子化卡片 + 链接优于反复重写长文。
- **软件配置管理**：diff 化的配置变更优于整文件覆盖。

### 灵感三：评估器必须位于优化循环之外

**核心思想**：任何自我优化系统，其评估标准和权限控制必须独立于被优化的对象之外，否则系统会过拟合自己的评分（reward hacking）。

**论文证据**：原文明确要求"评估器和权限控制应位于进化 harness 的循环之外，配合 held-out 测试、轨迹审计和关键决策点的人工审查"；Self-Harness 用 held-in + held-out 双回归才接受改动；STOP 在弱模型上退化说明"递归结构本身不够"。

**推广场景**：
- **绩效考核体系**：KPI 制定者不能是被考核者本人，且要引入独立审计。
- **自动驾驶安全**：规划模块（被优化）和碰撞验证器（独立）必须分离。
- **金融风控**：模型训练数据和回测/上线评估必须隔离，防止过拟合历史。
- **学术同行评审**：审稿人独立于作者，且引入 held-out 的复现实验。

### 灵感四：优化对象要不断"上移"——从产物到机制再到元机制

**核心思想**：当一个系统的某一层被优化到饱和后，下一阶段的红利在"更上一层"——优化"产生这个产物的机制"，再优化"优化机制的那个机制"。

**论文证据**：全文的进阶链 instruction prompts → structured context → workflow → harness code → optimizer code；ACE→MCE→Meta-Harness 的逐层上移；STOP 的"改进改进器本身"。

**推广场景**：
- **教育**：教知识（产物）→ 教学习方法（机制）→ 教"如何发现自己的学习方法"（元机制）。
- **产品迭代**：优化功能（产物）→ 优化研发流程（机制）→ 优化"改进流程的方法论"（元机制）。
- **个人成长**：改习惯（产物）→ 改"养成习惯的系统"（机制）→ 改"设计系统的元认知"（元机制）。

### 灵感五：让并行性与中间状态显式、可检查、可恢复

**核心思想**：在复杂的多步骤、多分支系统里，把并行结构、中间产物、执行历史显式地落盘成可检查的文件/日志，比让它们停留在临时内存里更健壮——系统才能在中断后恢复并自我推理。

**论文证据**：模式三"子代理与后端作业"的关键设计选择——"make parallelism explicit and inspectable"；模式二"文件系统作为持久记忆"；DGM 把每个候选 harness 存为文件系统中的字典（源代码、分数、rollout 轨迹、状态更新）。

**推广场景**：
- **分布式系统**：可观测性（observability）优先——显式日志、链路追踪、状态快照。
- **科研实验管理**：实验追踪（MLflow/W&B）把每次运行的配置、指标、产物显式记录。
- **项目管理**：看板/甘特图让并行任务显式可见，而非埋在聊天记录里。
- **数据处理流水线**：DAG 化 + 检查点，让每一步可重跑、可审计。

### 灵感六：能力门槛——递归自改进要求基础足够强

**核心思想**：任何"自我改进"机制都有一个能力门槛：基础能力低于阈值时，递归结构不仅不改进，反而会退化。设计自我改进系统前，必须先确认基础是否跨过了门槛。

**论文证据**：STOP 在 GPT-4 上持续改进，但在 GPT-3.5、Mixtral 上退化；原文明确"递归结构本身不够，基础模型必须足够强才能改进这个机制"。

**推广场景**：
- **组织自驱改进**：团队能力低于门槛时，"自驱复盘"会变成自我安慰，需要外部指导。
- **学生自主学习**：基础薄弱时直接放养会退步，需要先达到元认知门槛。
- **自动化运维**：系统不稳定时引入"自愈"会放大故障，先稳定再自愈。
- **民主治理**：公民素养、制度成熟度未达门槛时，过度放权可能导致倒退。

### 灵感七：把失败做成一等学习资源

**核心思想**：失败案例（尤其是带丰富上下文的失败轨迹）是最有价值的学习信号；系统应让失败易保留、易聚类、易回溯，而不是丢弃。

**论文证据**：Self-Harness 的 Weakness Mining 把失败聚类为失败模式，要求失败记录包含验证器原因、因果状态、抽象机制；原文 Future Challenges 强调"learning from failure is the best way to trim down the task search space"，并批评文献偏向成功、LLM 不擅长报告负面结果。

**推广场景**：
- **事故复盘文化**：航空/医疗的高可靠性组织靠详尽的事故报告学习。
- **推荐系统**：负反馈（不点击、划走）和正反馈同等重要。
- **科研记录**：失败的实验记录（negative results）应被发表和归档。
- **产品迭代**：用户流失原因比留存原因更有学习价值。

### 灵感八：人类应"上移"而非"移出"监督层

**核心思想**：在越来越自动化的系统里，人类的角色不是被移出循环，而是在更高的抽象层级、在正确的时间点提供监督——系统设计要预留这些"接触点"。

**论文证据**：原文 Future Challenges 第七点"Humans should move up the stack, not be removed from the loop"，并强调"我们是在为人类更美好的未来建造技术，而不是反过来"。

**推广场景**：
- **AI 辅助医疗**：医生从写病历上移到关键诊断决策审核。
- **自动驾驶**：驾驶员从操作上移到紧急接管和路线决策。
- **自动化投资**：投资人从盯盘上移到策略制定和风险边界设定。
- **教育 AI**：教师从批改作业上移到学习路径设计和情感支持。

---

## 附录：文中提到的六个有用基准

| 基准 | 任务 | 规模 | 当时最佳结果 |
|------|------|------|------------|
| **PaperBench**（ICML 2025） | 从头复制 20 篇 ICML 2024 Spotlight/Oral 论文 | 8316 个评分标准 | Claude 3.5 Sonnet ≈21% |
| **CORE-Bench**（TMLR 2024） | 已发表研究的计算可复现性 | 270 任务，基于 90 篇论文 | GPT-4o 最难任务 21% |
| **ScienceAgentBench**（ICLR 2025） | 数据驱动科学发现 | 102 任务，来自 44 篇出版物 | — |
| **RE-Bench**（ICML 2025） | 现实 ML 研究工程，AI vs 人类专家 | 7 个开放式环境，61 名人类专家 71 次 8h 尝试 | 2h AI 赢；8h/32h 人类赢 |
| **MLE-bench**（2024） | 离线 Kaggle 竞赛 | 75 个 Kaggle 竞赛 | o1-preview + AIDE 16.9% 达铜牌 |
| **KernelBench**（2025） | 生成 GPU kernel 的正确性和速度 | 250 个 PyTorch 任务 | — |

其中 **RE-Bench** 的时间尺度对比特别值得玩味：**2 小时预算下最佳 AI 代理得分比人类高 4 倍；但拉长到 8 小时和 32 小时，人类的长期回报反超**——这恰好印证了原文"许多优化目标过于短期"的判断，也提醒我们：**评估的时间尺度，决定了我们对"谁更强"的结论。**
