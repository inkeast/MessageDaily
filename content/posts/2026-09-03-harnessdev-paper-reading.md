---
title: "HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness? 精读"
date: 2026-09-03
draft: false
tags: ["论文精读", "Agent", "Harness", "Benchmark", "学术调研", "AI自进化"]
categories: ["paper-reading"]
summary: "ByteDance Seed 联合 SUTD/GaTech/M-A-P 发布 HarnessDev——首个把评测单元从'任务输出'改为'可运行基础设施'的基准：creator LLM 从无策略弱种子构建完整 harness（Creation），再基于下游执行反馈迭代改进自己的 harness（Evolution），在 2207 个下游实例上按 capability+efficiency 双轴评估。核心发现：模型自建 harness 在 writing/MLE 域追平甚至反超人类参考系统，但在 code/search 域差距显著；Evolution 的增益不稳定且严重绑定 executor；换 executor 后最高回退 10.32 分。这为'harness 工程能否自动化'提供了第一份系统性体检报告。"
---

> **论文链接**：[HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness?](https://arxiv.org/abs/2609.01437)
> **项目主页**：[self-developing-agents.github.io](https://self-developing-agents.github.io/)
> **发表时间**：2026年9月1日（arXiv:2609.01437v1）
> **机构**：**企业+高校重磅合作**——ByteDance Seed（企业出题、算力与工程基准）+ 新加坡科技设计大学 SUTD + 佐治亚理工学院 + M-A-P + TokenWave.AI（方法学与分析）
> **领域标签**：cs.SE / Agent Benchmark / Harness 工程

---

## 一、论文背景

### 1.1 Harness：被评测却从未被评测的东西

**Agent harness（智能体运行架）** 是包裹 LLM 的执行基础设施：它管理执行循环、工具调用、上下文、失败恢复与结果验证，把模型输出变成真实动作。业界的共识公式是"Agent = 模型 + Harness"。harness 的有多重要？论文开篇给了一个直观量级：**同样的 GPT-5 权重，在 Terminus 2 harness 里解 Terminal-Bench 2.1 的 35.2%，换到 Codex CLI harness 里解 49.6%**——14 个百分点的差距完全来自模型之外的那层代码。

这就产生了一个评测学上的怪现象：所有 agent 评测都在"harness 内"测模型——研究者选定一个 harness 作为实验配置，然后报告模型的任务分。harness 本身是评测的**背景板**，从来不是被评测的**对象**。但随着 agent 走向部署，恰恰是这块背景板消耗了最多的人类工程：现实里这批人叫**Forward Deployed Engineer（FDE，前置部署工程师）**——Palantir 发明的职位，如今被前沿模型公司普遍采用。FDE 驻场客户 site，把通用模型改造成能在客户数据格式、工作流与合规约束下真正跑起来的系统，并且持续维护它。

### 1.2 为什么 harness 工程难以自动化的关键难点

论文指出：harness 工程与普通代码编辑有本质区别。改一个独立程序时，目标行为是外部给定的、成功是局部可验证的；而**改自己的 harness 时，模型是在编辑"自己借以行动的执行基底"**——改动会改变模型自身在未来所有任务中观察、规划与恢复的方式。有效的 harness 改进要求模型：从执行轨迹里识别自己的行为短板、诊断所在系统的结构性瓶颈、做出能累积成持久能力的定向修改。这个能力随着前沿模型能编辑多文件代码库、能关真实 PR 已经"潜在具备"，缺的只是一个测量它的基准。

### 1.3 现有工作的空隙

近期工作开始研究 harness 的表示、自动化 agent 设计、以及"agent 造 agent 系统"（如 AutoHarness 合成 harness 代码、Meta-Harness 搜索 harness 空间等），但没有人回答过两个更基础的问题：**模型能否从零构建一个完整可运行的 harness？构建出来之后，能否基于反馈持续改进它？** 这两个问题需要把"开发 harness 的模型"与"执行 harness 的模型"分离、记录开发环境、测量下游性能/跨执行器迁移/与人类系统的距离/回归/成本——这就是 HarnessDev 的设计空间。

---

## 二、论文定位和关联工作

### 2.1 三条相关研究线

**线一：Agent 评测基准**。SWE-bench 系（仓库级 issue 解决）、Terminal-Bench（终端任务）、MLE-bench（机器学习实验）、BrowseComp（深度检索）——它们测"agent 在固定 scaffold 下的任务表现"，scaffold 是配置不是考题。**关键区别**：HarnessDev 把 scaffold 本身变成考题。

**线二：自动化 agent 设计与进化**。ADAS/AutoHarness/Meta-Harness 等自动搜索 agent 结构；GEPA/SkillOpt 等优化 prompt/skill。它们优化的是"agent 程序的某层"（多为 prompt 或组件级），HarnessDev 评测的是"完整可执行系统的创建与维护"，且首次系统引入 **executor 迁移性**维度（换一个跑这个 harness 的模型，harness 还灵不灵）。

**线三：对 FDE 工作的研究**。论文的 Background 一节把 FDE 工作拆成三块结构（目标模糊、反馈缺失、执行系统自建），并声明本基准聚焦第三块——执行系统的构建与维护。这个定位使 HarnessDev 与研究"需求澄清"或"反馈构造"的工作正交。

### 2.2 定位对比表

| 维度 | 传统 agent 基准 | agent 自动设计工作 | **HarnessDev** |
|------|----------------|-------------------|----------------|
| 评测单元 | 任务输出（答案/补丁） | agent 程序（多为 prompt/工作流层） | **可运行的完整 harness（执行循环+工具+上下文+状态+生命周期+验证）** |
| 起点控制 | 给完整环境 | 给搜索空间定义 | **无策略弱种子（跑分必为 0）** |
| 反馈类型 | 任务对错 | 训练信号 | **下游执行反馈（分数+轨迹+成本）** |
| 泛化测试 | held-out 任务 | held-out 任务 | **held-out 任务 + 换 executor 迁移** |
| 效率维度 | 少 | 少 | **执行 token 成本作为一等指标** |

**定位结论**：HarnessDev 是"harness 工程学科化"进程中的基准奠基工作——它不提出新方法，而是为"模型能否接管 harness 工程师"这个问题建立第一个可复现的测量框架。

---

## 三、问题定义

### 3.1 形式化

**具体场景**：一个 creator LLM 在开发环境 D 里写出一个 harness H；H 被冻结后，executor LLM 在 H 里跑下游任务 x 产出 y，由评测器 J 打分：

$$(L_C, D) \rightarrow H, \qquad (H, L_E, x) \rightarrow y \xrightarrow{J} \mathrm{score}$$

关键分离：D 只用于建 H；L_E 只在 H 冻结后使用。**creator 与 executor 可以是同一个模型（Self-Eval）或不同模型（Unified-Eval/固定 executor）**——这个自由度本身就是诊断工具。

**两个研究问题**：
- **RQ1（Creation）**：模型能否从弱种子 + 少量开发样例（1–3 个）构建对未见任务泛化的完整 harness？
- **RQ2（Evolution）**：模型能否用下游执行反馈改进自己的 harness 而不破坏已有能力？

### 3.2 抽象的精妙之处：弱种子设计

评测"造 harness 的能力"最大的陷阱是**起点给多给少都会失真**：空仓库会把 harness 设计能力与命令行/文件格式杂务混在一起；给成熟 agent 则把要考的规划与验证结构直接送分。HarnessDev 的解法是一个**刻意弱化但可运行的种子 H_seed**：它只做输入解析、暴露许可的低层工具、写出结果/轨迹/日志（统一的审计合同）——没有 agent 循环、没有任务分解、没有工具策略、没有上下文管理、没有持久状态、没有验证器、没有重试恢复、没有停止规则。**未经修改的种子在所有下游基准上得 0 分**——于是 Creation 阶段的任何非零分都必然来自 creator 添加的执行逻辑。这个"零基线"设计把"能跑"与"能干活"干净地区分开，是整个基准证明力的锚点。

### 3.3 harness 的解剖学定义

论文把 harness 的控制层拆成六个必须由 creator 实现的模块：**E**xecution loop（执行循环）、**T**ools（工具策略）、**C**ontext（上下文管理）、**S**tate（持久状态）、**L**ifecycle（生命周期控制）、**V**erify（验证接口）。这个 E/T/C/S/L/V 分解既描述了考题范围，也给后续研究提供了共同语言。

---

## 四、问题解法（基准设计）

### 4.1 双轴评估：capability 与 efficiency

评估生成的 harness 比评估生成的答案难得多——harness 可能过拟合写它的模型、记住开发样例、提升一种能力的同时静默回归另一种能力、或通过基准特定的 hack 提分但不迁移。HarnessDev 的回应是双轴：

- **capability**：冻结的 harness 在 held-out 下游任务上的任务级成功率；
- **efficiency**：部署该 harness 解下游任务时消耗的 executor token 数。

**为什么 efficiency 是一等公民**：实测发现 MLE-bench 上不同 creator 的 harness token 消耗差约 **19 倍**，且高成本并不可靠地换来高分——只看能力会选出"暴力堆 rollout"的 harness。

### 4.2 Creation 协议

Creator 收到：弱种子 H_seed + 任务族规格 + 工具与权限约束 + 简短设计教程 + 1–3 个开发样例。可在样例上迭代修改 harness，但**看不到人类实现与隐藏评测集**。成品 H 冻结后评估。为控制单次创建的方差（同一模型独立创建的 harness 表现可能差异巨大），报告 **avg@3**（独立创建三次取平均）。

### 4.3 Evolution 协议

从冻结的 RQ1 code harness H_0 出发。开发期可见反馈集：100 个 SWE-Pro 任务（731 公开实例的子集）+ 全部 89 个 Terminal-Bench 任务的完整评测结果。每个正式候选版本必须完成"两腿"评测（同一 commit 上 100 SWE-Pro + 89 Terminal-Bench 各跑一遍）才进入正式轨迹；预算为 10 个 post-H_0 评测对。全部轨迹结束后，每个正式版本在 **630 实例 SWE-Pro held-out 分割**上复评——这些分数对 creator 永不可见，不可能影响编辑、停止与最终版本选择。九条 lineage（5 条 self-runtime + 4 条 fixed-Gemini）共产出 73 个正式版本、64 次相邻版本切换。

### 4.4 参与模型与域

六个 creator：Opus 4.8 High、GPT-5.5 High、Gemini 3.1 Pro High、DeepSeek V4 Pro High、Qwen 3.7 Max、Seed 2.0 Pro High。四个域对应五个下游基准：code（SWE-Pro 731 公开分割 / Terminal-Bench 2.1）、MLE（MLE-bench 33 物理单元 2475 结果）、writing（EQ-Bench3）、search（BrowseComp）——合计 2,207 个独特下游实例。人类参照系：每域选定成熟人类工程系统（如 code 域的 OpenHands 级参考）作 system-level 参照。

---

## 五、评估指标与实验证据

### 5.1 RQ1（Creation）主结果：域间差距悬殊

**Self-Eval（creator 自己的模型执行自己的 harness）总表关键数字**：

| Creator | SWE-Pro succ. | Term-2.1 acc. | MLE medal | EQ-Bench3 | BrowseComp acc. | 平均 |
|---------|--------------|---------------|-----------|-----------|-----------------|------|
| Opus 4.8 High | 69.3 | 64.8 | 32.9 | 84.6 | 52.4 | **67.8** |
| GPT-5.5 High | 32.8 | 52.1 | 19.1 | 83.0 | 52.6 | 55.1 |
| Gemini 3.1 Pro High | 43.6 | 68.8 | 32.4 | 74.8 | 35.2 | 55.6 |
| DeepSeek V4 Pro High | 28.9 | 35.6 | 19.6 | 75.4 | 40.9 | 45.2 |
| Qwen 3.7 Max | 33.5 | 41.3 | 3.1 | 68.7 | 32.3 | 44.0 |
| Seed 2.0 Pro High | 10.8 | 6.0 | 5.3 | 71.1 | 3.2 | 22.8 |
| **人类参考系统** | 80.0* | 88.8* | 24.0 | 83.7 | 92.2* | **86.2** |

（* 为外部参考结果未重跑；平均分不计 MLE。）

三个层次的解读：

- **整体**：最强 creator Opus 4.8（67.8）仍落后人类参考（86.2）18.4 分；
- **域的差异是最大发现**：writing 域模型逼近参考（EQ-Bench3 84.6 vs 83.7，**反超**）；MLE 域 Opus/Gemini 的 medal rate 32.9/32.4 **显著反超**参考的 24.0；但 search 域差距巨大（BrowseComp 52.4 vs 92.2），code 域 SWE-Pro 差 10.7 分、Terminal-Bench 差 24 分。**解释**：writing/MLE 的成功判据相对局部（文本质量/实验流程），而 search 与 code 需要长程信息寻查与多轮"检查-编辑-验证"协调——恰是 harness 六模块中最难自动化的部分；
- **失败归因**：77.8% 的 Data 域失败任务可归因于 **harness 缺陷**而非 executor 能力——瓶颈在架子不在模型。

### 5.2 RQ1 的三个横切发现

- **单次创建方差异大**：同一个 Opus 独立创建的 harness 中，有一个在 Self-Eval 表现良好、换 Gemini 执行几乎崩溃——根因是它把 120 步上限硬编码（围绕原 executor 的习惯）。这证明"单个生成 harness 不具代表性"，avg@3 是必要的；
- **换 executor 重排座次**：固定 Gemini 执行时，Opus 的 SWE-Pro 从 69.3 跌到 33.0（‡ 标记），GPT-5.5 跌到 27.8；而部分 Seed/DeepSeek harness 在若干域反而更稳。**Self-Eval 分数 = harness 设计 × executor 能力 × 两者兼容性**三者纠缠，单独报告会系统性误导；
- **成本与质量脱钩**：MLE-bench 上 token 相差 19 倍，高分不一定高耗——harness 的"性价比"必须独立测量。

### 5.3 RQ2（Evolution）主结果：增益小、不稳、难迁移

| 设定 | Creator | 反馈对增益 H0→H_dec | Held-out-630 增益 | 终版差距 |
|------|---------|--------------------|--------------------|---------|
| Self | Opus 4.8 | +3.0 | **+4.44** | 1.59 |
| Self | Gemini 3.1 Pro | +8.8 | +2.70 | 0.00 |
| Self | GPT-5.5 | +5.9 | +3.81 | 0.00 |
| Self | Qwen 3.7 Max | +13.9 | +1.43 | 3.17 |
| Self | DeepSeek V4 Pro | +13.4 | +3.17 | 1.75 |
| Fixed Gemini | GPT-5.5 | +2.4 | **−10.32** | 16.51 |
| Fixed Gemini | DeepSeek V4 Pro | +6.5 | −2.38 | 3.02 |
| Fixed Gemini | Qwen 3.7 Max | +1.1 | −1.11 | 1.11 |
| Fixed Gemini | Opus 4.8 | +9.7 | +2.70 | 2.54 |

四个结论：

1. **可见反馈集上人人都能涨**（+1.1 ~ +13.9），但**held-out 上增益缩水**（最好的 Opus 也只有 +4.44）——部分"改进"是过拟合反馈集；
2. **换 executor 后多数血统回退**：fixed-Gemini 下 4 条 lineage 有 3 条 held-out 为负，GPT-5.5 造的 harness 甚至 −10.32 分且终版与最佳版差 16.51 分——进化收益深度绑定"当前 executor + 当前反馈集"；
3. **版本选择本身是未解难题**：多条 lineage 的"creator 自选终版"不是 held-out 上的最佳版本（final gap 列），说明模型还不会判断"哪个版本真的更好"；
4. **编辑统计/反馈响应等轨迹分析**显示改进多为局部修补，缺乏系统性重构。

### 5.4 实验设计如何支撑论点

这套协议的证明力来自三处隔离设计：① **弱种子零基线**保证非零分归因于 creator；② **held-out-630 永不可见**保证"反馈集增益 vs 泛化增益"可分离（揭示 +13.9→+1.43 的缩水）；③ **self vs fixed-executor 对照**保证 executor 依赖性可量化（揭示 −10.32 的回退）。没有这三个隔离，"模型能自进化 harness"就会被反馈集上的虚高误判为已实现。

---

## 六、效果优势的根源解释（本文为基准研究，解释"差距为何如此分布"）

**为什么 writing/MLE 能追平反超、code/search 差距巨大？** 从 harness 六模块的角度：writing 任务的执行循环短、验证靠输出质量（LLM judge 可用）、失败恢复简单——E/V 模块的实现门槛低，模型现有代码能力足以覆盖；MLE 的流程虽有长程性但结构高度模板化（下载数据→训练→提交），harness 需要的"聪明"有限。相反，search（BrowseComp）要求 harness 实现激进的多源检索-交叉验证-回溯策略，code（SWE-Pro/Terminal-Bench）要求跨数十轮的仓库理解-编辑-测试协调——这两者的 E（循环策略）/C（上下文管理）/S（状态）模块是真正的设计难题，模型目前只能给出"合格但平庸"的实现（如统一的探索-执行循环），难以复现人类系统里积累的启发式（如分阶段检索、按依赖分组的验证）。

**为什么 Evolution 增益绑定 executor？** 机制上：creator 改进 harness 的依据是"当前 executor 在反馈集上的失败轨迹"，它学到的修补必然针对该 executor 的失败模式（如某模型的冗长输出需要更强的上下文截断）。换 executor 后失败模式分布改变，针对性修补变成错配甚至有害约束（120 步硬编码就是典型）。这不是工程疏忽而是**从单一执行反馈源学习的结构性偏差**——除非反馈分布覆盖多 executor，否则进化出的 harness 就是"私人订制"。

**为什么 77.8% 的 Data 失败归因于 harness？** Data/MLE 任务里模型本身的"解题能力"（写训练代码、调参）已经足够强，瓶颈在于 harness 是否正确编排了环境配置、数据路径、提交格式等系统胶水——这恰是六模块中 L（生命周期）与 V（验证）的职责。这一发现对产业的含义直接：**企业 agent 落地的短板不在模型选型，而在 harness 工程产能**——这正是 FDE 紧缺的原因。

---

## 七、必要知识反推

**领域知识层**
- harness 的解剖学（六模块分解）：不知道 E/T/C/S/L/V 就无法定义"考题范围"，弱种子的"缺什么"也无从设计；
- 各下游基准的评测协议与失效模式：SWE-Pro/Terminal-Bench/MLE-bench/EQ-Bench3/BrowseComp 的计分方式、任务形态——没有这些就无法构造"隐藏评测集不可见"的可靠实现；
- FDE 工作的实际形态：从产业视角理解"目标模糊、反馈缺失、系统自建"三分法，才能把基准定位在正确的一块。

**方法论知识层**
- 评测学中的构造效度（construct validity）与选择偏差：理解"测到的分数"≠"想测的能力"，才会设计出弱种子零基线、avg@3、held-out 永不可见这些防污染机制；
- executor 迁移性测试的思想（类似软件的跨平台回归测试）：把"换模型跑同一 harness"作为一等实验轴；
- 过拟合-泛化分解：反馈集 vs held-out 的双报告设计直接借自 ML 的 train/test 方法论。

**工程知识层**
- 大规模评测基础设施：2,207 个下游实例 × 多 creator × avg@3 × Evolution 73 个正式版本的评测编排、冻结、去重与审计（同 commit 双腿评测的配对协议）；
- 沙箱与权限控制：creator 只能经许可接口行动、隐藏任务物理隔离；
- 成本核算：统一 token 计量口径使 19 倍成本差可比。

**知识融合的关键节点**：本工作最关键的融合是把"软件工程的可运行制品评测"（harness 能跑就行）与"机器学习的泛化评测"（held-out 不可见）两套范式焊接起来，再用 FDE 视角把问题翻译成产业语言。任何一环缺失——不懂 harness 解剖则考题失焦、不懂评测效度则基准可被 hack、不懂基础设施则跑不起 73 版本 × 双腿评测——都无法完成。

---

## 八、论文中可以提取的通用性灵感

**灵感一：把"背景板"变成"考题"——评测配置本身可以是评测对象**
- **核心思想**：任何系统中作为固定配置存在的组件（工具链、运行时、脚手架），都潜藏着"能否自动化该组件"的未测量问题；把它从配置提升为被评对象，能开辟新的能力维度。
- **论文证据**：harness 从评测配置变成考题后，立即产出域间差距（writing 反超/search 差 40 分）与 executor 依赖性等此前不可见的结构性发现。
- **推广场景**：评测"模型能否设计自己的评测协议"；评测"编译器能否自动调优 pass pipeline"；评测"运维系统能否自动写 runbook"；教育中评测"学生能否出好题"而非只会解题。

**灵感二：零基线弱种子——用"刻意残缺但可运行"的起点隔离能力归因**
- **核心思想**：测"建造能力"时，起点应保证未经加工的得分恒为零，且不混入环境杂务——这样任何非零分都可归因于被测者的增量工作。
- **论文证据**：H_seed 无任何任务策略、裸跑全零分，使 Creation 的每 1 分都可归因 creator。
- **推广场景**：编程教育（给能编译的空框架而非空文件，剔除环境配置噪声）；招聘笔试（给脚手架代码测算法而非工程）；自动化科学的实验设计评测；产品经理的"最小可用原型"测试。

**灵感三：能力 × 成本双轴——高成本不等于高质量**
- **核心思想**：对生成的基础设施类制品，只报成功率会系统性偏好"暴力解"；把资源消耗列为一等指标才能识别真正的设计质量。
- **论文证据**：MLE-bench token 差 19 倍且高耗不保证高分（DeepSeek 1449.9M token 反而低于 208.4M 的 Gemini）。
- **推广场景**：云架构评审（性能 × 账单双指标）；算法竞赛的复杂度分；招聘中"交付速度 × 维护成本"；科研基金评审的"产出 × 经费效率"。

**灵感四：从单一反馈源学习必然过拟合该源**
- **核心思想**：优化信号的分布决定结果的适用域——只用单一执行者/单一反馈集驱动进化，产物就是该源的"私人订制"，换环境即回退。
- **论文证据**：fixed-Gemini 下 3/4 lineage held-out 回退（GPT-5.5 的 −10.32 与 16.51 终版差距）。
- **推广场景**：推荐系统的用户分布外泛化；教师依赖单一教材的风格窄化；强化学习的 sim-to-real gap；组织内"只向一个上司汇报"催生的取巧行为。

**灵感五：报告"终版选择差距"——让决策失误可测量**
- **核心思想**：自主系统的能力不仅在于产生候选，还在于选对候选；把"自选版本与事后最优版本的差距"显式报告，可以量化"元决策"水平。
- **论文证据**：多条 lineage 的 final gap 达 1.59–16.51 分，说明 creator 普遍不会挑自己的最佳版本。
- **推广场景**：模型 checkpoint 选择与早停策略；科研中的实验配置选择；投资中的组合再平衡；A/B 测试的止损决策。

---

*本文基于 arXiv:2609.01437v1 全文（含基准设计、六 creator 完整数据表、Evolution 九 lineage 轨迹与附录配置）撰写。*
