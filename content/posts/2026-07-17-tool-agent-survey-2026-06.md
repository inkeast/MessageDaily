---
title: "2026-06 arXiv 智能体/工具智能体领域综述：916 篇分主题精读"
date: 2026-07-17
draft: false
tags: ["前沿调研","学术调研","Agent","Harness","Multi-Agent","强化学习","论文精读"]
categories: ["survey"]
summary: '工具智能体领域综述。LLM_Agents 桶 1001 篇扣除记忆子领域后按 13 主题分桶精读，提炼工具量质失衡、agent RL 信用分配、长程可靠性与上下文管理、过程级评测、工具环境不可靠、多智能体幻觉优势、治理权限可审计性等 7 大共识问题，并识别 strained coherence、agentic abstention、world-model collapse 等原创问题。'
---

# 2026-06 arXiv「智能体 / 工具智能体」领域综述

## 0. 范围、方法与数据

- 数据源：`work/june2026_papers.jsonl`（2026 年 6 月，cs.AI/SE/LG/CL/CV/stat.ML，共 11,930 篇，已逐篇真实分类到 25 桶）。
- 本任务范围：分类桶 B01 LLM_Agents = 1001 篇，扣除已单独成稿的「Agent Memory」（113 篇，与本桶重叠 85 篇），目标 916 篇。
- 已排除重复子领域：Agent Memory（已记录于 experiment_log.md）、代码生成（B23，见 codegen 综述）。coding-agent 框架仍属本稿。
- 方法：13 主题分桶 -> 逐主题精读 title + abstract（8 个方法学主题约 760 篇逐篇读摘要，EVAL/深度研究/应用类从标题归纳）-> 对声称新公式/新数学框架的论文下载 arXiv 全文 + 网络交叉验证。
- 主题分桶（首匹配优先）：TOOL 155 / SKILL 96 / HARNESS 79 / AGENT_RL 87 / PLAN_WM 57 / MULTI_AGENT 165 / TRUST_GOV_SAFE 57 / FAILURE_ABS 35 / COMP_USE 32 / WEB_MOBILE 27 / OS_PROTO_ORCH 25 / EVAL 54 / DEEP_RESEARCH 5。874/916 命中，42 篇未匹配多为领域应用。
- 交叉验证：5 个最关键的新框架已联网确认 2026-06 原创（arXiv 全文核对 + 0 引用），文中标注【已验证】；其余新公式基于摘要自述，标注【摘要，未联网核验】。

---

## 1. 提出的问题

### 1.1 大家都在提的问题（普遍共识）

**P1. 工具使用的「量」与「质」失衡。** 最密集议题。模型倾向过度调用工具（tool abuse / over-search / performative reasoning），即便内部推理已可解决：EAPO 用难度感知奖励抑制滥用；SlimSearcher 指 accuracy-focused 训练催生 brute-force 与盲目工具依赖；TAO-RL 指过度依赖引发输入分布漂移、过度保守又限制探索。反面是工具收益被高估：Do Multimodal Agents Really Benefit from Tool Use? 发现 93%–96% 的工具解题也可被非工具设置解出。

**P2. 信用分配（credit assignment）是 agent RL 的系统性瓶颈。** GRPO 把轨迹级 advantage 均匀广播给所有 token，在多步工具场景系统性失效。TAPO 形式化 credit misassignment 并量化（过半失败轨迹可纠正）；APPO 指粗粒度信用单元难定位关键决策；SGCD 指直接自蒸馏会摧毁工具使用；Evidence-Calibrated Policy Optimization 证明 dense credit 仍不够。

**P3. 长程（long-horizon）可靠性与上下文管理。** 上下文向窗口极限增长导致 stale-state、context degradation、成本爆炸。Signal-Driven Observation 指 DOM/accessibility 树每步上万 token 侵蚀推理；Plans Don't Persist 指上下文压缩会丢弃 plan；Self-Compacting 指固定阈值 compaction 丢弃派生中途结果；Handoff Debt 定义中断任务重新接手的重新发现成本。

