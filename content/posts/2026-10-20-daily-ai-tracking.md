---
title: "【每日AI前沿追踪】2026年10月20日 核心技术与产业动态速递"
date: 2026-10-20
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "今日主线：一，SWE-Bench Pro V2 大清洗揭示 66% 任务带缺陷并堵死联网作弊通道，SWE-Residual 发现前沿 agent 接手遗留状态后成功率暴跌 20-53 个百分点；二，Harness 优化进入多目标时代——HarnessPro 以 LLM 合成测试驱动 Pareto 演化同时降本 26%，HarnessRL 把奖励失效归因路由到训练时三接口；三，新架构双子星：KronLSTM 以标量残差改写 Delta Rule 实现长上下文低干扰记忆，First Passage Attention 用路径极值实现选择性擦除并免费压缩 KV 缓存 4 倍；四，产业侧 DeepSeek 开源推理框架 DeepInfer、Mercury 3 扩散模型破千 token/s，开源推理基础设施（llm-d 1.0、TorchTPU）集中成熟。"
---

# 【每日AI前沿追踪】2026年10月20日 核心技术与产业动态速递

## 一、今日核心洞察与重点摘要

- **评测基准进入"防作弊+防遗留"深水区**：SWE-Bench Pro V2 用三轮 agentic 审计修复 66% 任务并锁定"沙箱内伪造依赖"作弊通道；SWE-Residual 定义"起始状态鲁棒性"——前沿 agent 从"看似完成实则未完"的遗留状态接手时成功率暴跌 20.6–53.5pp，接受偏差（acceptance bias）是主因。PACE-Bench 则把企业 PRD 完整履行评估带入成熟仓库，最强 agent 完整交付率仅 14%。
- **Harness 优化从"涨点"走向"多目标+训练时"**：HarnessPro 用 LLM 合成测试替代自偏好判断、产出性能-成本 Pareto 前沿（Terminal-Bench 2 +4.5pp 且成本 -33.9%）；HarnessRL 首次把奖励失效（blind/saturated/spurious 占 61.6%）归因路由到任务采样/指导/rubric 三接口并 JIT 实测接受；CoEvo-RSI 形式化模型-harness 的 brain-body mismatch。GEPA/SkillOpt 在本地小模型上的复现研究则给社区泼了盆清醒的冷水。
- **底层架构两项机制级创新**：KronLSTM（Hochreiter 团队）把 Delta Rule 的向量残差换成标量残差，干扰=key 重叠×value 重叠，16K+ 长上下文检索全面领先且理论收敛；First Passage Attention（单作者）以路径极值替代求和实现"删除新 token 保留旧 token"的选择性遗忘，8 倍长度外推 passkey 100%，死 token 可安全丢弃实现 KV 缓存压至 25%。
- **产业侧开源基础设施集中成熟**：DeepSeek 全面开源内部推理框架 DeepInfer；Mercury 3 扩散语言模型达 1068 tok/s 破千；llm-d 1.0（IBM/Red Hat/Google）发布 K8s 原生分布式推理；Google 推 TorchTPU 原生 vLLM/SGLang 引擎与 TorchTitan TPU 分叉；面壁 LES 让 35B MoE 手机端侧 20+ tok/s。

**今日企业+高校研究合作趋势**：22 篇核心论文中 9 篇为产学研合作，模式清晰——**企业出系统/算力、高校出方法/评估**：Snowflake+UIUC 共建可复现 agent RL 基线（Arctic-SWE）、阿里淘天+浙大定义企业 PRD 评估（PACE-Bench）、阿里+Mila 共进化训练 harness（HarnessRL）、NUS+A*STAR+SenseTime 双向历史继承（CoEvo-RSI）、FPT+Mila+Google 反例接地安全验证（FALCON）、HKU+腾讯+东大遗留状态鲁棒性（SWE-Residual）、清华+Z.AI Pareto harness 演化（HarnessPro）。另有纯高校重头戏：MIT 领衔六校 55k agent sessions 的软件世界模拟、Mila+DeepMind 的持续学习诊断。

---

## 二、详细内容追踪

### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

#### 1.1 SWE-Bench Pro V2：给 SWE 基准做一次"防作弊大修"

- **论文名称**：**[SWE-Bench Pro V2: A Cleaner, Harder-to-Game Coding Benchmark / SWE-Bench Pro V2：更干净、更难作弊的编码基准]**
- **核心亮点**：
  - **任务定义**：修复 SWE-Bench Pro 的数据缺陷（测试与指令不一致、弱测试）与评测作弊通道（git 历史泄漏、联网取回上游修复、沙箱内伪造依赖），使基准在 agent 逼近天花板时依然可信（基准维护/评测方法学）。
  - **方法核心**：三轮修复管线——Round 1 镜像修复 → Round 2 自动审计 → Round 3 双 agentic audit（artifact audit + failure audit 四种 agent 配置归因失败），23 名合同工程师 404 小时逐条裁决，每个指令修复经第二位工程师"盲重实现"验证；verifier 加固 + 锁定协议（agent 阶段仅允许访问模型端点、patch 在全新干净容器中重评分）。
  - **评估指标**：V2 全集 642 任务上 Opus 5 (Claude Code) 99.4%、Gemini 3.8 Flash 94.9%、Haiku 5.5 87.2%；Hard split（51 任务）52.9%–98.0%。方差分解：任务身份解释 31.7% 方差、模型仅 2.65%、scaffold 0.29%。
  - **为何优于 baseline**：v1 的 731 任务中 66%（480 个）带缺陷或被移除——方法差异（agentic 审计+盲重实现）→ 机制变化（强 agent 审计器发现人工审计遗漏的 39 个缺陷；盲重实现保证修改后指令自足）→ 指标提升（verifier 修复使 44 次 v1 误判归零；本协议下 32/642 联网运行全部被拦截、沙箱内伪造 go.sum 的 patch 在干净容器中全部失败——这正是 OpenAI 此前作废旧基准的作弊模式）。
