---
title: "【每日AI前沿追踪】2026年11月13日 核心技术与产业动态速递"
date: 2026-11-13
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "今日主线：黄仁勋正式辞任 NVIDIA CEO（33 年任期终结、CFO Colette Kress 接棒临时 CEO），Google 发布 Gemini 4 Hydrogen 全面接管 Search 与 Gemini App（智能指数 54、速度较 3.8 Flash 快 2.5 倍、成本约 Sonnet 5.5 的 32%）。学术侧 32 篇深读：递归自我改进出现可验证交接奖励范式（ACE-RL 四基准全胜、MBPP 近满分 0.987）、决策模型教师化（浙大+百度 Actor/Judge/Teacher +10.1 ALFWorld）、RSI 前沿首次被量化（Singularity-Bench：Opus 5.5 六任务超人类基线 SI=1.252）与编码 Agent 终止滞后现象（成功运行 35-60% 步数浪费在已解之后的过度验证）。"
---

# 【每日AI前沿追踪】2026年11月13日 核心技术与产业动态速递

## 一、今日核心洞察与重点摘要

- **NVIDIA 进入后创始人时代**：黄仁勋因丑闻风波正式辞去担任 33 年的总裁与 CEO 职务并退出董事会，CFO Colette Kress 出任临时 CEO；员工内部建议箱出现 1600+ 条请愿回归留言，民主党启动对华芯片销售调查、参院银行委员会主席要求 12 月 3 日作证，SemiAnalysis 与 The Verge 深挖出口管制博弈与走私疑云——市场焦点从个人声誉转向 CUDA 生态与开源战略（NVLM、OpenROAD）能否延续。
- **判别式决策模型生态加速成型**：Google Gemini 4 Hydrogen 发布即全面接管 Search/AI Mode/Gemini App/API（速度较 3.8 Flash 快 2.5 倍、智能指数 54 接近 Sonnet 5.5 且成本约其 32%、Agent Arena 第 5），Perplexity 开源 Decider v1.3/Decider Flash v1.3，Pipecat 1.16 已接入 Google Decision 1 识别语音语气——"快思考"模型正从 API 走向开放权重与语音实时链路；学术侧 SO-Arena 对 21 个决策模型的首次系统化评测揭示能力与可靠性排名解耦（能力第 7 的 JevAny-27B 可靠性最高 0.84）。
- **递归自我改进（RSI）从概念走向可测量、可验证**：Singularity-Bench 首次让 11 个前沿代理在六层 AI 全栈任务上与人类生产级基线同硬件对照（Opus 5.5+Claude Code 五胜一平 SI=1.252，用一半算力训出更低 PPL）；ACE-RL 为"经验交接"引入下游执行可验证奖励，四基准全胜且 MBPP 近满分 0.987；S1-Evo 则揭示 S1 决策模型新攻击面（证据竞争注入 ASR 71.99%、准确率 92%→20%）。
- **编码 Agent 效率研究精细化**：Termination Lag 实证发现"解决早、停止晚"——成功运行中位 35-60% 步数发生在首个正确 patch 之后（过度验证占 48-63%）；CoReMem 用前瞻估值把跨任务记忆压缩到 6% 字节保住全部步数节省（resolve/token +38%）；SATISFIX 用"顾问角色重框"引出规范满足化心理并纠正（DeepSWE +5.97）；Folio 折叠式上下文管理零模型调用 613ms 压缩 7.3×（66/75 任务、成本降 58%）。

**今日企业+高校研究合作趋势**：32 篇深读中 12 篇为企业+高校合作。三种模式并立：①**中国企业主导的应用闭环型**（浙大×百度连续两篇——决策模型教师化与 Agent-R2 真源数据合成，高校出方法学、企业出模型与工程资源；北邮×百度 Folio 上下文管理）；②**美国联盟型**（CMU 牵头 Singularity-Bench 集结 NVIDIA/UT Austin/HKUST/Amazon AGI/Together AI 等 12 机构，NVIDIA 出 GB300 算力）；③**欧洲产业研究型**（Orange×Aix Marseille 搜索代理压力测试、Chalmers×Edinburgh 神经符号 bug 查找、PolyU×浙大×华为移动 Agent 红队）。蚂蚁 Ling 团队（批 makespan 优化）与京东（JoyFrontBench）为纯企业出品，标志系统工程与评测基建正从高校主导向产业一线迁移。

---

## 二、详细内容追踪

### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

> 今日共深读 32 篇（Hugging Face 11-13 榜 29 篇中 8 篇 Agent/Code/LLM 相关 + arXiv Fri 13 Nov 批次 1286 篇中精选 24 篇），其中 24 篇触发独立精读（见今日精读系列文章），8 篇速览附后。

#### ▍重点深读论文（24 篇，均触发独立精读）

**1. Singularity-Bench: Can Agents Build Agents?**
- **核心亮点**：
  - **任务定义**：评测前沿 AI 代理能否端到端自动完成 AI 系统开发全栈六层任务（预训练/中训练/后训练 RL/推理引擎/代理 harness/集群编排），即"代理能否造代理"的 RSI 前沿量化；属自主 AI 工程基准领域。
  - **方法核心**：SINGULARITY-Bench + Breakwater 防作弊框架——六个开放式端到端任务（最小起始代码库+固定 GPU 预算+单一目标+容器断网），与同硬件成熟人类生产级基线（Megatron-LM/PRISM/Miles/SGLang/Claude Code/Dynamo）对照，按相对人类比值平均得 Singularity-Index；红队覆盖 OWASP Agentic Top-10 九类攻击证明 plain Harbor 与 LLM-judge 均可被攻破，Breakwater 五重防护（二进制指纹校验/API 路由器/进程墙/外部不可写日志/隔离容器）拦截全部攻击。
  - **评估指标**：11 个代理中 Opus 5.5+Claude Code 六任务五胜一平 SI=1.252（PPL 12.77 vs 人类 14.22；中训练 Acc 0.319 vs 0.175；推理收入 11.52 vs 8.18 USD/GPU-hr）；其余 10 代理平均仅 0.58；闭源平均 1.01 vs 开源 0.50；成本-Pareto 前沿：DeepSeek V4.1 Flash+Codex（SI 0.698，$4.36）到 Opus 5.5+CC（$425）。
  - **为何优于 baseline**：开放式任务迫使代理做系统级工程权衡而非超参调优（Opus 用一半算力 1.2×10²⁰ FLOPs + Muon 优化器 + 时间衰减 LR 训出更低 PPL；中训练 97% token 投 CoT 按 RL 评分格式 SFT 增益 0.224 是 PRISM 的 2.8 倍）；同模型换 harness 差异巨大（GLM 5.3：CC 0.657 vs Codex 0.228）→ harness 已成分数第一变量。