**P4. 评测只看终态、掩盖过程。** 最终答案对错无法指出轨迹哪里出错。Where Do Deep-Research Agents Go Wrong（span-level error localization）、From Confident Closing to Silent Failure（false success：tau2 单控域 45–48% 失败、AppWorld 自评 75.8%）、SAFARI（轨迹超 context 后 attention dilution）、REFLECT（silent failure 归因）共同指向：过程级可观测性是刚需。

**P5. 工具/环境不可靠（tool-environment unreliability）。** 现有 benchmark 假设干净稳定可信环境，现实相反。ToolMaze（DAG 拓扑 + 显/隐、瞬/永 2x2 扰动）、ToolBench-X（可恢复可靠性危害）、When Tools Fail、Don't Blindly Trust It（不可靠反馈下更强 backbone 反而更盲从）。

**P6. 多智能体协作的幻觉优势。** MAS 不必然优于 SAS。The Illusion of Multi-Agent Advantage、GRPO Does Not Close the Multi-Agent Coordination Gap（dining philosophers，前沿模型 reward 仅 0.45–0.6）、How Much Coordination Gain Is Real?、When Is Emergent Consensus Real? 一致指出：多智能体增益常被噪声地板掩盖，需配对对照协议。

**P7. 治理、权限与可审计性。** agent 从聊天走向行动后，安全 = 行动对齐而非拒绝输入（Agent Safety Is Action Alignment）。lingering authority（临时能力在子目标后仍暴露）、confidence laundering（不确定性在接口丢失）、calibration is not control（标量风险预测针对错误对象）、自演化即权限提升路径（Agent libOS）。

### 1.2 偶尔几篇提出的新颖问题（原创问题定义）

- **Strained coherence（strained 一致性）**【已验证 2606.07889】：agent 在自身推理中承认问题、却仍照旧执行——安全相关失败子类，区别于能力不足/静默 bug/环境故障。flagged 轨迹失败率 94% vs 46%。
- **Agentic abstention（智能体弃权）**【已验证 2606.28733】：把何时停止行动定义为序列决策问题（POMDP），而非单轮 answer-or-abstain。挑战不只是能否弃权而是何时弃权。
- **Entity binding failure（实体绑定失败）**【摘要 2606.30531】：选对工具却操作错误的外部实体（wrong Alex / wrong thread / wrong account）——现有评测只查工具选择，漏掉实体维度。
- **False success（虚假成功）**【摘要 2606.09863】：agent 断言完成但环境状态不符，且 LLM judge 系统性失效。
- **Handoff debt（交接债）**【摘要 2606.02875】：编码 agent 接手中断任务的重新发现成本。
- **World-model collapse as phase transition**【已验证 2606.31399】：长程 agent 失败不是误差平滑累积，而是越过临界点后的突变；world-state fidelity 先于 action validity 失败。
- **Confidence laundering（信心洗钱）**【摘要 2606.20662】：脆弱上游决策以干净中间制品暴露给下游，不确定性在接口丢失，需要 latent carrier。
- **Instruction bleed（指令渗漏）**【摘要 2606.26356】：编辑一个 prompt 模块会静默改变其他模块行为（组合性行为漂移）。
- **Verification horizon**【摘要 2606.26300】：经典验证比生成容易的直觉在编码 agent 上被反转——生成已不难，可靠验证成了银弹难寻的瓶颈。
- **Constraint tax（约束税）**【摘要 2606.25605】：Tool Calling 与 JSON Schema 约束同时启用时，多个开源模型停止调用工具。
- **Hivemind effect（蜂群效应）**【摘要 2606.09039】：自治 agent 经济中过度策略趋同；需 entropy-controlled 多元对齐。
- **Silent Scope Omission (SSO)**【摘要 2606.08932】：应用通用规则却静默丢弃嵌套例外，产出看似合规实则违规。

---

## 2. 新变化与新范式（2026-06 的范式级位移）