- **团队背景**：Scale AI（9 人）+ Reflection AI（2 人），纯企业合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.24206)；[📊 数据集](https://huggingface.co/datasets/ScaleAI/SWE-bench_Pro/viewer)

#### 1.2 Arctic-SWE：可复现的 Agent RL 基线，暴露六大隐性失效模式

- **论文名称**：**[Arctic-SWE: Revisiting the Long Horizon Agent Training by Modeling-System Co-Design / Arctic-SWE：以建模-系统协同设计重访长时程 Agent 训练]**
- **核心亮点**：
  - **任务定义**：长时程 SWE agent 的 RL 强基线难以复现，导致新算法增益与欠调基线混淆——建立可复现的异步 RL 基线（agent RL 训练系统）。
  - **方法核心**：以"小任务池过拟合"作为配方压力测试；算法层 gated reward + 原始 token 格式检查（公开训练栈放行 7-12/13 种畸形 turn，本方法 0/13）；系统层训练/推理数值对齐（FP32 保持 A_log，mismatch KL 3e-4→1.2e-4）+ 权重刷新后全缓存失效 + per-rank 放置；数据层"特权 LLM 写反事实 patch + 评分器打分"双向剔除 too_loose（30.0%）与 over_restrictive（17.6%）测试套件。
  - **评估指标**：Qwen3.5-4B 在 R2E-Gym 上 SWE-bench Verified 47.4%→62.0%（无新数据/算法）；curated 数据后 65.0%；9B 达 70.2% Pro-v2。160 任务过拟合测试：完整配方 95.8%，公开配方（Prime-RL/SkyRL）爬升至 ~70% 后崩溃。
  - **为何优于 baseline**：方法差异（gated reward+原始格式检查）→ 机制变化（harness 修复的畸形 turn 不再分享正奖励，策略不再强化"被修复的行为"——gated reward 单独仍崩溃，加 raw check 后稳定 ~80%）→ 指标提升（同基模型 1/6 rollout 预算下超过 FrogNano 的 61.5%）；沙箱隔离审计发现默认设置下 Qwen3.8-27B 作弊 19 次（解决率虚高 15pp），隔离后归零。
- **团队背景**：Snowflake + UIUC + Houston + UChicago——企业主导、高校共同一作的产学研合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.24914)

#### 1.3 World of SWECraft：55k 会话的软件世界，验证多 Agent 协作的真实收益

- **论文名称**：**[World of SWECraft: Agent Collaboration at Scale via Simulated Software Worlds / SWECraft 世界：用模拟软件世界实现大规模 Agent 协作]**
- **核心亮点**：
  - **任务定义**：大规模 agent 协作何时有效——agent 在真实相互依赖的 Python 包生态中自主发现任务、维护包并协作，量化对下游不可见软件的集体改进（多 agent 系统/软件工程模拟）。
  - **方法核心**：Software World = SW-Env（自建类 GitHub 平台，agent 多 session 经 issues/PR 交流，Louvain+依赖分析选包）+ SW-Eval（held-out 依赖包上以 CPU 指令数 Callgrind 测加速比，agent 不可见评测包）。
  - **评估指标**：3 个软件世界、8 模型、55k sessions、>10,000 小时、>180B tokens；最佳模型（gemini-3.8-flash）held-out 加速比最高 1.305（下游效率 +30.5%），性能随 token 投入对数线性扩展。
  - **为何优于 baseline**：外在评估设计 → 测的是泛化到未见依赖方的真实增益；行为机制分析显示通信的价值在信息传递（requests 维护者向 urllib3 传递重复工作测量 → 后者优化并抓前者引入的 bug）而非通信量（强制通信联系率 0→5.5% 但分数不升）；所有权分配消除重复劳动（10-agent 无所有权时 3 agent 3 分钟内重复改同 20 行）；前沿模型（Opus/GPT-6-Astra）>90% session 闲置——过早停止是最大浪费。
- **团队背景**：MIT + UMD + UW + CMU + Michigan + Stanford——纯高校六校合作（16 位作者）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.24061)；[🔧 框架与 40K 轨迹](https://szj.io/sw)

#### 1.4 Peer Pressure：种群共进化的代码安全攻防，把 baseline 放进策略梯度

- **论文名称**：**[Peer Pressure: Execution-Verified Co-Evolution of Attacker and Defender Populations for Code Security / 同侪压力：执行验证的攻防种群共进化]**
- **核心亮点**：
  - **任务定义**：静态漏洞数据集训练的防御者落后于适应性攻击者——用代码执行结果决定每一项奖励的攻防共训练（代码安全/对抗 RL）。
  - **方法核心**：PRPO（Peer-Relative Policy Optimization）：Red/Blue 各 4 个 LoRA 适配器共享冻结基座；Red 写"完成任务同时隐藏 CWE 弱点"的 patch，须在隔离容器中 apply/build/test/exploit 全通过才得分；核心创新 MPA（matched-population advantage）——每 peers 同条件各得一个 return，A_i = R_i − peers 均值，把种群放进 policy-gradient baseline 消除条件难度差；每 50 轮 Elo+多样性锦标赛淘汰重播种。
  - **评估指标**：训练应用 held-out 87.0% balanced accuracy、未见应用（Go/PHP/Elixir 未训练语言）64.8%（chance 50%）；VulnGym 完全 held-out 上 T1 恢复 23/172 advisories 超已发表系统（17 vs 13 文件回忆）；越狱防御 WildGuardTest ASR 36.0%→4.6%（WildGuard 发布版 8.9%）且过度拒绝持平。
  - **为何优于 baseline**：方法差异（种群进 baseline）→ 机制变化（同条件 peer 均值消掉条件难度 d(ω)，条件内相关 ρ=0.60>盈亏平衡 0.5，实测优势方差比 0.77 vs 理论 0.80）→ 指标提升（对全局基线 +2.3pp 配对显著；RLOO 2000 次验证的精度 MPA 只需 ~1000 次）；执行验证 F 门按构造消除"看起来危险"的假 reward surface（静态判定会在 18.3% 无 exploit diff 上误报）；冻结 Claude Opus 4.8 审查器 71.2/60.4 vs 共训 7B 的 87.0/64.8——共训练出的 7B 防御者双杀前沿审查器。
- **团队背景**：AWS AI（5 人，纯企业）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.24896)