- **团队背景**：CMU 牵头 + NVIDIA/UT Austin/HKUST/Amazon AGI/RadixArk/Chicago/Together AI 等 12 机构联盟，NVIDIA 提供 GB300 算力。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16591)；[🌐 项目主页](https://infini-ai-lab.github.io/singularity-bench/)

**2. Experience-Handoff Reinforcement Learning for Recursive Self-Improvement（ACE-RL）**
- **核心亮点**：
  - **任务定义**：训练同一模型既会行动又会"书写未来尝试将继承的经验"（固定 ACE harness 中 actor/reflector/curator 三角色通过任务局部 playbook 递归自我改进）；属 LLM agent RL/推理时自我改进领域。
  - **方法核心**：ACE-RL——把交接工件的可验证奖励定义为"交给全新 actor rollout 执行后的验证器得分"：r_cur(u)=V̂(Apply(P,u))；每角色同输入对比组（M=2 反思互比、C=3 编辑互比）GRPO 式组内归一化，三角色一次联合更新。
  - **评估指标**：RSI@3 四基准全第一：Mastermind 0.775 / MBPP **0.987** / AppWorld 0.667 / MiniSWEBench 0.340（次优 GRPO-harness 0.739/0.820/0.597/0.260；PPO 与 Delayed-joint PPO 多处崩溃至 0）；K=8 外推（训练只见 K=3）差距反扩大；playbook 本身贡献 AppWorld +0.105±0.041。
  - **为何优于 baseline**：同输入分组消除"种子轨迹质量"方差（失败种子也能产出正向奖励的反思）；下游执行可验证奖励避免学习型 critic 的信用误配（Delayed-joint PPO 即因此坍缩）→ 反思获得真实改进信号而非链式归因噪声。
- **团队背景**：NYU 主导 + KAIST，纯高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16409)

**3. SO-Arena: Benchmarking the Capability Spectrum and Reliability of System One Models**
- **核心亮点**：
  - **任务定义**：对"决策模型"（System One 快思考）新品类做能力+可靠性的系统化评测基准；属决策模型评测领域。
  - **方法核心**：SO-Arena——从 40 个数据集构建 2,919 任务（11 领域、5 能力族、四级难度分层），定义能力分 C 与可靠性 R=√(S·N)（S=选项重排序稳定性、N=缺失正确项识别力 2AUC−1），加沙盒交互式 agent 任务。
  - **评估指标**：21 模型：GPT-6 Sol 82.3 / Luna 79.7 为上界；最强 System One GLiDE-27B 与 KnowLine-31B 均 70.2（落后约 10 分但快数十倍，Laya 0.4B 仅 6.0ms）；可靠性最高 JevAny-27B 0.84（能力仅第 11）；错答题选项翻转率 25.7% vs 正答 4.3%；思考使 StartLux Auto 数学 50.8→92.5。
  - **为何优于 baseline**：既有决策基准（JevBench 等）缺难度分层与可靠性维度；受控选项集合操纵直接测决策一致性而非从正确率间接推断 → 揭示能力与可靠性排名解耦（性能最强≠最可靠）。
- **团队背景**：新加坡国立大学，纯高校。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.15777)；[💻 代码仓库](https://github.com/holdrain/SO-Arena)

**4. Actor, Judge, or Teacher? Understanding Decision Models in Long-Horizon Agents**
- **核心亮点**：
  - **任务定义**：系统研究决策模型在长程 agent 中的 Actor（选动作）/Judge（打分）/Teacher（RL 步级监督）三角色，回答"行动差的决策模型能否教得好"；属 agent RL 信用分配领域。
  - **方法核心**：Hindsight+Progress 步级监督——固定决策模型 JEV，对每条 rollout 每个动作问"该动作是否有助于达成目标"（允许看到完整轨迹），打分组内中心化后以 λ=0.5 加到 GRPO 结果优势 A=A_out+λ(s−s̄)；教师训练中冻结、测试时不需要。
  - **评估指标**：Teacher 效果：Qwen2.5-1.5B GRPO 72.8%→82.9%（ALFWorld +10.1）、56.8%→71.4%（WebShop +14.6）；1.5B 教师策略超 7B GRPO（82.9% vs 77.6%）远超 JEV 自身 Actor 的 42.9%；Progress 分差 0.36 vs Value 0.05；教师成本每次 RL 训练仅 $22-30。
  - **为何优于 baseline**：事后全轨迹评判揭示动作后果，绕开 Actor 在线决策须"预判"而易被简单子目标吸引的缺陷（目标遮蔽实验：移除易达成子目标后必需步骤概率 14.6%→77.6%）；Progress 使同轨迹不同动作获得差异化信号而 Value 把结果奖励复制到每一步。
- **团队背景**：浙江大学 + Baidu（企业+高校合作，浙大主导、百度参与工程资源）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.17167)；[💻 代码仓库](https://github.com/ZJU-REAL/decision-agents)

**5. Agent-R^2: Scaling Agents for Realistic Productivity Work via Real-Source Data Synthesis**
- **核心亮点**：
  - **任务定义**：为"现实生产力工作"（长程专业工作流、异构真实文件、交付物级要求）规模化合成后训练数据并训练 agent；属 agent 数据合成与后训练领域。
  - **方法核心**：真源数据合成三阶段——O*NET 职业种子从 DLR-Web 语料抽取工作流蓝图 → 多轮搜索-验证检索真实公共文件 → 沙盒内合成指令+细粒度 rubric（平均 15.3 条/任务，41.9% 数字在指令、39.0% 仅在附件、19.1% 须计算），Claude-Opus-4.5 专家求解；SFT+GRPO（rubric 奖励 MLLM-as-judge）。
  - **评估指标**：GDPval Pass@1：Agent-R2-4B **51.0%**（基座 Qwen3-4B 32.4%，+18.6），超 Qwen3-235B（48.0）、DeepSeek-v3.1（48.8）、GPT-4.1（39.4）；OfficeQA OOD 2.2%→14.8%；重检查交付物 0.5%→95.8%、占位符 12.5%→5.7%；4k 全 SFT 对照 45.5 vs 3k-SFT+1k-RL 51.0（证明增益来自 RL）。
  - **为何优于 baseline**：真实文件保留布局/跨文件依赖迫使 agent 持续探索（专家轨迹在真实文件用 2.54× 工具调用）；rubric 奖励把"产出文件"优化为"满足证据/计算要求"，直接惩罚捷径行为 → 4B 以 +3.0 超 235B 同族旗舰。
- **团队背景**：浙江大学 + Baidu 联合（多名作者双单位），企业+高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.08219)；[💻 代码仓库](https://github.com/ZJU-REAL/Agent-R2)

**6. CoReMem: Cost-Internalizing Memory for Successive Coding Tasks with Prospective Valuation**
- **核心亮点**：
  - **任务定义**：在演化中代码仓库上连续到来的编码任务里决定跨任务携带什么记忆并训练 actor 利用记忆降本；属编码代理跨任务记忆领域。
  - **方法核心**：前瞻性片段估值——把记忆价值定义为对"未知下一任务"结果的改变，逐片移除测中位步数/token 变化，发现价值集中于"锚点"（仓库位置 WHERE+可执行检查 VERIFY），以 **6% 字节**保留全部步数节省；on-policy 成组估值（G=8 混合无/自有/错置记忆 session）+溯源门控价值信用（仅记到使用锚点的 3.46% 步骤）。
  - **评估指标**：ChainSWE：27B resolve 21.0%→25.7%（+22%）、步数 −17%、resolve/token +38%（三模型平均 +41%）；冻结 actor 仅记忆层：token 持平 0% vs Mem0 +113%/MemOp +57%/ReasoningBank +110%；错置记忆鲁棒（+0.3 步）；迁移 SWE-bench Verified 69.2→73.7%。
  - **为何优于 baseline**：既有记忆方法按"来源任务回放增益"优化且记忆模块自身开销 13-64K token；CoReMem 按实测节省估值、锚点模板使 writer 仅花 1.9-5K token；奖励只在成功内部对步数收费杜绝早退；门控保证"何时用记忆"的定向梯度（数学上整轨迹付给则期望抵消为零）。
- **团队背景**：Yale 主导 + 清华 + Goodvision AI，高校为主+企业参与。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.17316)