> 本节关注范式（paradigm），即「谁拥有控制、什么是单元、何为正确性」的根本性重新约定；数学公式只作为佐证。
> 【已验证】= arXiv 全文 + 联网确认 2026-06 原创（0 引用）；【摘要】= 基于摘要自述，未联网核验。

### 2.1 从「LLM 当 orchestrator」到「程序当 orchestrator、LLM 当被调用的自适应组件」
- **LLM-as-Code / Agentic Programming**【已验证 2606.15874】：把 loop/branch/sequence 的确定性控制权从 LLM 手里拿走，交给程序。LLM 只在需要推理/生成的叶节点被调用，且「无法改变程序执行路径」。token 爆炸、控制流幻觉、不可靠完成被定性为「范畴错误（category error）」而非实现 bug。
- 关键范式后果：**上下文由 call depth 决定，而非 step accumulation**；执行历史从「扁平对话日志」变为 call tree 的 DAG（每帧只保留已返回子节点的摘要）。OSWorld 上 15 步（86.8%）击败 100 步基线（80.4%）。
- 同向证据：Stop Hand-Holding 提出「loop specification」（trigger/goal/verification/stopping 是可复用制品）；XFlow 用可执行协议编程划定 prompt–harness 边界；Process Harness 把确定性工作流引擎外包推理；Agentic Redux 用 typed lambda calculus 证明可审计。
- **范式断言**：把确定性的活交给概率系统是范畴错误，更强的模型/更好的 prompt 只能降低单步错误率，无法消除长程复合。

### 2.2 从「能力授予」到「能力即权限（capability-based runtime）」
- **Agent libOS**【已验证 2606.03895】：核心设计律「tools are libc-like wrappers; runtime primitives are the authority boundary」。把 agent 当作 AgentProcess（进程身份 + 父子血缘 + 生命周期 + Object Memory + 显式能力 + human queue + checkpoint + audit）。文件/对象/睡眠/审批/JIT 工具注册/外部副作用都在 primitive 边界按显式能力+策略检查。
- 关键范式后果：**「模型可见的 affordance 可以演化，但资源权限只能经显式 audited runtime primitive 变化」**——自演化不再是权限提升路径。
- 同向证据：Revocable Resource-and-Effect Capabilities（lingering authority 回收）、RACG（因果门控 least-privilege）、AgentBound（行为治理）、Sovereign Assurance Boundary（certificate-bound admission）、AOHP（OS-level agent harness）。
- **范式断言**：agent 从「chat loop + tool registry」演化为「软件 actor」，需要 OS/能力论那一套抽象（identity/scheduling/authority/interrupts/isolation/recovery/audit），而非 prompt 工程。

### 2.3 从「行动」到「交易（transaction）」
- **Agentic Transaction Processing（ATP）/ Mnemosyne**【已验证 2607.00269】：生成动作 = 「不可信提案」，直到通过约束集 C 的确定性准入才提交。原则两侧：「提案不是真相；没有任何提案能预见所有扰动——什么都能提案，但只有运行时准入并提交」。已证明四条相对 C 的安全性质 + 有界反应式修复（LCRP）。
- 关键范式后果：**「相对 C 的提交态正确性，独立于提案层的胜任/诚实/学习」**——把数据库事务/工作流补偿（Gray-Reuter 传统）引入 agent，承认提案层不可信。
- 同向证据：Proof-Carrying Agent Actions（action certificate + 5 checkpoint）、RAILS（verification-native clearing for agentic commerce）、Notarized Agents（receiver-attested receipts，解决「日志生产者=被记录者」的结构性失信）。
- **范式断言**：agent 行动的正确性不应押注在生成层，而应由提交层（runtime + 约束集）兜底。