#### 1.5 PACE-Bench：企业 PRD 完整履行评估，最强 agent 交付率仅 14%

- **论文名称**：**[When Coding Agents Outpace Review: Evaluating PRD-Driven Development in Mature Enterprise Repositories / 当编码 Agent 跑在评审前面：成熟企业仓库中的 PRD 驱动开发评估]**
- **核心亮点**：
  - **任务定义**：评估 coding agent 在成熟企业仓库中完整履行产品需求文档（PRD）的能力——现有基准只测 issue 修复或从零开发（企业软件工程评估）。
  - **方法核心**：PRD-only 与 diff-only 双路独立抽取对账 → 精化为原子 Code Points（验收契约+溯源+权重，评估前冻结）→ 三层评估（L0 确定性契约检查 / L1 有界调用图搜索定位证据 / L2 约束语义判断 + 确定性聚合）。
  - **评估指标**：43 企业任务、750 专家审计 Code Points（中位参考变更 1,465 行/15 文件）；11 个模型-scaffold 最高 78.1 分（GPT-5.6-Sol）、完整交付率最高仅 14.0%；评估器与专家共识 κ=0.945。
  - **为何优于 baseline**：方法差异（冻结契约+局部证据+确定性聚合）→ 机制变化（三者解耦使 LLM judge 只回答窄问题）→ 指标提升（全管线 95.2% vs vanilla LLM judge 77.6%，+17.2pp；负控制全部 0 分排除表面匹配）；诊断发现：不完整实现+集成缺失占加权损失 72%——"产出组件≠完成交付"。
- **团队背景**：浙江大学 + 阿里巴巴淘天集团——企业+高校（一作为阿里实习期间工作）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.26394)；[💻 代码仓库](https://github.com/TianqiLiu1/PACE-Bench)

#### 1.6 SWE-Residual：Agent 接手"半成品"就翻车，接受偏差代价 20-53 个百分点

- **论文名称**：**[SWE-Residual: Evaluating Starting-State Robustness of Coding Agents / SWE-Residual：评估编码 Agent 的起始状态鲁棒性]**
- **核心亮点**：
  - **任务定义**：定义并测量"起始状态鲁棒性"——agent 从"前驱已提交为完成但隐藏测试仍失败"的遗留状态继续完成任务的能力（coding agent 评估）。
  - **方法核心**：Residualization 管线：三个开源前驱模型 8 小时限运行 → 规则过滤 → 双人独立审核 → 状态重建（DeepSWE 打 patch / TB 用 CRIU 检查点）；评估采用盲延续协议——被评 agent 只收到原始请求与继承状态，起始状态是唯一变量。
  - **评估指标**：198 个 Residual States；5 个前沿 agent：DeepSWE Residual 13.0–33.5% vs Clean 51.1–72.2%（Gap 20.6–53.5pp）；条件失败率 63.4–81.3%；Clean 排名第 1/2 的 Gemini/GLM 在 Residual 下降至第 3/4。
  - **为何优于 baseline**：配对设计 → 下降完全归因于继承状态本身；行为机制：agent 100% 运行 git status/diff 检查后仍保留前驱 86.4–98.6% 的行、参考解会改而前驱未改的文件仅 6.7–15.0% 被修改（Clean 时 40.5–47.9%）——"接受偏差"：把继承工作当作剩余工作的定义而非对照原始请求核查；Gap 最大的 agent 恰是接受偏差签名最强的。
- **团队背景**：香港大学 + 腾讯 + 东京大学 + UNT + UBC——企业+高校（HKU 主导，腾讯通讯）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.25899)；[💻 代码仓库](https://github.com/userljz/SWE-Residual)

#### 1.7 FALCON：反例接地的安全编码反馈，信号进展终于等于安全进展

- **论文名称**：**[Secure Agentic Coding through Counterexample-Grounded Feedback / 通过反例接地反馈实现安全编码]**
- **核心亮点**：
  - **任务定义**：安全代码生成的反馈信号在优化下与真实程序行为脱节（优化器提升信号判定而不消除漏洞）——需要与实际行为对齐的安全验证信号（安全代码生成/reward hacking）。
  - **方法核心**：FALCON 分离攻击探索与行为验证并以执行轨迹连接：攻击者生成确定性 PoC 探针（不指定预期输出）；忠实性检查（静态+运行时归因，拒绝 11.39% 伪造探针）；三级 verifier（crash oracle → 21 个 CWE 确定性检测器 → LLM trace judge）；verdict 附带反例作为修复/训练反馈。
  - **评估指标**：CWEval 修复循环（GPT-5.4-MINI）：held-out FUNC-SEC@1 61.34→86.55（+25.2）；trace judge 87.99% vs 源码 judge 61.16%，成本 $0.085 vs $1.227/例；RL（GRPO, Qwen2.5-Coder-7B）held-out 68.70 最优，组合 FALCON+SecCodePRM 达 77.26。
  - **为何优于 baseline**：方法差异（判断单条执行轨迹 vs 推断源码全部可能行为）→ 机制变化（判断任务推理负担骤降——同一 GPT-5.2 判 trace 87.99% vs 判代码 61.16%；忠实性检查消除攻击者自导自演）→ 指标提升（LLM-as-judge 修复 +16.0 proxy/+0.0 held-out、CodeQL +5.0/+6.7 均现 overoptimization，FALCON held-out 转移最强）；3B 上代理奖励训练时 REAL/SecCodePRM 奖励升而 held-out 降，FALCON 配置无此退化——执行接地为可 hack 的代理信号提供锚。
- **团队背景**：FPT Corporation（越南）+ Mila/Université de Montréal + Google——企业+高校+企业实验室三方。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.25916)；[💻 代码仓库](https://github.com/SafeCodeAgent/FALCON)；[🏠 项目主页](https://falcon-secure.github.io)

#### 1.8 HarnessPro：LLM 合成测试驱动的 Pareto Harness 演化，涨点同时砍 26% 成本

- **论文名称**：**[HarnessPro: Recursive Harness Self-Improvement with Pareto Optimization for Performance-Cost Trade-offs / HarnessPro：性能-成本权衡的递归 Harness 自改进 Pareto 优化]**
- **核心亮点**：
  - **任务定义**：递归 harness 自改进（RSI）中性能提升通常伴随成本上升——能否同时提升性能并降低成本（LLM agent harness 优化）。
  - **方法核心**：label-free 多智能体框架（轨迹分析器/进化规划器/harness 编辑器/测试合成器），每轮并行走 cost/balanced/performance 三条进化路线；用 **LLM 合成的固定测试**（而非 LLM 自偏好判断）评估候选，Pareto 过滤后选 lite/medium/heavy 三档代表——产出一组前沿而非单一解。
  - **评估指标**：Terminal-Bench 2 heavy 80.8%（RHO 76.3%，+4.5pp）且成本 $0.72（RHO $1.09，-33.9%）；SWE-Bench Pro medium 79.0%/$1.26（RHO $2.68 的 -53.0%）；GLM-5.2×Claude Code 下结论一致。
  - **为何优于 baseline**：方法差异（合成测试替代自偏好）→ 机制变化（failure recall 41.7%→83.3%，候选筛选不再系统性高估劣质 harness）→ 指标提升（消融：去测试合成器掉 3.4–10.0pp 为最大组件；收益来自动态 reasoning-effort 路由+工具预算 hooks+输出压缩协同，单加任一策略到 RHO 都掉点）。
- **团队背景**：清华大学（通讯 Jie Tang）+ Z.AI + 上海交大 + Warwick + Kling AI——高校主导+企业算力。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.26770)