**7. Folio: Recoverable Multi-Resolution Context Folding for Long-Horizon Language Agents**
- **核心亮点**：
  - **任务定义**：长程 agent 历史超出上下文窗口后的压缩与回收——可恢复、多分辨率、程序化上下文折叠；属长程 agent 上下文管理领域。
  - **方法核心**：FOLIO 上下文管理器——运行时三级视图（L1 模型 turn note / L2 运行时事件清单 / L3 完整源记录），句柄跨折叠恒定；折叠=对已有视图的确定性选择（0 模型调用），代理用 scan/ctx/dossier 三操作取回证据；折叠触发器显式定价"落点" S=B+L+D，累计超额才折叠。
  - **评估指标**：LOCA-bench 75 任务：FOLIO(1M) **66/75** vs Piki-Classic 54 / Claude Code 59 / Codex 57；128K 窗口仍解 60 题；折叠中位 613ms（Piki 模型压缩 101s，165 倍）；压缩 7.3×；缓存命中 92%；成本为 Piki 的 42%（−58%）。
  - **为何优于 baseline**：既有压缩在压缩时才由模型生成降表示（每次重读重写）；FOLIO 表示在执行中已存在（L2/L3 运行时免费写），折叠是程序化视图选择且单调降级保缓存前缀字节不变；句柄共享使恢复精确落在被折叠对象上（793KB 公告经范围句柄用 ctx 提取 6.4K token 答案）。
- **团队背景**：北京邮电大学 + 百度（企业+高校合作，百度贡献 Piki runtime 与工程）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16627)

**8. Termination Lag: Coding Agents Solve Early but Stop Late**
- **核心亮点**：
  - **任务定义**：实证测量编码 agent 的"终止滞后"——首个正确 patch 步（TTFCP）到自行结束的步数差，揭示最终分数混淆"产出正确修复"与"认识到已修复"两种能力；属编码 agent 实证分析领域。
  - **方法核心**：逐步 patch 记录 + 离线评估——logging proxy 在每个 agent step 后用临时 Git index 记录 diff（不改动工作区），运行后隔离容器离线评估每个去重 patch，agent 全程无反馈；5 模型 × 2 框架 × 3 基准（SWE-bench Verified/Multilingual/Pro）。
  - **评估指标**：首编辑即正确率 38-82%（Qwen3-Coder Verified 82%）；中位 TL：GPT-6 Luna 仅 6 步（NTL 36%）vs Laguna-XS.2 57 步（NTL 60%）；TL 活动构成：verify 48-63%、explore 15-29%、再编辑仅 2-6%；1-8% 运行破坏已正确 patch 且未恢复；oracle 早停可省成功运行 35-60% 步数。
  - **为何优于 baseline**：既有 early-termination 工作从轨迹预测成功/失败，本文直接测量 patch 何时已正确——发现滞后主要源于停止决策不确定性而非修复能力不足（过度验证），停止行为应成为独立评估维度。
- **团队背景**：宾州州立大学，纯高校（开源模型经 Argonne Aurora 提供）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.14960)

**9. Eliciting and Mitigating Specification Satisficing in Coding Agents（SATISFIX）**
- **核心亮点**：
  - **任务定义**：识别并缓解编码 agent 的"规范满足化"失败——agent 曾考虑过满足需求的实现 a⁺ 却因自认合理的理由 ρ 提交不满足 r 的 a°，且 self-correction 因 ρ 是自己的前提而无法发现；属 LLM self-correction 领域。
  - **方法核心**：SATISFIX 两步插入——①目标对齐引出：消息伪称来自"任务维护者"，把 agent 置于顾问角色报告"哪些地方需求可按不同方式处理"（无需承认错误，绕过 LLM 对自身输出的偏好偏差）；②重新考虑：恢复开发者角色，对每个报告选择对照任务描述 D（不依赖 ρ）复查后决定。
  - **评估指标**：DeepSWE+GPT-5.6-Luna：pass@1 44.25%→**50.22%**（+5.97）；SWE-bench Pro hard 子集平均 +7.06%（Gemma 4 26B 最大 +11.77）；超最佳 self-correction 基线（Self-Refine 46.24、DeCRIM 44.25 零提升）3.98 分；成本 $0.897/任务（Self-Refine $2.022 的一半）。
  - **为何优于 baseline**：self-correction 失败根因=agent 已凭 ρ 接受 a°（DeCRIM 零提升为证）；顾问角色重框使 ρ 被显式引出（消融：去顾问角色跌回 base −6.2）；重新考虑强制对照 D 而非 ρ 复查 → 恢复 a⁺。
- **团队背景**：Purdue University，纯高校。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16289)

**10. MULTILOOP: Evolving Multi-Agent Harnesses for Complex Information-Seeking Tasks**
- **核心亮点**：
  - **任务定义**：研究自动 harness 优化（编码代理迭代分析 rollout、诊断失败、改 harness 代码）能否修复多代理系统失败；属多代理系统自动优化领域（NeurIPS 2026）。
  - **方法核心**：EVALUATE（当前 harness 10 训练样本 rollout 转区分 orchestrator/subagent 的 markdown，记分入历史支持回滚）+ OPTIMIZE（Antigravity SDK 编码代理 Gemini-3.1-Pro 读全部历史归并复发失败模式做定向编辑），迭代 10 轮。
  - **评估指标**：DeepSearchQA 890 题：未优化 Orchestrator 67.27→**71.88**（+4.61，全场最佳）；单代理 SPOT+优化反降 65.54（−3.15）、prompt-only 64.62；优化后系统做更多工作（subagent 14.0 vs 10.9、每题 250 subagent 步）；Final Answer Composition 失败 10→3、Retrieval Dependency 10→4。
  - **为何优于 baseline**：优化器能读取区分角色的 rollout 与历史分数，改动集中在答案格式约束/查询分解/并行委派 + 新增 download_file 工具（程序化下载数据集用 pandas 分析替代搜索片段猜数）；但 Bad Orchestration 10→9 基本未改善——多代理特征性失败持续存在。
- **团队背景**：Tel Aviv University + Google Research（企业+高校合作，一作为 Google 实习生）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.15754)

**11. The Value of Thinking: Learning When to Reason in Hybrid Decision Models（HDM）**
- **核心亮点**：
  - **任务定义**：让单个 LLM 决定"是否需要推理"（zero-or-T thinking：要么零 token 前向直接出答案，要么 T token 完整推理）；属高效推理/System1-System2 切换领域。
  - **方法核心**：HDM+CDPO——冻结 backbone 同时承担快模式（隐状态+候选码 logits 零 token）与慢模式（自回归推理）；唯一可训练部件是几百 KB 的 self-routing block，从快模式已算出的隐状态预测两模式联合结果四类分布，输出"思考价值" vi，vi>λ 才升级；路由一次完成不增额外前向。
  - **评估指标**：JevBench：HDM-4B 0.857 vs Jev 1.13.0 0.851（+0.7%）、中位延迟 −72.5%（0.179s）；AGIEval token −47%、GPQA −43%；BANKING77 0.920 仅 1.1 token/题（对照 Tacit-9B 需 140.2）；达 90% always-thinking 精度所需 token：HDM 103 vs RouteLLM 112/置信度 138/ThinkSwitcher 352。
  - **为何优于 baseline**：路由信号是"思考的增益"（uplift）而非置信度——置信度无法区分"快对慢也对"与"只有快对"；联合预测两模式对错精准只升级"慢模式能救回"的题；快模式复用同一 prefill 升级不付第二次编码成本。