### 2.4 从「存储/检索」到「预测/协同演化」
- **COMAP**【已验证 2606.02372】：世界模型不再「训练后固定」，而是与 agent 策略闭环协同演化——每步用世界模型预测候选动作的未来状态反馈，agent 做 future-aware reflection（估计反馈可靠性并精化动作），产生的 on-policy 轨迹再自蒸馏更新世界模型。
- 关键范式后果：**「内部未来预测 = 同时精化策略与适配世界模型的自改进信号」**，无需外部奖励/标注。Qwen3-4B 上 +16.75%。
- 同向证据：Self-Evolving World Models（WorldEvolver）、Policy and World Modeling Co-Training、Text World Models（TWMs）综述、Procedural World Models（ProPlay）、Agent vs Parametric World Models（混合规划）。
- **范式断言**：长程 agent 的瓶颈不是「记不住」，而是「预测不准且不随自己演化」；世界模型与策略是非平稳耦合，必须 co-evolve。

### 2.5 从「语义相关」到「因果充分」的工具选择
- **Causal Minimal Tool Filtering（CMTF）**【摘要 2606.06284 / Contract2Tool 2606.07904 / GIST-CMTF 2606.16813】：只暴露「下一个因果必要的工具前沿」，而非语义匹配的全部工具。Contract2Tool 从元数据/schema/文档/执行轨迹推断 precondition-effect-risk-cost 契约。
- **Pre-call control（ToolGate）**【摘要 2606.03054】：工具输出进上下文前就决定 execute/skip——把「相关性筛选」前移到「调用前门控」。
- 关键范式后果：**工具选择的目标函数从「与请求相关」改为「因果上充分且当前步骤必要」**；menu size 不再线性拖累可靠性。
- 同向证据：ToolMenuBench（menu 构造评测）、SING（主动工具发现的合成意图图）、HyperTool（执行粒度从原子调用改为代码块）、Beyond Static Endpoints（tool programs as interface）。
- **范式断言**：大工具生态下，「选对工具」的判据必须从语义升级到因果/契约。

### 2.6 从「轨迹级广播」到「反事实见证/参数确定性」的信用
- **TAPO**【已验证 2606.05784】：利用信息获取工具的「参数确定性」（相似调用参数 ≡ 等价信息获取动作），在 batch 内构造反事实见证，用 confidence-gated conservative advantage correction 补偿 GRPO 的 credit misassignment。
- 关键范式后果：**信用纠正的来源从「外加标注/模型/采样」变为「batch 内工具调用的参数等价性」——免费的对照**。
- 同向证据：Popoviciu 上界驱动的样本聚焦（RODS）、GiGPO（step-level anchor 优势）、3SPO（状态分数监督）、T²-GRPO（turn-trajectory）、APPO（procedural branching）、SGCD（sibling-guided bounded credit weighting）。
- **范式断言**：agent RL 的信用分配不应靠更细的离散化，而应利用工具/环境的结构（参数等价、sibling 对比）构造可证伪的对照。

### 2.7 从「平滑漂移」到「相变前兆」的失败观
- **World-Model Collapse as Phase Transition**【已验证 2606.31399】：长程失败不是误差平滑累积，而是越过临界点的突变；world-state fidelity 先于 action validity 失败。相图三区：solved plateau / transition band / collapse floor。更强模型只平移临界边界、不消除相变。
- **Strained Coherence**【已验证 2606.07889】：pre-failure signal——agent 显式承认冲突却仍照旧执行（5 类冲突）。flagged 轨迹失败率 94% vs 46%；首个 flag 出现在轨迹 83–84% 处。
- 关键范式后果：**「更多搜索/自检在临界后已太晚」**——必须在相变前介入；失败有可观测的前兆，而非事后归因。
- 同向证据：Bistable state monitors（State Saturation Trap，无 moment-detection regime）、Calibration Is Not Control（标量风险预测针对错误对象）、Confidence Laundering（不确定性在接口丢失需 latent carrier）。
- **范式断言**：agent 可靠性应建模为相变系统（控制参数/序参量/前兆），而非误差累积曲线。