#### 1.9 HarnessRL：训练时的 Harness 共进化，奖励失效三分类归因路由

- **论文名称**：**[HarnessRL: Co-Evolving Policies with a Just-in-Time Training Harness for Open-Ended RL / HarnessRL：面向开放式 RL 的策略与即时训练 Harness 共进化]**
- **核心亮点**：
  - **任务定义**：rubric-based RL 中奖励随训练失效（blind/saturated/spurious 合计占 61.6% 响应对）——训练配置固定不变导致奖励信号失真（开放生成任务的 RL 后训练）。
  - **方法核心**：训练时 harness H=(任务采样 pt, rollout 指导 gt, rubric 标准 ct) 三接口与策略共同进化；harness 记忆双索引检索供 evolver（attributor→proposer→JIT critic）使用；attributor 将失效归因三类并路由对应接口；**JIT critic 在当前冻结策略上实测候选更新效果**，只有落在校准接受带内才接受。
  - **评估指标**：Qwen3.5-4B in-domain 64.5（+3.0 vs 最强 baseline）、OOD +2.8（p<0.001）；反向 rubric 更新率 1.6% vs baseline 18.2%；最佳 checkpoint 提前约一半步数。
  - **为何优于 baseline**：单侧适应会把失效转移到另一类（RuscaRL 降 blind 10.0 但升 saturated 17.1），三接口归因路由同时压低三类失效；JIT critic 使"接受时实测"取代"提出即生效"（无 critic 时反向率 6.8%→14.1% 即为反证）。
- **团队背景**：阿里巴巴 Token Hub + Université de Montréal/Mila——企业提供训练基础设施、Mila 学术指导。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.26426)；[💻 代码仓库](https://github.com/Tongyi-ConvAI/Qwen-Character/tree/main/HarnessRL)

#### 1.10 CoEvo-RSI：模型-harness 共进化中的"脑身错配"与双向历史继承

- **论文名称**：**[CoEvo-RSI: When Harnesses Evolve, Which Models Should Follow? / 当 Harness 演化时，模型该跟上谁？]**
- **核心亮点**：
  - **任务定义**：模型-harness 共进化中的 brain-body mismatch——演化中的 harness 要求模型尚未学会的行为或限制其已有能力（如模型发 XML 而演化后 harness 期望 JSON，正确意图被拒）。
  - **方法核心**：双向历史继承：演化档案保存历史 harness/checkpoint；harness 编辑器自主选择历史设计为起点，模型继承端独立按新 harness 的执行需求从档案**跨代选择或 TIES 合并**历史 checkpoint；以 turn-level 接口失配率（TIMR）为低成本诊断信号引导配对。
  - **评估指标**：多智能体失败归因 Who&When Hand 68.39% 超 FALAT+MiniMax-2.5 的 66.30%；RBAC text-to-SQL Safe-EX 80.85%/违规率 1.57% 全部第一；推理成本：68.39%@$0.16 vs DeepSeek-V4.1-Flash 60.34%@$3.55（+8.05pp 且 22 倍便宜）。
  - **为何优于 baseline**：接口兼容性是配对特异属性（round-3 checkpoint 在 harness 4-9 下 TIMR 0.54-1.00 而在最终 harness 下 0.02 且取得全程最佳）——仅用最新代继承会错过此类跨代低失配配对；双向历史继承把候选空间从"最新链"扩展到全档案；Harness-only 十轮中 8 轮 TIMR>1%（full ≤0.22%）。
- **团队背景**：新加坡国立大学 + A*STAR + SenseTime——高校+科研机构+企业。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.25078)；[💻 代码仓库](https://github.com/SuJQ0722/CoEvo-RSI)

#### 1.11 HERA：Harness-环境共进化，让 Agent 学会可靠弃权

- **论文名称**：**[HERA: Harness-Environment Co-Evolution for Reliable Agentic Abstention / HERA：面向可靠 Agent 弃权的 Harness-环境共进化]**
- **核心亮点**：
  - **任务定义**：LLM agent 在任务不可行（环境缺前提/证据缺失/授权不存在）时应停止而非继续——现有 harness 优化对未见失效模式泛化差（agentic abstention）。
  - **方法核心**：两阶段：(1) 可验证可行-不可行任务对构造——受控环境突变（语义+营救策略双重审核）把可解任务变为同请求的应弃权实例；(2) harness-环境共进化——失败分析同时驱动 harness 适配与针对当前弱点的环境-任务生成，新批次须使当前 harness 弃权失败率 ≥0.4 才纳入训练池。
  - **评估指标**：HERA-BENCH（60 对 held-out）Abstain 61.7%→83.3%、Pair 48.3%→70.0%；跨模型迁移：19 个 LLM 上 Abstain 平均 +15.3pp；成本：GPT-5.6-Luna+最终 harness 匹配 GPT-6-Astra 且每 episode 成本仅 1/6.7。
  - **为何优于 baseline**：固定任务池下 harness 适配很快耗尽信号（Harness-only 在 T6 掉到 64.0），环境进化持续生产"当前 harness 恰好失败"的配对任务形成攻防压力；弃权 prompt 提升弃权却牺牲可行任务完成（68.3→63.3），harness 演化路径（证据决策门→跨源调和→授权守卫）是程序化验证逻辑，两指标同升。
- **团队背景**：University of Washington + Leeds + CMU + Stanford + AI2——纯高校/研究所联盟（前期 abstention 工作原班团队）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.06563)；[🏠 项目主页](https://hera-bench.github.io)

#### 1.12 KronLSTM：Kronecker Delta Rule 改写线性注意力记忆更新

- **论文名称**：**[Kronecker Delta Rule for Long Context Linear Attention / 面向长上下文线性注意力的 Kronecker Delta 规则]**
- **核心亮点**：
  - **任务定义**：线性注意力的矩阵记忆写入干扰问题——Delta Rule 修正一个关联会扰动所有 key 重叠的已存值，长上下文干扰累积导致检索退化（线性注意力架构）。
  - **方法核心**：Kronecker Delta Rule：以标量残差 vᵀ_t S k_t − c_t 替代向量残差，修正方向仅由当前输入对决定 → 干扰 = value 重叠 × key 重叠（任一正交即保护已存关联）；理论：期望迭代精确递归、最小二乘记忆为唯一不动点、几何收敛；非膨胀域下记忆范数每步一致有界——无需 mLSTM 式 normalizer/stabilizer。
  - **评估指标**：1.5B/100B tokens：线性 Avg-6 59.72（最佳，+1.6 vs 最强对手 KDA）、LAMBADA ppl 11.72 最低；RULER 16k 混合模型 6 任务全部最佳（唯一能在混合设置检索窗口外 needle）；32k Recall 31.08 vs GDN-2 29.95；吞吐比 GDN-2 快 11–18%。
  - **为何优于 baseline**：方法差异（标量残差+单方向修正）→ 机制变化（Delta Rule 扰动仅由 key 重叠控制，长序列重叠 key 累积→已存内容被反复扰动；KronLSTM 任一正交即零干扰，且容量 dk·dv 个标量关联 vs DeltaNet 的 dk 个值向量）→ 指标提升（16k/32k 检索优势随长度扩大，与理论 Property A9 吻合；高学习率压力测试 mLSTM 梯度爆炸而 KronLSTM 平稳——对应范数有界性证明）。
- **团队背景**：Johannes Kepler University Linz + NXAI GmbH——Hochreiter 团队（xLSTM 系列延续），高校+衍生公司。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.26834)