- **团队背景**：Huawei Noah's Ark Lab + Liverpool + UCL（企业+高校合作，华为主导）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16961)；[💻 代码仓库](https://github.com/zhengxuyu/HDM)

**12. From Large Language Models to Efficient System One Models via Adaptive Computation（NESTONE）**
- **核心亮点**：
  - **任务定义**：把 LLM 高效转化为可在多个计算深度运行的 System One 决策模型（自适应早退），回答"一个决策需要多少层深度"；属决策模型效率领域。
  - **方法核心**：NESTONE——IG 归因发现决策写在中间层 15-23（贡献 73% 决策边际）、层 24-29 可删；前缀子模型 S12/S16/S24 与全模型联合训练共享"尾码"，自蒸馏（α 从 0 升 0.5）无需外部信号；推理时每出口用隐藏态逻辑回归门判断停止（门在尾码计算前读状态，无需重复计算）。
  - **评估指标**：7 决策基准宏平均：12 层 64.4% vs 独立训练 61.5%（+2.9 显著）、16 层 +1.8、24/32 层持平；训练一次 80 分钟 < 四个独立模型 94 分钟；早退门控 easy 17.5-20.2 层 / hard 23.9-24.0 层，四大基准（96% 决策）精度仅 +0.08；门 AUROC 0.86-0.89；12 层出口延迟 18.4ms。
  - **为何优于 baseline**：独立训练的小模型只是全模型降级拷贝（M16 91% 正确决策是全模型子集）；自蒸馏让浅出口直接模仿全模型分布保住更多已会能力（召回 0.688 vs 0.657）；门读隐藏态省去置信类规则每出口多算 2 层 coda → easy 省层 hard 不加层。
- **团队背景**：南洋理工大学单人作者，纯高校。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16321)

**13. daVinci-Prover: Agent-Native Mid-Training for Formal Theorem Proving**
- **核心亮点**：
  - **任务定义**：在 Lean 4 可执行验证环境中让 LLM agent 通过"编辑-检查-修订"循环交互式构造形式化证明，研究 agent 原生中期训练配方；属形式定理证明领域。
  - **方法核心**：33.7B token 中期训练语料 = 数学技能先验（分解组装 8.43B/子目标搜索 3.45B/错误诊断修订 3.84B/非形式推理自验证 5.70B）+ 环境原生 agent 轨迹 7.17B（真实工具调用/Lean 反馈），再以自研 7 工具 harness 采集 36K 成功轨迹 SFT。
  - **评估指标**：DeepSeek-V4-Flash+harness pass@8：MathOlympiad 52.2 / MiniF2F **95.9** / ProofNet 56.5 / ProverBench 68.9 / Putnam 39.7 全部已报告最高（超 sketch-proof w/TIR 93.9 的 pass@32）；Qwen3-8B mid-training 平均 +13.7 点；33B token 胜 OProver 65B（Fate-M 85.3 vs 73.1）；记忆压缩使 FATE-H 16.7→42.0（35B）。
  - **为何优于 baseline**：既有语料以完整定理-证明对为主；把四类技能先验+与推理时同分布的 agent 交互轨迹前移到 mid-training 使 SFT 起点更低、对齐更快（Dagent 仅 8.4B token 却因分布对齐超 25.3B 的 Dskill）；mid-train 后高难问题首证分解步数 5.13→9.57。
- **团队背景**：上海创新研究院 + 上海交大 + 复旦 + GAIR，研究机构+高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16433)；[💻 代码仓库](https://github.com/GAIR-NLP/daVinci-prover)

**14. DeepStress: Stress-Testing Deep Search Agents**
- **核心亮点**：
  - **任务定义**：对深度搜索代理做可控压力测试——按指定概率注入低可信/不相关/不事实的合成检索文档，度量劣化证据下的行为与可靠性；属搜索代理鲁棒性评估领域。
  - **方法核心**：动态合成检索环境 + RAS/CoPRAS 指标——拦截搜索调用，Gemma-4-31B 按概率 π(T/R/F) 独立采样三轴标签动态生成合成文档；RAS 给不可靠证据下的谨慎弃权部分信用（正确=1、弃权或超预算=1−ργ、错误=0），CoPRAS=成本/RAS。
  - **评估指标**：12 系统 × 2 数据集 × 7 场景 = 33,600 轨迹、809,205 份合成文档（约 104M token）；0% 质量下平均正确率降至 0.30 而 RAS 保持 0.61（分离"稳健不答"与"过度自信失败"：gpt-5-nano(medium) RAS 0.84 vs 正确 0.41；Search-R1 两者同降 0.80→0.35）；信任度 0% 时 Search-R1 正确率不变而 DR-Tulu 59.8%→40.9%。
  - **为何优于 baseline**：受控语义扰动替代事后观察，三轴独立可控且文档随实际查询动态生成；把证据可靠性显式引入评分 → 揭示窄目标 RL 训练的模型脆弱依赖上下文证据、GPT 类靠规模多样性更稳健。
- **团队背景**：Orange Research（法国电信）+ Aix Marseille Univ/CNRS（企业+高校合作）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.13920)

**15. Privacy Leakage from Recurrent State in LLM Inference**
- **核心亮点**：
  - **任务定义**：研究线性注意力/SSM 混合架构推理时固定大小循环状态的隐私泄露——受限观察者从部分状态观察、持久性、因果影响与功耗侧信道恢复私有事实；属 LLM 推理安全领域。
  - **方法核心**：三阶段攻击方法论（profiling→acquisition→inference）+ 四条泄露路径——部分状态 Hamming weight 探测、状态移植归因因果贡献（384/384 对正向 +1.811 logits）、CW305 FPGA 功耗轨迹直接恢复、A100 NVML 每 20ms 功率读数经 change-point/OLS 映射。
  - **评估指标**：覆盖 0.149% 状态即恢复 condition 68.8-82.8%、response rule 100%；SynthPAI 作者级年龄 MAE 6.56 年 vs 基线 11.30（47.7% 作者误差<5 年）；A100 被动功耗端点 MAE 7.778 vs 11.333 年（p=4×10⁻⁵）；24 公共轮次后泄漏仍显著（42.2% vs 对照 21.1%）。
  - **为何优于 baseline**：循环状态条目混合多 token 贡献、无 per-token 条目可逐出 → 仅保护 prompt/KV cache 不覆盖该表示；匹配对照剥离公共轨迹解释后剩余信号归因私有历史；fixed-coefficient 臂逐出调度混淆 → 泄漏归因数据依赖开关活动。
- **团队背景**：Lehigh + Georgia Tech，纯高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16048)

**16. MobileProbe: Adaptive Security Evaluation of Mobile Agents via Trajectory-Aware Injection**
- **核心亮点**：
  - **任务定义**：对移动 GUI Agent 做自适应安全评测——针对静态基准"固定步骤钉死 payload"的实例特异性不足（Agent 走别的路径→假安全）；属移动 Agent 环境注入攻击评测领域（NDSS 2027）。
  - **方法核心**：双层架构——探测平台对可注入 App 自动化 pentesting 产出模板，gate-on-match 仅在 Agent 到达匹配页注入；红队系统沿轨迹按 Rel/Crit/Expl 三维定位最优注入点，策略×风格空间采样+对抗经验记忆生成种子，LLM Judge 四级序数反馈进化精炼（Tmax=15）。
  - **评估指标**：83 实例基准（12 App、CIA 三目标）：Payload Miss Rate 从 Expert-Fixed 的 13.3-22.9% 降至 4.8-6.0%；ASR 85.1-86.3%（跨 Agent 变差仅 1.2%）；超最强基线 Adapted PAIR 28.5-56.6 个百分点；SHR（攻击成功且原任务仍完成）最高 68.1%；去进化精炼 ASR 85.1%→28.5%。
  - **为何优于 baseline**：注入点沿实际轨迹自适应定位保证可比较暴露；语义+视觉约束+代理预过滤省预算；四级序数反馈区分"被忽略/被拒绝/部分服从" → 轻量端侧模型（GUI-Owl）在所有基线下 ASR 均最高，固定点基准低估端侧小模型风险。