### 2.8 从「完成任务」到「弃权/停止作为一等公民」
- **Agentic Abstention**【已验证 2606.28733】：把「何时停止行动」定义为 POMDP 序列决策（M=(S,A,O,T,Ω,R)），而非单轮 answer-or-abstain。挑战不只是「能否弃权」而是「何时弃权」。CONVOLVE 把轨迹蒸馏成可复用 stopping rules playbook（Llama-3.3-70B 及时弃权召回 26.7%→57.4%）。
- 关键范式后果：**「不做」也是动作，需要显式学习何时停止**；benchmark 必须测「本不该进行」的任务（infeasible/underspecified）。
- 同向证据：Will the Agent Recuse Itself（in-band access-deny 信号）、What Benchmarks Don't Measure（abstention competence）、Search Discipline、Knowing When to Ask、Uncertainty Decomposition for Clarification。
- **范式断言**：agent 能力的另一半是「知道自己不该继续」；这无法靠 refusal 训练（那是 chatbot 范式）解决。

### 2.9 从「文本技能」到「参数化技能」
- **LatentSkill**【摘要 2606.06087】：文本技能经预训练超网络转换为 plug-and-play LoRA adapter，绕过每步注入 prompt 的上下文开销与明文暴露。
- 同向证据：Skill-to-LoRA（从用技能到学行为）、Parametric Skills、LatentSkill、SkillCAT（对比式自演化）、SkillWiki（活的知识基础设施）。
- 关键范式后果：**技能从「读得到的 prose」变为「可加载的参数」**——技能库 = adapter 库，技能演化 = 参数更新。
- **范式断言**：技能的内化通道应从 in-context 文本转向 in-weight 参数，才能突破上下文预算。

### 2.10 从「自改进假设静态评估」到「agent-验证者共演化」
- **Red Queen Gödel Machine**【摘要 2606.26294】：自改进方法普遍假设静态评估准则（固定 verifier/benchmark）；而评估器本身可被博弈。提出 agent 与 evaluator 共演化。
- **PACE**【摘要 2606.08106】：自改进的弱在 acceptor（决定是否提交变更的规则）而非 proposer；用 anytime-valid acceptance tests 替代「小留出集取最高分」。
- 关键范式后果：**自改进系统的瓶颈是验收器而非生成器**；held-out 选择需递归化（RSEA 三层自然语言制品）。
- 同向证据：Beyond Goodhart's Law（动态合规基准）、Verification Horizon（验证比生成更难）、PseudoBench（自研究催生伪科学）。
- **范式断言**：把 evaluator 当固定 ground truth 的自改进会过拟合/被博弈；evaluator 必须与 agent 同步演化。

### 2.11 从「chatbot 安全」到「行动对齐（action alignment）」
- **Agent Safety Is Action Alignment**【摘要 2606.28739】：agent 调工具/转钱/删记录/发消息——把 chatbot 时代的「训练拒绝不安全输入」配方搬过来是错的对象。
- 同向证据：Policy-as-Code autoformalization（PolicyGuard，从概率 guardrail 到形式化策略）、Deontic Trees（对抗 SSO）、Intent-Governed Tool Authorization、Linguistic Firewall（几何作为路由防御）、MESA（脆弱通信信道优先级）。
- 关键范式后果：**安全的对象从「输入/输出文本」变为「动作序列与权限流」**。
- **范式断言**：agent 安全需要全新的对齐对象——行动及其授权链，而非话语。

### 2.12 其它范式位移（更小但明确）
- 从「最终答案」到「claim-centric 过程溯源」：DRIFT（claim 支持度追踪）、Typed Execution Provenance Graph（执行溯源 typed graph + evidence tracing 投影）、GRADE（execution edges + dependency edges）。
- 从「静态 benchmark」到「配对噪声地板协议」：How Much Coordination Gain Is Real?、When Is Emergent Consensus Real?、The Illusion of Multi-Agent Advantage——无配对对照的 MAS 增益不可信。
- 从「token 通信」到「latent 通信」：Beyond tokens（agent 间直接交换 embedding/hidden state/KV-cache，绕过文本生成瓶颈）。
- 从「挂载 prompt」到「harness 递归」：Recursive Agent Harness（递归单元是带工具的完整 agent harness，而非 model call）。
- 从「best-of-N」到「orchestration reward modeling」：OrchRM（自监督奖励建模训练编排器）。
- 从「text debate」到「architectural debate」：Mixture of Debaters（在架构层而非实例层辩论，避免多模型副本）。
- 从「内部推理校准世界模型」到「环境探针」：Ask the World Before Acting（belief drift 无法靠更长推理或自反思修复，缺的是环境里的证据）。

