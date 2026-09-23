---
title: "Harness-Zero：通过 Agent-as-Harness 实现 Harness 蒸馏——论文精读"
date: 2026-09-23
draft: false
tags: ["Agent", "Harness", "AI自进化", "后训练", "论文精读", "知识蒸馏", "模仿学习"]
categories: ["paper-reading"]
summary: "精读北京大学与 Google 合作的 Harness-Zero，提出 agent-as-harness 范式：用一个审查型智能体在学生模型的响应边界上把进化出的专用 harness 收益「翻译」成可训练轨迹，经 SFT 把外挂行为蒸馏进权重，部署时彻底移除外挂。Qwen3.5-9B 宏平均成功率从 23.3% 提升到 44.3%，甚至超过挂载原 harness 的 41.7%。"
---

# Harness-Zero: Harness Distillation via Agent-as-Harness —— 精读

> **论文链接**：[arXiv:2609.24974](https://arxiv.org/abs/2609.24974) ｜ [GitHub](https://github.com/metaevo-ai/harness-zero)
>
> **发表时间**：2026 年 9 月（arXiv:2609.24974v1，提交于 2026-09-21）
>
> **发表机构**：北京大学（通用人工智能全国重点实验室、智能科学与技术学院）+ Google
>
> **作者**：Haoran Ye、Yuxing Lu、Haonan Dong、Zhaochen Su、Guojie Song 等
>
> **关键词**：LLM agents、agent harness、harness evolution、harness distillation、agent-as-harness、fine-tuning

---

## 一、从「模型」到「模型 + 脚手架」：为什么 Harness 值得单独研究

要读懂这篇论文，先得弄清一个被很多人忽略的事实：**今天你用的大模型 Agent，真正决定它能不能把活干好的，往往不是模型本身，而是包裹在模型外面那一层「系统」。**

论文把这一层叫做 **Agent Harness（智能体脚手架）**——即那些「不属于模型权重本身的、用来组织工具调用、管理上下文与状态、控制与环境交互的外部系统」。用一句通俗的话说：**模型提供智能，Harness 让这种智能真正可用。**

一个裸模型本身只会接收文本、输出文本，它默认无法：

- 在多轮交互之间维持持久状态；
- 执行代码、操作文件系统；
- 获取实时信息；
- 搭建运行环境、安装依赖。

这些能力全部属于 Harness 层。换句话说，**Agent = Model + Harness**。你看到的 Claude Code、Codex、Kimi Code，本质上都是「一个强模型 + 一套精心设计的脚手架」。

近年来，「Harness 工程」已经成为提升 Agent 性能的核心杠杆：

- **编程 Agent 的 harness** 组合了 shell 访问、持久化内存的文件系统、子 Agent、后台任务；
- **科研 Agent 的 harness** 组织假设生成、实验执行、证据收集的工作流；
- 更进一步，**Meta-Harness** 之类的自动化方法（Lee et al., 2026；Lin et al., 2026；Zhang et al., 2026）甚至能自动优化 harness 代码本身。

但问题恰恰出在这里：harness 优化改进的是「模型外面的脚手架」，而不是「模型本身」。这就引出了本文要解决的真正难题。

---

## 二、核心矛盾：进化出的专用 Harness 收益，带不进生产

既然最好的 harness 能大幅提升 Agent 表现，那是不是只要不停进化 harness 就行了？论文点出了一个被忽视的代价：

**harness 的收益是「绑定在部署时的那个 harness 上」的。**

原因很现实——**「最好的 harness」会因领域、任务实例、乃至基础模型的不同而不同**（Liu, 2026；Luo et al., 2026；Zhang et al., 2026）。一个在电子表格处理上表现出色的专用 harness，放到分子逆合成任务上可能完全帮倒忙。于是通用 Agent 面临一个两难：

1. **用一套共享 harness**：为了兼容所有场景，只能放弃一部分专门优化带来的增益；
2. **为每个领域维护一套专用 harness**：需要路由（routing）机制，并持续付出上下文、模型调用、工具调用和编排的成本（Chang et al., 2026；Lin et al., 2026；Zhang et al., 2025/2026）。

**无论哪种选择，都没有把「发现出来的 harness 改进」真正搬进模型权重里。** 脚手架永远是外挂，换个环境或模型就得重来。

为此，论文正式提出一个新任务——**Agent Harness Distillation（智能体脚手架蒸馏）**：

> 用一个「针对某领域/某实例优化过的 harness」作为训练期引导，把它所诱导出的行为**蒸馏进模型参数**，使得这些增益能够在一个**单一固定的目标 harness** 下存活下来。

这里的难点是决定性的：**源 harness（进化出的专用 harness）和目标 harness（部署用的极简 harness）在「动作空间」和「可用信息」上都不一样**。例如，专用 harness 可能给学生模型多挂载了 RDKit 分子校验工具、预填单元格保护中间件；而目标 harness 只有一条 `execute` 命令。因此，在源 harness 下收集到的轨迹，**无法直接当作目标 harness 下的模仿样本**——动作对不上、信息也对不上。

论文给出了一个触目惊心的对照数据：直接把更强模型（teacher）在目标 harness 下的轨迹拿去 SFT 学生模型，**学生成绩纹丝不动，还是 12%**（详见第七节 Table 3）。这正是「动作空间失配」的硬证据。如何跨过这道鸿沟，就是 HARNESS-ZERO 要回答的问题。

---

## 三、HARNESS-ZERO 总览：把「外挂」蒸馏进「权重」

论文的解法叫 **HARNESS-ZERO**，核心思想是 **agent-as-harness（以智能体充当脚手架）**。它分三个阶段（对应论文 Figure 1）：

**阶段一：进化与适配（Evolve & Adapt）**
- 在训练任务 `Dtrain` 上，先用现有自动化方法（如 meta-harness）进化出一个**学生侧专用 harness**，记作 `h⋆`。
- 再把 `h⋆` **适配（Adapt）** 成一个仅供「审查型智能体」使用的**私有参考 harness**，记作 `K`。
- 记号：`h` = 固定目标 harness（部署时保留的极简 harness）；`h⋆` = 进化出的学生侧 harness；`K` = 由 `h⋆` 相对 `h` 适配而来的私有参考 harness。

适配的精髓是「**保留意图、改变作用点与受众**」：

| 学生侧 `h⋆` 的组件 | 适配到 `K` 后的形态 | 变化本质 |
|---|---|---|
| Tool（直接扩展动作空间） | Action recipe（说明如何在学生动作空间构造等价动作） | 工具 → 构造配方 |
| Middleware（在循环中修改/阻断执行） | Review middleware（当条件命中时，私下提醒审查智能体） | 执行期拦截 → 审查期告警 |
| Skill（教学生工作流） | Review guidance（用工作流诊断当前步并构造替换） | 直接指令 → 诊断准则 |
| Memory（记录失败与经验） | Failure patterns（把每条经验变成可识别的审查条件 + 干预动作） | 经验库 → 审查条件 |

举论文里的例子：在 `h⋆` 里有一条中间件 `prefilled_guard`，它会在学生「交卷」时**直接进沙箱 diff 输入输出工作簿**，发现覆盖了预填示例单元格就拒绝提交。但审查智能体**不应**拥有独立操作学生环境的接口（否则这种行为就蒸馏不进学生权重了）。于是适配后的 `K` 里，对应的 review middleware 改成：**从学生可见轨迹里寻找「是否做过预填对比」的证据**，如果没有，就私下提示审查智能体把这次 finish 替换成一个验证动作。

**阶段二：Agent-as-Harness 轨迹收集**
- 学生模型在 `h` 下运行，每轮先自己提出响应 `yt`；
- 审查智能体用 `K` 在响应执行前进行审查，决定 PASS（原样通过）或 REPLACE（给出最小修正）；
- 被接受的响应通过 `h` 执行，结果进入学生可见轨迹；被拒绝的提案和私有审查过程**不进入**学生可见轨迹。

**阶段三：训练与部署**
- 对「审查后的轨迹」做监督微调（SFT），把示范行为内化进模型参数；
- 部署时，**彻底移除** `h⋆`、`K` 和审查智能体，只剩「蒸馏后的学生 + 目标 harness `h`」。

---

## 四、Agent-as-Harness：在响应边界上的「教练式」纠错

这是整篇论文的灵魂。让我们把它讲透。

### 4.1 它到底在做什么

在训练数据收集阶段，审查智能体（`πH`）像一个**坐在学生旁边的教练**，包住了学生的「响应边界」。每一轮：

1. 学生政策 `πS`（在 `h` 下运行）先提出一个响应 `yt`；
2. 审查智能体拿到「学生可见上下文 `ct` + 当前提案 `yt` + 私有参考 `K` + 自己的私有历史 `r<t`」，输出一个决策：
   - `PASS`：提案没问题，原样通过；
   - `REPLACE`：需要干预，给出一份**在 `h` 动作空间内合法的完整替换响应** `zt`；
3. 被接受的响应 `ỹt`（PASS 时等于 `yt`，REPLACE 时等于 `zt`）通过 `h` 执行，成为下一轮上下文的一部分。

形式化地（论文式 3）：

```
yt   ~ πS(· | ct)              // 学生提案
(dt, zt) ~ πH(· | ct, yt, K, r<t)   // 审查决策
ỹt  = yt      若 dt = PASS
     = zt      若 dt = REPLACE
ct+1 = Th(ct, ỹt)              // 目标 harness 执行
```

**关键约束**让这套机制能变成训练数据：

- **最小化改动**：能 PASS 就 PASS；非要干预时，只做「最小且连贯」的修正，尽量保留学生原提案的意图、变量名、命令结构。每个替换都是一份从当前学生可见状态继续下去的完整响应。
- **权限受限**：审查智能体唯一特权是「读 `K`」，**不能**偷看隐藏答案或验证器反馈，**不能**访问 `ct` 之外的环境状态。它若想获取额外证据（比如怀疑学生提前交卷），只能通过 `h` 提交一个动作（例如跑一段检查代码），让结果自然出现在学生可见轨迹里。这一「接地（grounding）约束」保证了审查干预是可被学生独立复现的。

### 4.2 一句话类比：教练 vs 手册

论文把两种范式放在一起对比，这里给一个直观类比：

- **Code-as-Harness（以代码充当脚手架）** 像**发给学生一本写死的操作手册**：手册（`h⋆` 的代码）直接裹在学生外面，既规定动作、又负责执行 `yt`。它把「模型应该如何行动」的假设写死在代码里。
- **Agent-as-Harness（以智能体充当脚手架）** 像**旁边坐了一位教练**：教练（`πH`）读着参考要点 `K`，逐条审查学生的每一份提案，判断「现在该不该插手、怎么插手」，只在必要时给出最小修正。

更妙的是，论文指出 code-as-harness 有个致命弱点：**代码 harness 把「模型该如何行动」的假设固化了，随着模型能力变化，这些假设会过时，必须为每个新模型重新适配**（Liu, 2026；Qian et al., 2026）。而 agent-as-harness 把这种适配**搬进了推理过程**——审查智能体结合当前轨迹解读 `K` 并决定何时干预，于是同一份 `K` 可复用，且「更强的审查模型」会直接提升干预质量。论文预期：随着基础模型越来越强，agent-as-harness 相对固定 code harness 的优势会持续拉大（前提是审查模型本身够强，详见第八节）。

---

## 五、从审查轨迹中学习：SFT 与「推理遮罩」

阶段三的学习目标很清晰：在目标 harness 下做模仿学习（imitation learning），对审查轨迹里**所有被接受的响应**（无论 PASS 还是 REPLACE）施加 SFT。

但这里有一个工程上的坑：REPLACE 产生的替换响应，是在「私有审查上下文」里写出来的，它**可能不小心带上「审查者视角」的口吻**——比如写着「这个提案应该被改写为……」「我（教练）决定通过」。如果直接拿来训练学生，会让学生学会一种奇怪的、第三方视角的自言自语。

论文的解法是 **mask（遮罩）掉这类审查者视角推理的 token**，不计入 loss（§D 给出了具体的过滤规则与正则模式，覆盖 proposal/proposed/draft/review 等词汇）。背后的理念是：**训练学生时，它必须用自己的第一人称口吻、基于自己可见的证据来表达**。被遮罩后的替换响应，看起来就像「学生自己发现了错误、自己纠正了方向」。

于是部署时，系统就是 `(π̂θ, h)`——**只有蒸馏后的学生 + 目标 harness `h`**，没有 `h⋆`、没有 `K`、没有审查智能体。训练目标就是让 `π̂θ` 在 `h` 下复现 `h⋆` 所诱导出的行为模式。

需要强调：本文的审查与修正发生在**「响应（response）粒度」**——PASS 原样通过，REPLACE 替换整份响应。这与第六节将要讨论的「token 级修正」是正交的两种精度，但共享同一核心原则（见第六节）。

---

## 六、相关工作交叉验证（WebSearch）

为了让读者判断 HARNESS-ZERO 的「新」究竟新在哪，我用检索交叉验证了三类相近工作，并明确区分「方法相似」与「结论相近」。

### 6.1 蒸馏家族：知识蒸馏、On-Policy 蒸馏与 DAgger

**（1）知识蒸馏（Knowledge Distillation）** 是 Hinton 等人提出的经典范式：用大模型（teacher）的输出分布去监督小模型（student）。但标准离线蒸馏有个老问题——**暴露偏差（exposure bias）**：训练时学生看到的是 teacher 强制出的状态分布，推理时学生自己一步步生成，一旦走偏就进入训练时没见过的状态，错误会累积。

**（2）On-Policy 蒸馏（OPD）** 是直接相关的近亲。根据 VERL 文档与 EmergentMind 的综述，OPD 的核心做法是：**让学生从自己当前策略采样 rollout，再由 teacher 在这些「学生实际会访问到的状态」上提供逐 token 的监督**。它把 on-policy RL 的「训练/推理状态对齐」与 KD 的「稠密监督」结合，理论上能把多步推理的复合误差从 `O(εT²)` 降到 `O(εT)`（ε 为每步错误率，T 为序列长度）。

**（3）DAgger（Dataset Aggregation，Ross, Gordon & Bagnell, 2011）** 是这一思想的鼻祖级方法。它要解决的是行为克隆（behavior cloning）的**分布偏移 / 协变量偏移（covariate shift）**问题：训练时学生只见专家状态，部署时一点点小错把它推到专家从未访问的状态，错误随步数**复利式累积（compounding error）**，长程任务里尤其致命。DAgger 的迭代方案是——先行为克隆，再**用当前学生策略在环境中 rollout，把学生真正访问到的状态送给专家标注正确动作，聚合回数据集重新训练**。其理论遗憾界随 horizon `T` 线性而非二次增长，是首个在序贯决策下有无悔保证的模仿学习算法（参考 [roboticscenter.ai 的 DAgger 词条](https://www.roboticscenter.ai/en/glossary/dagger)）。

> **方法相似点定位**：HARNESS-ZERO 的 agent-as-harness 本质上是一种**「harness 层面的 DAgger 式 / on-policy 式交互模仿」**。它不从「学生之外」灌入 teacher 轨迹，而是**从学生自己的轨迹出发**，在每个状态（响应边界）上由审查智能体给出最小修正——这正是 DAgger「在 student 实际访问到的状态上标注正确动作」与 OPD「在学生诱导出的状态上训练」的同一原则。差别在于：DAgger/OPD 多在 token 或状态级提供监督，HARNESS-ZERO 在**响应级**提供 PASS/REPLACE，并额外要求修正必须落在目标 harness 的动作空间内。可以说，本文把「on-policy 纠正」从通用序列生成推广到了「跨 harness 动作空间迁移」这一新场景。

可核验链接：
- DAgger 词条：<https://www.roboticscenter.ai/en/glossary/dagger>
- On-Policy Distillation（VERL）：<https://verl.readthedocs.io/en/latest/algo/opd.html>
- On-Policy KD 综述（EmergentMind，明确连接 DAgger）：<https://www.emergentmind.com/topics/on-policy-knowledge-distillation>

### 6.2 Harness / Agent 训练迁移类工作：区分「改进 harness」与「把 harness 蒸馏进权重」

检索到几篇与「harness 训练/迁移」相关的论文，需逐一区分：

- **Self-Harness（Zhang et al., 2026，[arXiv:2606.09498](https://arxiv.org/abs/2606.09498)）**：提出「让 Agent 自己改进自己运行的 harness」，迭代循环为 Weakness Mining（从轨迹挖模型专属失败模式）→ Harness Proposal（生成最小且多样的 harness 修改）→ Proposal Validation（回归测试通过才接受）。它在 Terminal-Bench-2.0、AppWorld 等上把模型专属 harness 越改越好。**方法差异**：Self-Harness 的终点是「更好的 harness 本身」，仍然依赖外挂；HARNESS-ZERO 的终点是「把 harness 行为内化进权重并彻底移除外挂」。两者共享「harness 是值得被优化的第一公民」这一世界观，但目标相反（一个改脚手架，一个消脚手架）。

- **Agentic Routing / Agent-as-a-Router（[arXiv:2607.11399](https://arxiv.org/abs/2607.11399)、[arXiv:2606.22902](https://arxiv.org/abs/2606.22902)）**：这是任务中提到的「Agent Routing」一脉。它们研究的是**在 harness 状态条件下做 step 级模型路由**（把每个子任务派给最合适/最省的模型），并强调「每次路由决策天然产生一条带环境标签的数据记录，形成 harness-native 数据飞轮」。**目标不同**：它们是成本/能力调度问题，不是把某个 harness 蒸馏进权重；与 HARNESS-ZERO 没有方法继承关系，只是同处「harness 作为系统核心」这一大趋势下。

- **Co-Harness（[arXiv:2607.22688](https://arxiv.org/abs/2607.22688)）**：联合优化 harness 与模型权重——用 LLM 批评者（HarnessCritic）从失败轨迹归因、提局部 diff，再用改进后 harness 生成的高质量轨迹 SFT 模型，形成「harness 越好→轨迹越好→模型越强→暴露新瓶颈」的正反馈。**方法最相近**，但论文在 Related Work 里明确点出关键差别：Co-Harness 等「模型—harness 联合优化」工作，其收益**仍可能与 harness 绑定**；而 HARNESS-ZERO 用临时审查智能体引导、最终把行为蒸馏进权重，部署时**不留 harness 也不做路由**。此外 Co-Harness 也承认存在「Harness Debt」（训练时依赖补偿性脚手架，测试时无 harness 会掉点），而 HARNESS-ZERO 恰好是冲着消除这种 debt 去的。

- **OPHSD（Zhao et al., 2026，[arXiv:2605.08741](https://arxiv.org/abs/2605.08741)，论文引用 [57]）**：「On-policy harness self-distillation」，把顺序 draft–verify / plan–solve 工作流产生的特权输出蒸馏进独立模型。**精神最近**，可视为 HARNESS-ZERO 的前身式证据；但 OPHSD 只覆盖单一机制、不涉及交互式 Agent 循环，HARNESS-ZERO 则给出了「完整工具使用型 Agent harness」的通用蒸馏方法。

- **EvoHarness-RL（Ning et al., 2026，[arXiv:2608.05446](https://arxiv.org/abs/2608.05446)）**：最早在 ALFWorld 上给出 harness 内化证据（训练出的 Agent 学会管理外部状态、减少 harness 调用），但**部署时仍保留外部工作区**。HARNESS-ZERO 在「部署即移除」这一点上更彻底。

### 6.3 相近结论：DAgger 的分布偏移，与「整段模仿会破坏模型—harness 适配」

最值得强调的是一篇**直接佐证 HARNESS-ZERO 核心结论**的同期工作——Salesforce AI 的 **Co-Evolving Harnesses and Models: On-Policy Correction Helps Weaker Models Catch Up Where Imitation Fails（[arXiv:2609.09134](https://arxiv.org/abs/2609.09134)）**。它的发现几乎与本文互为镜像：

- 先为弱模型进化一个 harness，再**让强专家在该 harness 下生成完整轨迹去微调弱模型**——结果在 7 个企业任务上**全面回退 4–30 分**（即便同样做法在「未进化 harness」下是有益的）；
- 归因是**「模型—harness 适配被破坏」**：弱模型照搬了专家的规划策略，却没有执行它的能力，于是与「围绕自身原生规划风格进化出来的 harness」不再匹配；
- 解法是 **on-policy 专家纠正**：让一个元级智能体**定位弱模型自己 rollout 里出错的那一回合，只让专家重写那一回合**，保留模型原本的规划分布。

这与 HARNESS-ZERO 的消融（Table 3）结论**高度一致**：直接 SFT teacher 轨迹 → 12%（动作空间失配，学生疯狂尝试不可用的 harness 工具直至耗尽回合）；而**「在学生自己轨迹上、由 harness 引导的最小修正」**→ 30%。两者共同指向同一个根因——**DAgger 式的分布偏移**：当监督来自「专家/源 harness 实际走出的轨迹」而非「学生自己会访问到的状态」时，整段模仿会把学生推离它本来的分布，破坏它与目标环境/目标 harness 的适配。HARNESS-ZERO 用「响应边界上的 on-policy 最小修正」解决，Salesforce 那篇用「定位失败回合、只重写那一步」解决——**方法不同，结论同源**。

> 关于任务里提到的「token 级修正」：严格说 HARNESS-ZERO 做的是**响应级**修正（REPLACE 整份响应），比 token 级更粗；但它与 OPD/DAgger 的「token 级/状态级监督」共享同一底层原则——**纠正必须发生在学生实际走过的状态上，而非对专家轨迹的 wholesale 模仿**。把「token 级修正」理解为「在 student 自身体验的局部做最小干预」这一家族思想的代表即可。

---

## 七、实验结果：数字说话

论文在三个领域评测：**电子表格知识工作（SpreadsheetBench Verified）**、**多应用工具使用（AppWorld）**、**科学推理（USPTO 逆合成）**。目标 harness `h` 是一个极简的 mini-SWE-agent（固定 system prompt + 单个 Bash `execute` 工具）。蒸馏实验以 **Qwen3.5-9B** 为基础模型，**GPT-5.6 Sol** 担任审查智能体，harness 进化用 **Kimi K3**（Kimi Code）。训练为 LoRA SFT（Tinker recipe，rank 32，2 epoch）。

### 7.1 推理期：Agent-as-Harness 胜过 Code-as-Harness（无需训练）

表 1 在 GPT-5.6 Sol 与 DeepSeek-V4-Pro 两个前沿模型上，变化两个因素：是否有进化出的 harness、以及它「以代码包裹学生」还是「以审查智能体审查学生」抵达。

| 设定 | 平均（六组设定） | 相对 `h` |
|---|---|---|
| mini-SWE-agent（`h`，无 harness） | 68.6% | — |
| meta-harness（`h⋆`，code-as-harness） | 78.1% | +22.5% |
| agent-as-harness（空 `K`） | 69.2% | +1.0% |
| **agent-as-harness（适配 `K`）** | **81.1%** | **+27.6%** |

结论很清晰：**有进化 harness 时，agent-as-harness 平均 81.1%，超过 code-as-harness 的 78.1%**；而光有审查、没有 `K`（空 `K` 仅 69.2%）说明「审查本身」贡献很小，**增益来自 harness 引导的审查**。此外 agent-as-harness 还有「跨模型更新更自适应」的好处：code harness 把模型行为假设写死、易过时，agent-as-harness 把适配搬进推理、随更强审查模型直接变好。

### 7.2 蒸馏后：移除外挂，反而更强

表 2 是本文最重要的结果。所有设定都用 Qwen3.5-9B，区别在部署时挂不挂 harness：

| 设定 | SpreadsheetBench | AppWorld | USPTO | 宏平均 |
|---|---|---|---|---|
| 基础模型（`h`） | 31.0 | 26.8 | 12.0 | **23.3** |
| meta-harness（`h⋆`） | 39.0 | 48.2 | 38.0 | **41.7** |
| DeepAgents（通用 harness） | 35.0 | 19.6 | 7.0 | 20.5 |
| Claude Code（通用 harness） | 31.0 | 10.7 | 6.0 | 15.9 |
| **HARNESS-ZERO（`h`，外挂全移除）** | **44.0** | **58.9** | **30.0** | **44.3** |

- **HARNESS-ZERO 把宏平均从 23.3% 拉到 44.3%，相对提升 +90.1%**；
- **它不仅超过基础模型，还超过了「挂载着原 harness `h⋆`」的 41.7%**——也就是说，**蒸馏进权重后，连那个专用外挂都不需要了，反而更好**；
- 两个通用 harness（DeepAgents、Claude Code）对这个 9B 模型反而**有害**（15.9% 甚至低于裸 `h` 的 23.3%），因为 9B 模型撑不起它们更大的通用工具集和超长上下文，而 USPTO/AppWorld 需要的是 `h⋆` 里那种领域专属工具与约束。

### 7.3 消融：什么才是有效的监督信号

表 3（USPTO）逐一替换「训练轨迹来源」，结论极具说服力——**有效监督来自「harness 引导的 on-policy 审查」，而不是更强的示范本身**：

| 训练轨迹来源 | 收集成功率 | 蒸馏后测试 pass@1 |
|---|---|---|
| 基础模型（不训练） | — | 12.0 |
| 直接蒸馏：Teacher 轨迹（GPT-5.6 Sol 在 `h` 下） | 52.0 | **12.0** ↑0.0 |
| Teacher 在 `h⋆` 下 | 62.0 | 3.0 ↓9.0 |
| 学生 在 `h⋆` 下 | 39.4 | 12.0 ↑0.0 |
| 审查：空 `K` | 44.2 | 11.0 ↓1.0 |
| 审查：给 oracle 答案 | 98.6 | 15.0 ↑3.0 |
| **HARNESS-ZERO（`K`）** | 59.4 | **30.0** ↑18.0 |

几个反直觉但关键的发现：

- **更强模型的直接轨迹毫无用处**（52% 收集成功率 → 12% 测试），因为动作空间失配，学生疯狂尝试不可用的 harness 工具直至耗尽回合；
- **收集成功率高 ≠ 蒸馏价值高**：给 oracle 答案让收集成功率飙到 98.6%，但蒸馏后只有 15%——oracle 访问诱使出**不可泛化的捷径式修正**；而 `K` 不含任何任务答案、收集成功率仅 59.4%，却蒸馏出 30%。
- 这再次印证第六节：必须从学生自己的轨迹出发、把 harness 引导翻译为与其当前状态兼容的修正。

### 7.4 行为内化：28 个模式，平均恢复 82.3%

表 4 把 `h⋆` 里工具/中间件/记忆/技能诱导出的行为，翻译成轨迹探测器，挑出「基础模型只在 `h⋆` 下、在 `h` 下从不出现」的 28 个模式（SpreadsheetBench 18 个、USPTO 6 个、AppWorld 4 个），测 HARNESS-ZERO 在这些任务上是否复现该行为。结果**平均恢复率 82.3%**，覆盖记忆、技能、工具、中间件四类来源。例如 SpreadsheetBench 的「先检查再编辑」（100%）、「保护预填单元格」（92%）、「写计算后的字面值而非公式」（90%）；USPTO 的「先用 RDKit 解析产物」（100%）、「用 RDKit 校验每个结构」（100%）、「SMILES 规范化」（100%）。

**一个细分结论（Obs.❸）**：程序性 harness 行为比深领域知识更容易内化。SpreadsheetBench 与 AppWorld 上，HARNESS-ZERO 在仅用 `h` 时就**反超**了 `h⋆`——因为它们的 harness 主要编码「状态检查、定向修改、验证」等可直示范的程序；而 USPTO 上 HARNESS-ZERO（30%）虽远超基础模型（12%），仍落后于 `h⋆`（38%），因为 `h⋆` 还额外提供了反应先验、候选生成逻辑和可执行的 SMILES 校验——这类深层知识可能需要更广的预训练/mid-training 覆盖或更多蒸馏轨迹。

---

## 八、讨论、局限与未来

论文坦诚地列出了局限：

1. **依赖够强的审查模型**。弱模型上审查会变成负担甚至有害。表 8 把 USPTO 比较扩展到更弱的模型：agent-as-harness 相对 meta-harness 的优势与模型能力**强相关**——在 GPT-5.6 Sol（+1.0%）、DeepSeek-V4-Pro（+4.0%）上为正，到 DeepSeek-V4-Flash（-1.0%）、Qwen3.6-35B-A3B（-12.0%）上转负。最弱模型上审查智能体激进地替换了 66% 的步骤，但净效果是有害的。可见「有效审查与干预」需要足够的底层能力。

2. **收集成本**。当前设计在每步执行前都审查一次，至少每步两次模型调用（学生提案 + 审查），且审查要处理整段轨迹前缀与 `K`。USPTO 上 agent-as-harness 平均 237.2 秒/任务，约为 mini-SWE-agent（100.1 秒）的 **2.4 倍**。好在这笔开销只发生在轨迹收集期，蒸馏后部署为零。

3. **并非所有 harness 机制都能表达为响应**。例如上下文管理（context management）无法开箱即用地写成学生响应。HARNESS-ZERO **不消除对 harness 的需求，而是收窄了它必须提供的东西**——论文预期随着 harness 蒸馏技术进步，一个极简 harness 就够用。

未来方向也很有想象空间：

- 把审查智能体建模为**「反事实预测」问题**——预测执行学生提案会如何改变环境、干预能否带来更好轨迹，这指向更强的 **Agent World Model**；
- 框架层：**用代理信号选择性地触发审查**（而非每步都查）、支持更细粒度机制如 **token 插入、隐空间引导（latent-space steering）**；
- 训练层：每次 REPLACE 在同一状态下配对「被拒响应 + 偏好响应」，天然适合做 **偏好学习（preference learning）**，可吸收 response 级 SFT 丢弃的比较信号。

---

## 九、总结：对 AI 自进化与后训练的范式意义

把全文收束成一句话：**HARNESS-ZERO 把一个「进化出的学生侧 harness `h⋆`」变成了「在固定目标 harness `h` 下运行的学生模型」的训练监督，通过 agent-as-harness 在响应边界上把 harness 引导重写为可执行的修正，再用 SFT 把行为内化进权重，部署时彻底移除外挂。**

它的三层贡献值得反复品味：

1. **提出了 agent harness distillation 这一新任务**，并给出 HARNESS-ZERO 这一通用解法——把「通过 harness 优化发现的增益」转化为「在固定目标 harness 下可用的模型能力」；
2. **发明了 agent-as-harness**，在学生的响应边界把优化 harness 的引导「翻译」成跨动作空间的可执行监督，使不同 harness 之间的模仿学习成为可能；
3. **用实验证明了三件事**：agent-as-harness 在推理期能胜过 code-as-harness（81.1% vs 78.1%）；蒸馏后移除 harness 仍保留甚至超越其增益（44.3% vs `h⋆` 的 41.7%）；蒸馏模型能恢复 `h⋆` 专属行为（28 个模式平均 82.3%）。

放到更大的图景里，这篇论文呼应了当下「**AI 自进化 / 后训练（post-training）**」的核心命题。今天的 harness 工程已经能自动进化出领域专属脚手架，但这些增益碎片化地散落在各种外部系统里。如果能在后训练阶段，把**众多领域/任务专属 harness 各自诱导出的行为与知识，蒸馏进一个共享模型**，那么 agent 系统之间积累的经验就不再被困在外部脚手架中，而是**沉淀进模型权重**——harness 开发本身成为一种可扩展的训练信号：更好的模型造出更好的 harness，每个 harness 又把它发现的增益返还给权重。这正是一条通往「**递归式自我改进（recursive self-improvement）**」的新范式路径。

当然，路才刚开头：审查模型的能力门槛、收集成本、深层领域知识的内化瓶颈，都是下一步要啃的硬骨头。但至少，HARNESS-ZERO 已经把「外挂」与「权重」之间的那道墙，凿开了一个可通行的口子。

---

**参考资源**

- 论文：<https://arxiv.org/abs/2609.24974> ｜ 代码：<https://github.com/metaevo-ai/harness-zero>
- 交叉验证文献：DAgger（[roboticscenter.ai](https://www.roboticscenter.ai/en/glossary/dagger)）、On-Policy Distillation（[VERL](https://verl.readthedocs.io/en/latest/algo/opd.html)）、Self-Harness（[arXiv:2606.09498](https://arxiv.org/abs/2606.09498)）、Agentic Routing（[arXiv:2607.11399](https://arxiv.org/abs/2607.11399)）、Co-Harness（[arXiv:2607.22688](https://arxiv.org/abs/2607.22688)）、On-Policy Correction 同期工作（[arXiv:2609.09134](https://arxiv.org/abs/2609.09134)）、OPHSD（[arXiv:2605.08741](https://arxiv.org/abs/2605.08741)）。