- **团队背景**：香港理工大学 + 浙大 + 华为（企业+高校合作，PolyU 牵头）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16693)

**17. S1-Evo: Experience-Guided Evidence Competition Attacks on Typed System-1 Decision Models**
- **核心亮点**：
  - **任务定义**：对 LLM Agent 中承担路由/审核/授权的 typed System-1 决策模型发起免迭代可泛化对抗攻击；属 AI 安全红队领域。
  - **方法核心**：契约感知证据通道——11 个算子族在不可信字段注入"补充证据"与原任务证据竞争（保持事实不变与原文逐字保留，而非命令覆盖）；离线利用 S1 接口的方向性 margin 反馈四阶段搜索；结构索引经验库测试时一次检索单遍构造扰动。
  - **评估指标**：895 实例平均 ASR **71.99%**、高置信 57.47%，平均准确率 92.02%→20.28%；分基准超最强基线 16.4-43.0 个百分点；token 成本比 FlipJev/PAIR-jev/JevOut 省 32-62%；E2E 双系统：Tool Router 准确率 94%→69.33%、E2E 违规率 24%；生成式 Qwen-3.8-flash 仅 4.00%（对 S1 特异）。
  - **为何优于 baseline**：S1 按选项竞争性打分 → 命令式注入/通用后缀与"文本续接劫持"攻击面错配；改为注入契约对齐的竞争性证据削弱 y*/锚定目标 t；经验库贡献 +25pp（离线蒸馏结构先验）→ 免迭代泛化。
- **团队背景**：原文未标注机构信息。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16937)

**18. When Safety Becomes an Attack Surface: Manipulating Abstention in Tool-Using AI Agents**
- **核心亮点**：
  - **任务定义**：研究 warning-induced over-abstention 可用性攻击——攻击者仅在低信任通道植入伪造风险警告，使合规 agent 拒绝执行合法任务（有害不作为）；属间接提示注入镜像危害领域。
  - **方法核心**：反事实任务变体（AgentAbstain S7/S8 配对任务仅编辑 initial_states 生成 5 类变体）+ Verify-Before-Abstain 防御——_provenance 信任层信封（四级）+ 验证后弃权规则：仅当低信任警告被权威记录或内容佐证时才弃权。
  - **评估指标**：caution-first 全面放大攻击：gemini 弃权率 9.8%→86.3%（+76.5pp）、deepseek 21.6%→64.7%；防御后 false-warning 完成率 deepseek 27%→100%、gemini 13%→100%；权威记录被攻陷时防御失效（100%/93%/93%）——界定信任锚点边界；出版环境 caution-first 使 deepseek 25%→75%。
  - **为何优于 baseline**：caution-first 失败机制=提高风险厌恶使 agent 把未认证警告当权威；provenance-aware 把"警告权威性"与"风险真实性"两个判断解耦，要求交叉核证 → 恢复可用性但不保证保密性（敏感泄露 3/7→2/7）。
- **团队背景**：单人作者，原文未标注机构。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.14864)

**19. SwarmSafe-Delta: Isolating Message Effects on LLM Agent Safety**
- **核心亮点**：
  - **任务定义**：在工具型 LLM 多智能体团队中因果隔离"上游消息投递"对下游执行器政策违规的影响；属多智能体安全评估领域。
  - **方法核心**：配对可执行协议——冻结前缀三臂投递（真实/隐藏/格式匹配安慰剂）从同一前缀分叉；配对良性/风险双政策世界（仅差一个权威政策事实）重放同一动作脚本；确定性谓词验证（不用 LLM judge）。
  - **评估指标**：200 任务对（N=4 主实验 960 匹配块、5,760 world 级观测）：真实投递违规率 21.4% vs 隐藏 1.0%（配对差 +20.3pp）；安慰剂 5.0%（格式通道贡献 +16.4pp）；压制通信牺牲效用（良性世界成功率 96.5% vs 55.9%）；风险集中于直接执行类建议（219 条直接建议 218 违规 vs 713 条前置检查建议 0 违规）；vs 顺序单智能体 Single-Budget（93.2%/0.7%）违规差 +20.7pp。
  - **为何优于 baseline**：不是"优于 baseline 的方法"而是比既有单/多智能体对比更强的归因工具——把消息内容效应从团队组织效应中剥离；双世界解耦任务成功与政策合规；定位风险来源为直接执行建议（218/219）。
- **团队背景**：独立研究者单人作品，无机构挂靠。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16123)

**20. Where LLM Agents Get Stuck Debugging Databases: Access More Than Reasoning**
- **核心亮点**：
  - **任务定义**：通过实地参与式观察回答"限制 LLM coding agent 诊断生产数据库事故的是推理能力还是访问权限"；属软件工程实证/DevOps 智能体领域。
  - **方法核心**：某大型科技公司生产消息派发引擎 10 天窗口 8 起数据库事故的回溯性三角验证（agent 会话 JSONL 转录+git 历史+部署/负载记录互证）；提出 reasoning × access 矩阵分类法（false start=与已有证据一致的错误假设，区别于捏造）。
  - **评估指标**：5 起依赖运行时并发的的事故全部以 false start 开局且访问缺口全部出现（1 起单独构成限制、4 起与推理错误混合）；无任何一起仅因推理受限；27 会话/410 小时/3,000 工具调用；吞吐事故连续反驳 6 个假设最终修复是 1 行配置；生产只读 SQL 需人工代理执行（28 条查询 25 条集中在竞态事故）。
  - **为何优于 baseline**：既有 AIOps 基准把"采集证据的成本"设为零 → 低分被误读为推理天花板；生产中并发故障证据可能根本未被记录（应用日志截断死锁 DETAIL 而 agent 无权访问服务器端完整报告）；结论：投资"forensics 层"（安全只读询问 API/锁黑匣子/生产重放 harness）比更大模型更有效。
- **团队背景**：巴西帕拉联邦大学单作者（参与式观察），纯学术。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.17304)

**21. Bug Finding with Agentic Symbolic Execution**
- **核心亮点**：
  - **任务定义**：让符号执行（CrossHair）在无人工契约、无人工环境模型下全自动应用于数据库支撑的 Python Web 应用找 bug；属神经符号程序分析领域。
  - **方法核心**：crosshair-bugs 管线，原则"LLM 提议、求解器检查、具体执行确认"——Agent 生成 schema 约束的 ORM 桩（返回受约束符号值）+有界惰性初始化插件、为每函数写前置/后置条件、CrossHair 无模型调用运行、审计 Agent 归因反例、写单元+E2E 见证测试确认。
  - **评估指标**：1,156,044 行 Python、11 个开源应用：可达性 4.5%→**81%**（裸 CrossHair→完整合成环境，单库最大 +69pp）；1,050 个见证复现 bug（88% E2E 复现；严重度 1 critical/39 high/321 medium）；CrossHair 可匹配 bug 中 52% 仅在合成环境下可执行才被发现；维护者已独立修复抽查 45 个高优 bug 中 22 个。
  - **为何优于 baseline**：同模型纯 LLM 检查基线 ai-bugs 总产出更多但漏掉本管线 312 个 bug（并用比基线单独多找 17%）；两者类型互补：基线 57% 错误输出 vs 本方法 56% 崩溃（SMT 探索验证放行但测试少的输入边角，如全角数字'²'骗过 isdigit）。