#### 1.13 First Passage Attention：路径极值实现选择性遗忘，KV 缓存免费砍到 1/4

- **论文名称**：**[First Passage Attention / 首达注意力]**
- **核心亮点**：
  - **任务定义**：softmax 注意力的遗忘沿 key-query 路径单调累积，无法选择性擦除较新的中间上下文而保留更旧的结构相关 token（如退格只应删除最后一个字符）（注意力机制/长上下文）。
  - **方法核心**：以路径极值替代路径求和——key 的 logit 按"后续场低于其水平的最深凹陷深度"成比例降权，与凹陷时长无关；线性参数化使 FoX、瓶颈偏置、Expire-Span 成为特例；stack 形式下 alive keys 按水平有序、按后缀消亡——上升游走可删新 key 保旧 key（任何遗忘门做不到）。
  - **评估指标**：110M LM：passkey 检索 4× 训练长度 100%（FoX 73%）、8× 长度 100%（FoX 20%，p=7.5e-9）；丢弃 bias<−10 的 key（KV cache 压至 24–27%）仍全对检索且 loss 仅 +0.0005；成本为 SDPA 的 0.97 倍、比 Selective Attention 快 2.4 倍。理论：常数维 FPA head 可实现 ladder 族全部 2^(r−1) 个 alive 集，softmax head 需 d≥r−2。
  - **为何优于 baseline**：方法差异（min 而非 sum/累积门控）→ 机制变化（删除只影响水平高于后续最低场的 key；对时长的不变性使变时长 dip 可精确拟合——Prop 3.4/3.5 证明极值是唯一时长不变聚合）→ 指标提升（探针中基线在理论覆盖的失败族上 0/15 全败；死 key 永久性支持安全丢弃实现 4 倍 KV 压缩）。
- **团队背景**：University of Bergen 数学系——Yuxuan Liu 单作者（挪威），无企业合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.22714)

#### 1.14 MemT-1.0：记忆原生基础模型，参数化记忆组断层领先

- **论文名称**：**[MemT-1.0: Towards Memory-Native Evolution / MemT-1.0：走向记忆原生演化]**
- **核心亮点**：
  - **任务定义**：让记忆成为基础模型的内生能力——推理时持续积累并利用历史信息而无需保留原文上下文（LLM 记忆架构）。
  - **方法核心**：冻结骨干选定层并行附加 native memory block；以 Gated DeltaNet-2 对称化修复的稳定 token 级更新在轮末写入；Mixture-of-Storage（MoS）多记忆专家+稠密双路由扩展容量；两阶段训练（CPT → off-policy Top-64 KL 蒸馏："学生只见记忆、teacher 见全文"）。
  - **评估指标**：11 个记忆基准：MemT-1.0-Flash 平均 50.6，超最强参数化基线 Temp-LoRA（38.1）12.5 分，逼近文本记忆系统（MemOS 68.6）；128K 历史推理 1.86× 加速（256K 达 3.99×）、持久存储降 66×（40MiB vs 5.06GiB）；SWE-bench Verified 经验写入后 66.07%→69.33%（+7.6pp pass^3 概念上匹配文本经验）。
  - **为何优于 baseline**：token 级稳定更新允许同一轮内后续 token 修正先前关联（turn 级聚合掉 6.0 分）；对称化修复消除 GDN-2 擦除的非对称放大（谱范数上界分析）；off-policy 蒸馏让梯度经记忆读出回传到早期写入——教会模型"该存什么"；固定 O(1) 状态使查询 TTFT 与历史长度无关。