### 2.13 一句话总结范式位移
2026-06 的工具智能体正从「**让 LLM 当 orchestrator、把工具当 API、把动作当输出**」迁移到「**程序当 orchestrator、工具当能力（受权限约束）、动作当交易（受约束集准入）、世界模型当协同演化的预测器、弃权当一等动作、失败当相变前兆**」。数学化（Contagion Tensor、相变图、ATP 安全性质、TAPO 反事实见证）服务于这些范式断言，而非相反。

## 3. 解决思路（特别是新思路）

### 3.1 普遍思路
- 更细的信用分配：step/turn/state/procedural branching（GRPO->GiGPO->3SPO->T2-GRPO->APPO）。
- 更强的过程监督：process reward（ARBOR、StainFlow、VisCritic）。
- 更聪明的数据合成：trajectory synthesis（WRIT、ISE、StateGen、RODS、FORT-Searcher）。
- 分层：planner-executor、skill hierarchy、hierarchical recovery。
- 检索/记忆增强：experience graph、skill library。

### 3.2 新出现的思路（范式级）
- **调用前控制（pre-call control）**：工具输出进上下文前就决定执行/跳过（ToolGate）；因果最小工具过滤只暴露下一个因果必要的工具前沿（CMTF / GIST-CMTF / Contract2Tool）。本质：把工具选择从语义相关改为因果充分。
- **反事实见证做信用纠正**：TAPO 用 batch 内参数等价的反事实补偿错配信用。本质：用工具的参数确定性构造免费对照。
- **把确定性外移出 LLM**：LLM-as-Code、XFlow、Process Harness、Agentic Redux（typed lambda calculus 可证明可审计）。本质：LLM 做概率推理，确定性控制面做 loop/branch/verify。
- **能力即权限（capability-based security）**：Agent libOS、Revocable Capabilities、RACG、AgentBound。本质：用 OS/能力论改造 agent 运行时。
- **交易化 agent 行动**：ATP、PCAA、Mnemosyne、RAILS。本质：把数据库事务/合同思维引入 agent 行动。
- **弃权/停止作为一等公民**：Agentic Abstention（CONVOLVE 把轨迹蒸馏成 stopping rules）、Will Agent Recuse、Knowing When to Ask。本质：不做也是动作，需显式学习。
- **相变/前兆思维**：World-Model Collapse、Strained Coherence、Bistable monitors。本质：失败是相变不是漂移，要在临界前介入。
- **可证伪的耦合度量**：Contagion Tensor / Coupling Gain Validity Diagnostic。本质：先有度量学，才能区分真涌现与伪迹。
- **慢-快通道与执行粒度重定义**：Latent Bridge、HyperTool、Skill-to-LoRA。本质：重定义一次行动的粒度与延迟。
- **验证者与 agent 共演化**：Red Queen Godel Machine（自改进假设静态评估是错的）、PACE（anytime-valid acceptance tests，弱在 acceptor 不在 proposer）。本质：自改进系统的瓶颈是验收器而非生成器。

---

## 4. 解决技术（特别是新技术）

### 4.1 训练侧新技术
- **Counterfactual credit transfer**（TAPO 2606.05784）：batch 内参数等价反事实 + confidence-gated conservative advantage correction。
- **On-policy distillation 家族**：OPD-Evolver / ATOD / SAGE-OPD / OPID / PACT（privileged trace co-training）/ SGCD（sibling-guided bounded credit weighting）。
- **Failure-driven RL**：SENTINEL（任务分布与策略能力错配->失败驱动课程）。
- **Reflection-augmented PO**：ReGRPO（工具失败后修复）、Closing the Reflection Gap（calibration bonus）。
- **Latent skill via hypernetwork**：LatentSkill 文本技能->LoRA adapter。
- **Stateful MCP training environments**：PROVE（20 stateful MCP servers / 343 tools，session-scoped state isolation + state-machine 数据合成 + 自适应效率惩罚）。
- **Cached rollouts**：CacheRL（小模型 92% 过程准确率，逼近 GPT-5 的 94%，算力 1/100）。