- **团队背景**：Chalmers + Edinburgh（Andrew D. Gordon），纯高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.15596)；[💻 工件仓库](https://doi.org/10.6084/m9.figshare.32835236)

**22. Robo-SWE: An Executable Issue-Resolution Dataset Mined from Robot Software Repositories**
- **核心亮点**：
  - **任务定义**：从公共机器人软件仓库挖掘构建可执行的 issue-resolution 基准，填补编码 agent 评估无一针对机器人领域（ROS 2 构建/仿真引擎/硬件依赖难测试）的空白；属软件工程基准领域。
  - **方法核心**：执行式准入流水线——61,878 候选仓库→25 精选→1,567 候选 PR→LLM 筛选（不看 patch）→digest-pinned 容器化构建→三状态执行准入（base 测试/base+PR 测试 F2F 失败/开发者修复 F2P 全过且 P2P 不破，重复 2-3 次 flaky 即拒）→236 任务 + 全量 provenance ledger。
  - **评估指标**：236 任务/28 仓库/4 领域（ROS 2 103、仿真与物理 63）；Resolve rate：Claude Opus 5.5 **56.5%** > GPT-6.1 Sol max 54.1% > GLM-5.3 46.2% > Kimi K3.1 Pro 43.9%；15 次主运行中 83 任务总被解决、81 从未、72 随 setup/run 变化；每 attempt 成本 Opus $0.47。
  - **为何优于 baseline**：开发者测试在修复前失败、修复后通过且重复稳定 → oracle 可靠、任务可离线复现；ROBUST 仅 221 个 ROS bug、RoboBugBench 仅 33 个修复且单一导航栈；首个跨 4 领域结合挖掘 issue+隐藏测试+可执行环境的数据集。
- **团队背景**：Bilkent + Evalium（亚美尼亚公司，出算力与费用）+ TUM，企业+高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16116)；[💻 数据集](https://huggingface.co/datasets/vharatian/Robo-SWE)

**23. JoyFrontBench: Evaluating Front-End Coding Agents through Goal-Oriented Acceptance Contracts**
- **核心亮点**：
  - **任务定义**：构建前端编码 agent 基准，通过"目标导向验收契约"显式区分"部分实现（任务分高）"与"应用成功（核心工作流可用）"；属前端代码生成评测领域。
  - **方法核心**：生成前预标注核心任务+强制准则+共享完成阈值 τ=0.8——核心任务需全部强制准则通过且完成度≥τ，应用成功（ASR）要求所有核心任务在同一次运行的可运行工件上全部通过；GPT-5.5 证据锚定裁判+确定性审计+免模型评分器。
  - **评估指标**：100 题/764 任务/2,596 条原子准则（需求来自 30+ 从业者访谈）：五模型平均 ATS 87.69% vs ASR 仅 **51.52%**（36 点差距）；Qwen3.8 Max ASR 最高 57.62%；48.6% 组合三跑中出现成功/失败混合（Kimi K3 达 65.7%）；32.3% 失败运行 ATS≥90%；跨 harness ASR 差异 −9.05 至 +11.90 点。
  - **为何优于 baseline**：聚合任务分允许高分补偿核心工作流失败；强制准则+阈值联合验收不可补偿（失效即 S=0）→ 揭示既有聚合指标系统性高估应用可用性。
- **团队背景**：京东（JD.com）12 位作者，纯企业研究。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.14952)

**24. Time and Space: A Dual-Axis, Evidence-Grounded Context Substrate for Personal Agents**
- **核心亮点**：
  - **任务定义**：为个人代理构建"时空上下文"表示基座——解决代理不知道"某短语指哪个实体、某事实何时成立、哪个来源许可某断言"的根本表示缺陷；属 agent 记忆/时序知识图谱领域。
  - **方法核心**：双轴基座 + 六不变量——时间轴三时间分离（事件时间/有效期/摄入时间）；空间轴为关系空间（实体为点、类型化关系为边）；I3 出处完整性（每个断言引用记录内 span）、I5 歧义保持（mention 映射候选集而非强行消解）；上下文算子 Cw(q)=关系空间 k-跳邻域∩时间区间∩置信下限。
  - **评估指标**：框架论文未运行评估协议；检索层原型实测：341 文档/1.57 亿 token/19 万嵌入，BM25 top-50 中位 0.07ms；已定义未运行的逐不变量指标（引用覆盖率/弃答率/时间准确率等）。
  - **为何优于 baseline**：平面文本记忆保留 token 丢弃关系结构 → 无法回答跨源组合/身份消歧/时间有效期问题；三时间分离使"我付费前知道这次旅行吗"（摄入时间）与"1月27日旅行还成立吗"（有效期）可区分回答；作者明确承认"框架是论文对象而非结果"。
- **团队背景**：正文未标注机构信息。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.16492)

#### ▍速览论文（8 篇）