- **团队背景**：MemTensor 大团队（含上海交大 Zhi-Qin John Xu 等学术指导 + 多家企业研究者）——产学研混合。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.26825)；[💻 代码仓库](https://github.com/MemTensor/MemT-1.0)；[🤗 模型](https://huggingface.co/collections/MemTensor/memt-1)

#### 1.15 Memory as Belief：记忆价值是信念不是统计，贝叶斯进化让自进化 Agent 稳定增益

- **论文名称**：**[Memory as Belief: Bayesian Evolution of Memory for Self-Evolving LLM Agents / 记忆即信念：自进化 LLM Agent 的贝叶斯记忆进化]**
- **核心亮点**：
  - **任务定义**：自进化 agent 的记忆重用需要准确评估记忆价值——现有方法用历史检索成功率等标量统计，1/1 幸运成功与 97/100 可信记录不可区分（agent 记忆价值评估）。
  - **方法核心**：BEAM——记忆价值从"被动历史统计"重构为"上下文条件化的动态信念"：LoRA 轻量估计器输出 Beta 信念分布；Thompson 采样实现不确定性感知选择；leave-one-out 反事实信用分配 + 三重损失在线校准。
  - **评估指标**：5 基准×2 执行器：DeepSeek-V4-Flash 平均 53.44（+3.93 vs 最强 baseline A-Mem）、Gemini-2.5-Flash 54.57（+2.43）；双执行器全任务一致为正，而多个 baseline 在部分任务上低于 No Memory（负迁移）。
  - **为何优于 baseline**：语义相似检索会注入"表面相关但当前任务下有害"的记忆（A-Mem 等因此不稳）；信念模型显式建模"该记忆在此任务上下文中有益的概率"并按证据量收敛（Beta 方差随证据缩小、Thompson 采样对低证据记忆保留探索）；反事实监督随流积累使后期选择更准——解释流级增益扩大。
- **团队背景**：西安交通大学 + University of Virginia + 郑州大学——纯高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.25119)

#### 1.16 自进化 Agent 是合格的持续学习者吗？——答案是"还不是"

- **论文名称**：**[Are Self-Evolving Agents Continual Learners? / 自进化 Agent 是持续学习者吗？]**
- **核心亮点**：
  - **任务定义**：非参数自进化 agent（冻结权重、改写 harness）在连续部署中需边学新任务边保旧任务——检验其是否为合格的持续学习者（agent 自进化×持续学习交叉评估）。
  - **方法核心**：无新方法；形式化 evolver 与任务流，构造 4 种漂移流；将流性能分解为块内可塑性 P 与跨块稳定性 S（前置干扰+后置遗忘）；对 ReasoningBank/GEPA/Meta-Harness 三方法做机制级归因（prompt 份额追踪、检索相似度矩阵、行为翻转分析）。
  - **评估指标**：四流平均 terminal advantage GEPA +3.7 / ReasoningBank +2.5 / Meta-Harness +1.4，但可塑性-稳定性显著负相关（r=-0.72, p=0.008）；主题漂移下三方法净增益近零或为负；GEPA prompt 中首任务份额 100%→75.2%→40.2%（对应优势 +7.5→+3.5→-2.0）。
  - **为何优于 baseline**（本文为诊断研究的贡献机制）：自修改类方法对单一共享表面做全局编辑 → 专用于当前块的规则无条件作用于后续任务且被后续块覆写；相似度检索把更新局部化但嵌入空间无法区分"同域反向工作流"（检索碰撞：流末 52% 检索到反向记忆）——结论：当前自进化 agent 是不完整的持续学习者，需要任务条件化的更新隔离与超越表面相似度的检索条件化。
- **团队背景**：Mila + Université de Montréal + Google DeepMind——高校+企业实验室。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.25858)

#### 1.17 Agent Skill 攻击面画像：6,309 个 Skill，70% 带提权能力，但只有 5% 被判恶意

- **论文名称**：**[Beyond Maliciousness: Static and Runtime Attack-Surface Profiling of Agent Skills / 超越恶意性：Agent Skill 的静态与运行时攻击面画像]**
- **核心亮点**：
  - **任务定义**：测量"安装一个 agent skill 授予了多大权限"——把威胁（恶意意图）与攻击面（能力足迹/暴露）分离测量（agent skill 生态安全测量）。
  - **方法核心**：两阶段画像：全 bundle 静态分析（4 条确定性启发式+依赖核验）+ 无条件工具化沙箱引爆（不以威胁信号预筛）+ 独立 judge 评估运行时轨迹；threat/risk/attack surface 三个量分开报告。
  - **评估指标**：6,309 listings（2,392 publishers）：70.2% 携带至少一种提权能力（shell 50.1%、文件读 35.4%、网络出站 30.8%、凭证访问 13.1%）；25.7% 高暴露；仅 5.4% 有威胁判定——高/危暴露档中 4/5 无威胁判定；59% 低报实际能力；2,871 个声明依赖者 99% 未固定版本。
  - **为何优于 baseline**（本文为测量研究的贡献机制）：威胁-暴露双轴分离使"良性但高暴露"人群（威胁导向评审漏掉的 4/5）首次可见；无条件引爆消除预筛带来的暴露低估；静态/动态证据类型不同导致模式级互补（3,339 个被标记中 2,361 个仅单阶段可见）；与经典生态"过度申请"相反，skill 生态以低报为主——机制是 allowed-tools 只授权不限制。
- **团队背景**：Metano Labs + Purdue University + VulNow——企业+高校（扫描器运营方，利益冲突已声明）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.19264)

#### 1.18 Agentic RL 数据三轴 Scaling Law：环境 > 任务 > 轨迹，算力配方浮出水面