### 4.2 推理 / 架构侧新技术
- **Pre-call gating**：ToolGate 外部轻量控制器（轨迹文本 + 结构特征预测 execute/skip）。
- **Causal tool filtering**：CMTF / Contract2Tool / GIST-CMTF（precondition-effect 契约 + goal-state inference）。
- **Diffusion-guided planning**：DiG-Plan（masked denoising 解耦组合探索与结构生成，Pass@10 0.320->0.943）。
- **Neural tool invocation via learned compression**：NTILC（learned latent retrieval 替代 in-context 注册表查找，避免线性上下文膨胀）。
- **Latent communication**：agent 间直接交换 embedding / hidden state / KV-cache，绕过文本生成瓶颈（Beyond tokens 统一框架）。
- **Semantic OS layer / observation interface**：LUMOS（给 agent 紧凑语义接口而非像素）、Agent-Computer Observation Interfaces（observation 接口作为设计轴）。
- **Tree-structured trial-error-return**：TreeSeeker、Collective Skill Tree Search（OpenClaw-Skill CSTS）。
- **Speculative execution / rollback**：Cost-Aware Speculative Execution（五维）、Speculative Rollback Correction、Certified Speculative Execution。

### 4.3 可观测性 / 验证侧新技术
- **Span-level / claim-centric auditing**：DRIFT（claim 支持度追踪）、TELBench、ARBOR（reusable rubric buffer）。
- **Causal replay / counterfactual attribution**：Causal Agent Replay（哪一步致错）、REFLECT（controlled replay + outcome flip 精化归因）、Knowledge-Based Zero-Replay Debugging。
- **Typed execution provenance graph**：From Agent Traces to Trust（执行溯源 typed graph + evidence tracing 投影）、GRADE（execution edges + dependency edges）。
- **Conformal risk control**：ToolChain-CRC（工具链漂移下保形风险）。
- **Bayesian controller for orchestration**：cost-sensitive sequential hypothesis testing。
- **False-success / abstention 检测**：CONVOLVE（stopping rules playbook）、Agentic Abstention benchmark（28,000 任务）。

### 4.4 治理 / 安全侧新技术
- **Capability-based runtime**：Agent libOS（AgentProcess / Object Memory / 能力表 / audited primitives）。
- **Proof-carrying actions / action certificate**：PCAA、Notarized Agents（receiver-attested receipts，解决日志生产者=被记录者的结构性失信）。
- **Policy-as-code autoformalization**：从组织政策到 neuro-symbolic 合规审查引擎（PolicyGuard）、Deontic Trees（对抗 SSO）。
- **Revocable resource-effect capabilities**：lingering authority 的回收。
- **Agentic transaction processing**：ACID 式提交检查。

### 4.5 评测侧新技术
- **长程 / 超长程基准**：OSWorld2.0、SWE-Marathon（百万 token/小时级）、ChainWorld（原子任务组合）、StaminaBench（100 交互轮 stamina）。
- **工具-环境不可靠基准**：ToolMaze、ToolBench-X、Beyond Function Calling。
- **过程级 / 溯源基准**：TELBench、PAVE（step-wise 因果传播路径标注）、SentinelBench（时间演化监测）。
- **弃权 / 自我感知基准**：Agentic Abstention、From Knowing to Acting（self-awareness）。

---

## 5. 关键判断与警示