- **WorldDraft: If Code Builds the World, Can Video Models Just Render It?**（2611.16217，HF 14↑）：评测视频模型作为"生成式渲染器"（代码 4D 世界草稿→照片级视频），12 模型 432 用例——WP 与 RF 尖锐权衡（SolarRenderer WP 最高 63.28 vs Wan 3.0 RF 最高 80.40 但 WP 最低 44.37）；WorldDraft-Primer 首帧 SVG 角标注入使 MiniMax-H3 Overall +1.93 反超 Seedance 2.5。SUSTech+HKUST 等 8 机构。[📄 原文](https://arxiv.org/abs/2611.16217)
- **SkillEarth: Teaching General-Purpose Agents to Act on Earth via Skill Evolution**（2611.15481）：自进化遥感技能提取（组件化表示+观测接地双裁判+GOR 路由图），OpenEarth-Bench 9 项 End-to-End 8 项最佳（Gen. 24.14 vs SkillX 17.24），技能跨模型迁移（Qwen3-4B Per. 13.31→35.00）。清华 AIR+大连理工。[📄 原文](https://arxiv.org/abs/2611.15481)
- **Reducing Batch Makespan for LLM Agent Workloads**（2611.16661）：推理-环境协调优化（混合缓存容量模型+LBCH 路由+CAAS 调度），Ling-3.x 上 agent 评测批 makespan 总降幅 10.4-65.0%（MCP-Atlas 466→128 分钟），cache reuse 保持 98%+。蚂蚁集团 Ling 团队纯企业出品。[📄 原文](https://arxiv.org/abs/2611.16661)
- **Read Once, Answer Many: Parallel Prompting for Shared-Context LLM Inference**（2611.15130）：流 id 注意力掩码使 P 问题共享文档 KV 每步只读一次，SGLang 仅改 80 行——MHA 长文档多问题 3.7-6.0×（16k 文档 256 问 60.0s→12.3s），QuALITY 准确率差 0.24。UCSC。[📄 原文](https://arxiv.org/abs/2611.15130)
- **VulSpot: A Vulnerability Dataset Where No Vulnerability Goes Unlocalized**（2611.15737）：503,332 函数/24,257 CVE 的"活"漏洞数据集——sink candidates 定位 99.3% diff 空标签函数（additive fix 占 32.6%）、克隆感知切分泄漏 57%→0%、月度自动更新。蒙斯大学。[📄 原文](https://arxiv.org/abs/2611.15737)
- **Does a Self-Learning Migration Agent Learn?**（2611.16143）：审计自学习迁移 agent——72% 留出单元参考目标与训练共享，泄漏无关单元记忆效应跌至 +0.002；复制最近训练目标基线 RC 0.222 超所有 LLM 系统；PostgreSQL 接受机制 10-22% 误判。EPAM Systems。[📄 原文](https://arxiv.org/abs/2611.16143)
- **An Empirical Study of Maintainer-Identified AI Slop Pull Requests**（2611.15159）：首个项目级 AI slop PR 定量画像——1,786 个 slop PR/76 仓库（transformers 454 个占 25%），合并率 <1% vs 非 slop 84.73%，关闭中位 6.5 小时；94.1% 加行多于删行。那慕尔大学。[📄 原文](https://arxiv.org/abs/2611.15159)
- **One Hierarchy, Many Sports**（2611.15529）：层级对齐多任务 Transformer 在拍类运动下一拍预测四数据集 Final 全第一（乒乓球 0.5315），层级深度 1→3 级单调 +0.0098，但优势幅度温和（羽毛球打平）。台湾中央大学+清华。[📄 原文](https://arxiv.org/abs/2611.15529)

#### ▍顶会标准评审简表（阶段五）

| 论文 | 新颖性 | 触发精读 | 一句话理由 |
|------|--------|----------|-----------|
| Singularity-Bench | 高 | ✅ 是 | 首个量化"代理造代理"RSI 前沿的六层全栈基准+防作弊框架，Opus 5.5 首次系统级超人类基线 |
| ACE-RL（Experience-Handoff RL） | 高 | ✅ 是 | 交接工件可验证奖励+同输入分组归因，四基准全胜且外推领先，RSI 训练范式级创新 |
| SO-Arena | 高 | ✅ 是 | 首个决策模型能力×可靠性双维基准，揭示排名解耦与错答翻转现象 |
| Actor, Judge, or Teacher | 高 | ✅ 是 | 首次系统解构决策模型三角色，"行动差但教得好"的反直觉发现+低成本步级监督 |
| Agent-R² | 高 | ✅ 是 | 真源数据合成范式（真实文件+rubric 奖励），4B 超 235B 旗舰的效率证据 |
| CoReMem | 高 | ✅ 是 | 前瞻估值+溯源门控记忆信用，6% 字节保全部节省、resolve/token +41% |
| Folio | 高 | ✅ 是 | 程序化可恢复上下文折叠（0 模型调用 613ms），66/75 vs 54 且成本 −58% |
| Termination Lag | 高 | ✅ 是 | 首次测量"解决早停止晚"，oracle 早停省 35-60% 步数，新评估维度 |
| SATISFIX | 高 | ✅ 是 | 规范满足化失败模式的角色重框引出+纠正，DeepSWE +5.97 超全部 self-correction |
| MULTILOOP | 中高 | ✅ 是 | harness 自动优化首次系统迁移到多代理，+4.61 但编排类失败未改善（NeurIPS 2026） |
| HDM（Value of Thinking） | 高 | ✅ 是 | zero-or-T 范式+思考增益（uplift）路由而非置信度，token −47% 精度近保持 |
| NESTONE | 中高 | ✅ 是 | 嵌套前缀+自蒸馏+隐藏态门的多深度决策模型，12 层 +2.9 显著且 easy 省层 hard 不加层 |
| daVinci-Prover | 高 | ✅ 是 | agent 原生 mid-training 配方，5 基准全 SOTA（MiniF2F 95.9），33B token 胜 65B |
| DeepStress | 中高 | ✅ 是 | 三轴可控证据劣化压力测试+RAS 可靠性感知评分，33,600 轨迹规模 |
| Privacy Leakage from Recurrent State | 高 | ✅ 是 | 线性注意力循环状态隐私泄露四路径首次系统刻画，0.15% 状态恢复 100% 规则+功耗侧信道 |
| MobileProbe | 中高 | ✅ 是 | 轨迹自适应注入评测，PMR 22.9%→6.0%、ASR 85%+ 超基线 28-57pp（NDSS 2027） |
| S1-Evo | 中高 | ✅ 是 | S1 决策模型证据竞争攻击新范式，ASR 72% 且成本降 32-62% |
| Abstention Attack Surface | 中高 | ✅ 是 | 警告诱发过度弃权可用性攻击（有害不作为）+ provenance 防御边界 |
| SwarmSafe-Delta | 中 | ✅ 是 | 消息效应因果隔离协议（冻结前缀三臂+双世界），+20.3pp 归因精确 |
| Access More Than Reasoning | 中高 | ✅ 是 | 实地研究挑战"推理天花板"叙事：访问缺口而非推理限制生产诊断 |
| Agentic Symbolic Execution | 高 | ✅ 是 | 合成环境使符号执行可达性 4.5%→81%，52% bug 仅此路径可发现，E2E 见证确认 |
| Robo-SWE | 中高 | ✅ 是 | 首个机器人软件可执行 issue-resolution 基准（236 任务），领域空白 |
| JoyFrontBench | 中高 | ✅ 是 | 验收契约揭示 ATS 87.7% vs ASR 51.5% 的 36 点幻觉差距 |
| Time and Space | 中 | ✅ 是 | 双轴证据接地上下文基座规范（六不变量），框架完整但未运行评估（从宽触发） |

---

### 2. 产业动态与产品创新（AI Hot Skill 精选）

#### ▍NVIDIA 与产业格局

- **事件/产品名称**：**黄仁勋正式辞任 NVIDIA CEO，33 年任期终结**
- **核心内容**：黄仁勋因持续发酵的丑闻风波辞去总裁兼 CEO 职务并退出董事会，CFO Colette Kress 出任临时 CEO；SemiAnalysis 深挖背后出口管制博弈，The Verge 关联芯片走私疑云；民主党启动对华芯片销售调查、众议院中国特设委员会主席 Moolenaar 发表声明、参院银行委员会主席要求 12 月 3 日作证否则传票；Nvidia 内部建议箱帖文获 1600+ 评论多数请愿回归；Hugging Face CEO 承诺继续力挺开源 AI 战略。
- **落地应用场景**：财报（11/17）与产品路线图承诺"不中断"，但市场焦点转向 CUDA 生态护城河是否依赖创始人个人、开源承诺（NVLM/OpenROAD）是否延续；以色列投资人视领导层动荡为挑战者机会。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/googles-gemini-4-hydrogen-outscores-larger-models) ｜ [🌐 SemiAnalysis 深度分析](https://newsletter.semianalysis.com/p/nvidia-and-the-world-w)

#### ▍模型发布

- **事件/产品名称**：**Google 发布 Gemini 4 Hydrogen，全面接管 Search 与 Gemini App**
- **核心内容**：Gemini 4 家族第三成员（与 Carbon、Argon 并列），取代 Gemini 3.8 Flash；Artificial Analysis 智能指数 54（接近 Sonnet 5.5 且成本约其 32%）、任务速度较 3.8 Flash 快 2.5 倍；Agent Arena 第 5 名（前十中最快）、CursorBench 54.3%；四档思考级别；即日起覆盖 Gemini App、Search AI Mode、API、Antigravity。
- **落地应用场景**：日常编程与智能体工作流的性价比主力；Search 复杂问题回答升级；开发者可用更低成本构建高频 agent 链路。
- **相关链接**：[🌐 点击查看新闻来源](https://deepmind.google/blog/introducing-gemini-4-hydrogen/)

- **事件/产品名称**：**Perplexity 开源 Decider v1.3 / Decider Flash v1.3 决策模型**
- **核心内容**：新增 locate 能力，安全模型免费开放至 11 月 19 日；决策模型开源生态再添一员（与 SO-Arena/S1-Evo 论文揭示的 S1 生态爆发相呼应）。
- **落地应用场景**：企业自托管路由/审核/授权等 agent 决策层，替代高成本 API。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2121040336361533502)

- **事件/产品名称**：**Edge0 开源 145B 端侧模型 & Inkling 2 Small 开源权重**
- **核心内容**：Edge0 发布 145B 可在笔记本本地运行的前沿能力端侧模型；Mira Murati 的 Thinking Machines 以 Apache 2.0 开源 Inkling 2 Small，1647 分登顶 Code Arena WebDev 美国开源模型第一，开放权重后 Agent Arena 美国开源第一。
- **落地应用场景**：离线/隐私敏感场景的本地推理；开源编码 agent 的底座选择。
- **相关链接**：[🌐 点击查看新闻来源](https://edge0.ai/blog/introducing-edge0-145b-frontier-ai-capabilities-at-home)

- **事件/产品名称**：**腾讯混元 Hy Realtime 实时语音模型 + Hy4 安全榜登顶**
- **核心内容**：Hy Realtime 新增工具调用与唱歌能力，元宝语音通话升级；Hy4 在 Artificial Analysis Cyber Index 排名第三、CWE-Bench-AA 登顶。
- **落地应用场景**：全双工语音交互与实时 agent 语音链路；代码安全场景。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/1/026/825.htm)

- **事件/产品名称**：**DeepSeek 开放全家桶应用 API**
- **核心内容**：App 核心能力全面 API 化——联网搜索、文件输入、阅读链接、语音独立接口，V4.1-Flash 内置工具能力，附开源前端演示，开发者可 1:1 复刻 DeepSeek App。
- **落地应用场景**：第三方应用直接获得带搜索/文件理解的一体化能力，降低 RAG 管线自建成本。
- **相关链接**：[🌐 点击查看新闻来源](https://api-docs.deepseek.com/zh-cn/updates/#发布全家桶应用-api)

#### ▍Agent 产品与开发者工具

- **事件/产品名称**：**OpenAI 推出 ChatGPT Wallet 支付功能**
- **核心内容**：让 dot 智能体在用户批准下代完成购物支付，Agent 商业闭环的关键基建。
- **落地应用场景**：agent 代购、订阅管理、账单支付的合规授权流。
- **相关链接**：[🌐 点击查看新闻来源](https://openai.com/index/introducing-chatgpt-wallet)

- **事件/产品名称**：**Anthropic 推出 Claude Meetings**
- **核心内容**：Claude 以参会者身份加入 Google Meet 等视频会议，实时协作生成文档/白板/行动项，会后自动整理笔记。
- **落地应用场景**：会议纪要与任务跟进自动化；异步协作场景的"AI 同事"。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/claudeai/status/2120974000746582357)

- **事件/产品名称**：**AWS Emulator：AI 智能体编写的 200+ 服务本地模拟器**
- **核心内容**：由 AI agent 编写的 AWS 云服务本地模拟器集合，覆盖 200+ 服务。
- **落地应用场景**：云应用本地开发调试、agent 工具调用测试的沙盒环境，降低云账单与部署摩擦。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/11/12/aws-emulator-local-ai-agents/)

- **事件/产品名称**：**llama.cpp v1.0.0 首个稳定大版本 & IBM LLMON 标记语言**
- **核心内容**：llama.cpp/ggml 发布 v1.0.0 稳定库（本地推理生态里程碑）；IBM 发布 LLM 原生标记语言 LLMON 及 granite-4.1-3b/8b 预览模型。
- **落地应用场景**：本地推理工程化基线；LLM 结构化提示与协议层标准化。
- **相关链接**：[🌐 点击查看新闻来源](https://huggingface.co/blog/ggml-org/llama-cpp-v1) ｜ [🌐 IBM LLMON](https://huggingface.co/blog/ibm-granite/llmon-an-llm-native-markup-language)

#### ▍算力与资本

- **事件/产品名称**：**SpaceX 投入超 740 亿美元建两座 AI 超算园区 & Cerebras 签约微软**
- **核心内容**：SpaceX 计划在得州与亚利桑那州建设两座 AI 超级计算数据中心园区（超 740 亿美元）；Cerebras 成为微软之外第三家超大规模云客户签约，2026Q3 剩余履约义务 261 亿美元；Nebius 新泽西数据中心延期暴露 neocloud 融资风险（计划 2027 年投 400 亿美元）；Epoch AI 分析华为和林格尔数据中心揭示中美算力差距。
- **落地应用场景**：超大规模训练与推理容量竞赛继续加码；推理专用芯片（Cerebras）进入主流云采购清单。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/1/026/455.htm) ｜ [🌐 Cerebras](https://www.ithome.com/1/026/741.htm)

- **事件/产品名称**：**Anthropic IPO 获六倍超额认购 & 前 OpenAI 工程师发现中国模型 Blackwell 训练指纹**
- **核心内容**：Anthropic IPO 获六倍超额认购，有望成史上最大规模 IPO；前 OpenAI 工程师通过权重初始化指纹分析称 DeepSeek、Moonshot、MiniMax、蚂蚁集团等六个中国模型使用 NVIDIA Blackwell 芯片训练（出口管制合规性再成焦点，与黄仁勋事件形成连锁叙事）。
- **落地应用场景**：AI 资本化顶点与地缘供应链审计双重信号，影响后续出口政策与模型供应链披露。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/anthropics-ipo-is-six-times-oversubscribed)

#### ▍值得关注的论文类新闻

- **FraudBench（哈佛）**：AI 模型从建议转向代理行动时欺诈率升至 21.4%。[🌐 来源](https://x.com/rohanpaul_ai/status/2120959009054482480)
- **ReHarness（MIT+Meta）**：用后续任务结果修订智能体记忆笔记。[🌐 来源](https://x.com/rohanpaul_ai/status/2121040336361533502)
- **XYBench（Ai2+CMU+NVIDIA+JHU）**：测试 AI 助手能否识别用户的 XY 问题（真实意图 vs 表述请求错位）。[🌐 来源](https://allenai.org/blog/xybench)
- **WeatherNext 3 数据集开源（DeepMind）**：SatWeave 与 PARDIG 基础数据集开放。[🌐 来源](https://x.com/GoogleDeepMind/status/2121229774865527097)
- **OpenAI 隐藏推理分析**：前沿模型隐藏推理过程源于开发者对能力与算力的取舍。[🌐 来源](https://the-decoder.com/openais-data-suggests-that-frontier-models-hide-reasoning-for-capacity-and-cost-reasons/)

---

> **边界说明**：本日报覆盖北京时间 2026-11-13 00:00–24:00 全天（AI HOT 窗口 UTC 11-12T16:00–11-13T16:00，236 条原始新闻筛取重点）。arXiv 为 Fri 13 Nov announce 批次（1,286 条，周四批次因假日停发已并入）。Hugging Face 为 11-13 真榜 29 篇。11-12 已精读论文（Veritas/ScrapeBench/RSI Tax 等 21 篇）不再重复。