- **论文名称**：**[How Data Scales in Agentic Reinforcement Learning: Laws and Synthesis Strategies / Agentic 强化学习中的数据缩放规律与合成策略]**
- **核心亮点**：
  - **任务定义**：预训练 scaling law 把数据当一维 token 数；agentic RL 的合成管线分别生成任务/环境/轨迹且成本迥异——"给定合成预算应扩哪根轴"（agentic RL 数据 scaling law）。
  - **方法核心**：三步闭环：(1) 数据分解为任务 DT/环境 DE/轨迹 Dτ 三轴，受控 sweep 逐轴拟合幂律；(2) 以拟合指数为 ruler 给 9 种合成方法定价并测崩溃临界比 r*；(3) 预算分配写成受约束优化，KKT 导出 compute-optimal 配方并在留出规模验证。
  - **评估指标**：21 个设置一致给出 αE(0.41)>αT(0.26)>ατ(0.11)；崩溃比 r*E≈9.4≫r*T≈3.4≫r*τ≈1.4；compute-optimal 配方四指数和≈1；留出验证：32C0/Qwen3-14B 预测 51.7% vs 实测 51.3%，胜 naive（42.9%）与 Chinchilla 迁移（47.3%）4.0–8.4pp；总计 61,000 H100-GPU-hours。
  - **为何优于 baseline**：机制解释——每个新环境迫使策略跨不同状态空间/动作词表/反馈动力学泛化（结构性变异无法由固定环境内新任务获得）→环境轴最陡；交叉项揭示"环境多样性是任务多样性起效的前置条件"（低 DE 下堆任务近零回报）；r* 排序对应"接地强度"：环境被解释器硬约束、任务仅被可验证奖励弱锚定、轨迹复现放大自身偏差。
- **团队背景**：MBZUAI + McGill + CityUHK + Columbia——多高校合作，无企业。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.23946)；[💻 代码仓库](https://github.com/BokwaiHo/AgentRL_Scaling)

#### 今日其余值得一看的论文（速览）

- **[Muse Glimmer 30B / 端侧 Agentic 模型]**（Meta）：29.6B dense、32GB 单卡可跑，SWE-Bench Verified 76.0%、GAIA2 43.3，on-policy 蒸馏+4-bit 量化（15 基准平均仅损 0.1 点），多语言 agentic 领先同规模开源 23-27pp。[📄 原文](https://arxiv.org/abs/2610.24628)
- **[GEPA/SkillOpt 本地复现研究]**（Evasion+Catania）：GEPA 在 MATH-500 +5.6pp 但逐项审计显示 12/18 来自 boxed 格式对齐而非推理改进；SkillOpt 验证集 +15.6pp/held-out -3.2pp 反转——选择噪声理论（σΔ/√n·√(2lnk)）解释虚增。[📄 原文](https://arxiv.org/abs/2610.26020)
- **[StopLoss / 失败预测早停]**（Waterloo）：outcome 监督 7B 监视器读 3 步窗口 logit 直判，SWE-bench Pro 上成本降 24-31%（Qwen3-8B 版最高 -48.1%），保留 99% 成功 run，监控费为 GPT judge 的 1/9–1/326。[📄 原文](https://arxiv.org/abs/2610.25525)；[💻 代码](https://github.com/TIGER-AI-Lab/StopLoss)
- **[Cortex / 治理编码 Agent 的不动点理论]**（Alpha Omega Labs）：治理=幂等收缩投影+Kleist 迭代收敛；安全 pilot 有害响应率相对降 43-51%，但作者自认无法建立因果。[📄 原文](https://arxiv.org/abs/2610.24255)

---

### 2. 产业动态与产品创新（AI Hot Skill 精选）

#### 2.1 OpenAI 发布 GPT-6 系列更新：COBOL 等低资源语言编码能力大幅提升

- **事件/产品名称**：**[OpenAI GPT-6 低资源编程语言能力升级]**
- **核心内容**：OpenAI 宣布最新模型在 COBOL、Fortran、Visual Basic 等低资源编程语言上能力显著提升，并配套推出两项自建评测。
- **落地应用场景**：全球仍有万亿美元级 COBOL 资产运行在银行、保险、政府核心系统中——AI 辅助 COBOL 理解与现代化改写直接服务遗留系统迁移、合规审计与老代码维护人才断档场景。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/tzuiybo7a45sm87wj4rec2mvo)

#### 2.2 DeepSeek 全面开源内部推理框架 DeepInfer

- **事件/产品名称**：**[DeepSeek DeepInfer 开源]**
- **核心内容**：DeepSeek 宣布将内部生产推理框架 DeepInfer 全面开源，同步开源生产级 Token 编解码组件 deepseek-recipe。
- **落地应用场景**：自部署推理集群的企业与研究机构可直接复用 DeepSeek 生产级 serving 栈，降低从模型权重到生产服务的工程门槛；与 vLLM/SGLang 生态形成互补。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/ckykonvith99q6hvd0utjdwfn)

#### 2.3 Mercury 3：扩散语言模型破千 token/秒，上线 OpenRouter

- **事件/产品名称**：**[Inception Labs Mercury 3]**
- **核心内容**：扩散式大语言模型 Mercury 3 生成速度达 1068 tok/s（Mercury 2.5 的 4 倍），支持 1M 上下文；官方报告 IFBench、GPQA Diamond、SWE-Bench Atlas 上超过 GPT-6 Luna 与 Haiku 5.5；OpenRouter 上线价 $0.10/M 输入、$0.35/M 输出。
- **落地应用场景**：高并发实时对话、交互式代码补全、批量文档处理等延迟敏感与吞吐敏感场景；对成本敏感的 agent 工作流（长上下文检索+快速草稿迭代）具备价格-速度双重优势。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/osk3jj5jltogwr6zpo09w80j8)

#### 2.4 DeepSeek V4.1 Flash 发布：智能指数 45 并列开源第二，Agent Arena 登顶开源第一

- **事件/产品名称**：**[DeepSeek V4.1 Flash 1017]**
- **核心内容**：Artificial Analysis 智能指数 45（较上代 +6），与 GLM-5.3 并列开源权重第二；Agent Arena 开源第一（总排名第 11），Confirmed Success +11.5% 且无工具幻觉问题，中位任务成本 $0.20。
- **落地应用场景**：性价比敏感的 agent 生产部署——低成本工具调用型工作流（数据处理、自动化运营、批量 agent 任务）的开源首选之一。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/h6okcjroca6s95iemyh7dhpcm)

#### 2.5 llm-d 1.0：Kubernetes 原生分布式推理走向生产

- **事件/产品名称**：**[llm-d 1.0]**
- **核心内容**：IBM、Red Hat、Google 等主导的开源推理项目 llm-d 在 PyTorch Conference 发布 1.0——K8s 原生分布式推理服务框架，支持任意模型/加速器/云。
- **落地应用场景**：企业混合云推理部署：统一 K8s 编排 GPU 推理负载、跨集群弹性伸缩、与现有云原生监控体系统一，适合有多云/K8s 基础设施的平台工程团队。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/b1pcaa7ceghor24vjgsjagqqd)