1. **重心迁移**：从让 agent 能用工具迁移到度量/约束/验证/信任 agent 的工具行为。可观测性、信用分配、弃权、治理是 2026-06 的四大新热点。
2. **新生技术力量**：因果工具过滤、反事实信用纠正、能力论运行时、交易化行动、相变/前兆检测、保形风险控制、参数化技能。
3. **需警觉**：
   - strained coherence / false success / confidence laundering —— agent 会自信地错，且更强模型可能更盲从（Don't Blindly Trust It）。
   - building-to-the-test / verification horizon —— 生成变易、验证变难，benchmark 驱动的自改进有过拟合与伪科学风险（PseudoBench）。
   - 多智能体幻觉优势 —— 无配对噪声地板协议的 MAS 增益不可信。
   - world-model collapse —— 长程失败是相变，更多搜索/自检在临界后已太晚。
4. **盲区**：本稿基于标题+摘要（关键论文已联网验证全文），未对全部 916 篇逐篇读全文；EVAL/应用类从标题归纳；新公式中除 5 个【已验证】外，其余基于摘要自述。

---

## 6. 与既有子领域的关系

- Agent Memory（113 篇，已单独成稿）：本稿已剔除，避免重复。
- 代码生成（B23）：coding-agent 框架（SWE-Marathon/StaminaBench/ClayBuddy 等）因属 agent 范畴保留，纯代码生成方法归 codegen 综述。
- 本稿与 memory/codegen 三者互补，合计覆盖 B01+B23 的方法学主体。

---

## 附录 A：已联网交叉验证的范式论文（2026-06 原创，0 引用）

| 范式论文 | arXiv | 范式主张 | 验证来源 |
| --- | --- | --- | --- |
| LLM-as-Code (Agentic Programming) | 2606.15874 | 程序当 orchestrator；LLM 当被调用的自适应组件；DAG 上下文由 call depth 决定 | arXiv 全文 + papers.cool |
| Agent libOS | 2606.03895 | tools=libc 包装；runtime primitives 是权限边界；AgentProcess 抽象 | arXiv 全文 + GitHub(16 stars) + HF |
| Agentic Transaction Processing (Mnemosyne) | 2607.00269 | 生成动作=不可信提案直到通过约束集 C 确定性准入；4 安全性质 + LCRP | arXiv 全文 + GitHub |
| COMAP (Co-evolving World Model+Policy) | 2606.02372 | 世界模型与策略闭环协同演化；未来预测=自改进信号；+16.75% | arXiv 全文 + GitHub + papers.cool |
| Contagion Tensor + CAF | 2606.28839 | T in R^{M x N x T}, cell=D_JS(w||w0); CAF=E[T_cond]/E[T_base] | arXiv 全文 + HF |
| TAPO (credit misassignment) | 2606.05784 | 参数确定性 + 反事实见证 + confidence-gated advantage correction | arXiv 全文 + pubdb |
| World-Model Collapse | 2606.31399 | 相变：控制参数/序参量/前兆；相图三区 | arXiv 全文 + HF |
| Strained Coherence | 2606.07889 | 显式承认 + 非解决行动；5 类冲突；pre-failure signal | arXiv 全文 + AgentPatterns.ai |
| Agentic Abstention | 2606.28733 | POMDP M=(S,A,O,T,Omega,R)；序列弃权；CONVOLVE stopping rules | arXiv 全文 + HF + moltbook |

## 附录 B：范式位移总览（从 X 到 Y）

| 维度 | 旧范式 | 新范式（2026-06） |
| --- | --- | --- |
| 控制权 | LLM 当 orchestrator | 程序当 orchestrator，LLM 当被调用组件 |
| 权限 | 能力授予（tool registry） | 能力即权限（runtime primitive 边界） |
| 动作 | 输出 | 交易（约束集准入 + 提交） |
| 世界模型 | 训练后固定 | 与策略协同演化 |
| 工具选择 | 语义相关 | 因果充分 + 调用前门控 |
| 信用 | 轨迹级广播 | 反事实见证/参数确定性 |
| 失败 | 平滑漂移 | 相变前兆 |
| 弃权 | 不存在/单轮 | 序列决策一等公民 |
| 技能 | 文本 prose | 参数化 adapter |
| 自改进 | 假设静态评估 | agent-验证者共演化 |
| 安全 | chatbot refusal | 行动对齐 |