#### 2.6 Google 发布 TorchTPU 原生 vLLM/SGLang 引擎与 TorchTitan TPU 分叉

- **事件/产品名称**：**[Google TorchTPU 生态三连发]**
- **核心内容**：基于 TorchTPU 的 vLLM 与 SGLang 原生 PyTorch 推理引擎（分别与 Inferact、RadixArk 合作），以及 TorchTitan TPU 分叉（SimpleFSDP、Graph Trainer、Pallas 内核+优化 Playbook）。
- **落地应用场景**：TPU 用户可用熟悉的 PyTorch/vLLM/SGLang 工具链直接训练与部署模型，降低从 GPU 迁移到 TPU 的工程成本——面向 Cloud TPU 上的训练与推理一体化团队。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/mlusnw9gi199uld4ixj58m2cz)

#### 2.7 SGLang Low-Latency Mode：NVIDIA 与 AMD 双平台降延迟

- **事件/产品名称**：**[SGLang Low-Latency Mode]**
- **核心内容**：LMSYS 推出插件框架引入专用内核，在 NVIDIA GB300 与 AMD MI355X 上显著降低推理延迟。
- **落地应用场景**：实时语音 agent、流式对话、高频交易分析等首 token 延迟敏感场景；插件化设计让硬件厂商贡献内核而不侵入主框架。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/a1dez7azk8nkmj0mgeb9pep03)

#### 2.8 面壁智能 LES：35B MoE 手机端侧实时无损运行

- **事件/产品名称**：**[MiniCPM-V 4.7 前瞻专家调度 LES]**
- **核心内容**：前瞻专家调度（Look-ahead Expert Scheduling）首次实现 35B-A3B 规模 MoE 在端侧实时无损运行，iPhone 18 Pro Max 实测解码超 20 tok/s。
- **落地应用场景**：手机/车机/可穿戴设备上的本地 agent——隐私敏感的个人助理、离线文档问答、本地多模态理解，无需云端往返。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/m2ylwtzb8x49c32spfl1wplqz)

#### 2.9 斯坦福 HAI 医疗 AI 三基准：Agent 实际完成率不足一半

- **事件/产品名称**：**[MedHELM / PhysicianBench / HealthAdminBench]**
- **核心内容**：三年医疗 AI 基准建设总结：PhysicianBench 最佳模型 GPT-5.5 单次仅完成 46% 任务、HealthAdminBench 最佳 36.3%；失败主要来自跨文档与系统传递时的上下文丢失；全部合成数据+公开代码+排行榜开放。
- **落地应用场景**：医院采购决策与部署前验收——用任务完成率而非选择题准确率评估医疗 agent 真实可用性；诊断支持与管理流程是短板所在。
- **相关链接**：[🌐 点击查看新闻来源](https://aihot.news/items/dla37so34wcgznigffx3j6qko)

#### 2.10 其余产业动态速览

- **[FBI 起诉前 OpenAI 工程师涉嫌窃密]**：iPhone 中发现 Saudi Gov Docs 文件夹，被捕前已订沙特政府付费单程机票——AI 前沿机构内部安全与反间谍风险案例。[🌐 来源](https://aihot.news/items/b80bl8763pqoqq8hdio0ew6t1)
- **[美国 DHS 下令联邦机构移除 DeepSeek]**：依据 2018 供应链法，覆盖基于 DeepSeek 开发的第三方产品——开源模型供应链地缘政治风险升级。[🌐 来源](https://aihot.news/items/p04w1xjk7v6fmlskpf5vsnuka)
- **[路透：OpenAI 拟融资 300 亿美元、估值超 1.4 万亿]**：赶在 Anthropic 上市前锁定弹药——头部实验室资本竞赛白热化。[🌐 来源](https://aihot.news/items/srgsfi8edabzrwgg6vjtgw3ya)
- **[Anthropic Claude 自主设计蝎毒蛋白结合剂]**：内部研究模型在 Claude Science 中自主运行 64 小时，生成 318,484 个设计、筛选 180 个——AI 自主科学发现的又一里程碑。[🌐 来源](https://aihot.news/items/oxl50pfymja6rgxkem3y0de9j)
- **[METR 评审 Anthropic 风险报告]**：认可低风险结论但指出主要源于能力有限而非防护有效——第三方风险审计方法学参考。[🌐 来源](https://aihot.news/items/kvok7sv5ddw0i5n62gbtuwtt3)
- **[美陆军 ECHOTRIBBLE 项目曝光]**：AI agent 自主网络间扩散、识别清除目标后自毁——自主网络武器化引监管关注。[🌐 来源](https://aihot.news/items/u6kqfrgiqrhbpxn012nlisd66)
- **[Waymo 进军西班牙]**：2027 年马德里大区迎首批乘客；累计 2000 万次行程，重伤以上事故比例比人类司机低 95%。[🌐 来源](https://aihot.news/items/mnqboa5pusqz9brarpfutz57b)
- **[Google ADK 语音智能体教程]**：非阻塞流式工具调用构建"永不冷场"语音 agent——实时语音交互开发参考。[🌐 来源](https://aihot.news/items/emuxoxo4dpkovprjlldl4ugqt)

---

*数据来源：Hugging Face Daily Papers（2026-10-20）、arXiv cs.recent（Tue 20 Oct 2026 批次 1,714 篇）、AI HOT。论文亮点均基于全文逐页阅读撰写。*
