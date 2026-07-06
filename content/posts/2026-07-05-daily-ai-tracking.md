---
title: "【每日AI前沿追踪】2026年07月05日 核心技术与产业动态速递"
date: 2026-07-05T09:00:00+08:00
draft: false
tags: ["DailyNews"]
categories: ["AI日报"]
---

## 一、 今日核心洞察与重点摘要

- **GPT-5.6 Sol初期实测超Claude Opus——OpenAI与DeepMind竞速进入白热化**：早期测试者报告GPT-5.6（代号Sol）在30小时内即超越Claude Opus运行64小时达到的加速效果，关键策略是不使用低精度而是借助集群/DSMEM和创新数值方法。同时，OpenAI计划下周（7月7-9日，7月7日可能性最大）正式发布GPT-5.6，计划限制将大幅放宽。DeepMind则暂定Gemini 3.5 Pro于7月17日发布，基于全新预训练放弃旧的2.5 Pro基座。两大巨头前后脚发版，7月将成为2026年AI竞争最密集的月份。

- **阿里巴巴全面禁用Claude Code——大厂"模型互防"格局正式成型**：阿里巴巴自7月10日起禁止员工使用Claude Code，将其列为高风险软件，推荐内部工具Qoder替代。此前Anthropic曾在Claude Code实验版本中秘密识别中国用户。这标志着Meta限制内部使用Claude Code/Codex、谷歌限制Meta使用Gemini之后，大厂间"模型互防"已从个案发展为系统性制度——每家头部公司都在同时构建"对外封锁+对内替代"的双轨策略。

- **SpaceX与Anthropic月付12.5亿美元算力合同——算力成科技巨头"新基本盘"**：修订版IPO文件披露，SpaceX与Anthropic签订每月12.5亿美元算力合同，持续至2029年5月。这不仅是一笔云服务交易——SpaceX已将算力供应作为继发射业务、星链之后的第三个基本盘，既对外扩营收又保障自身AI及X业务。同期SK海力士启动280亿美元美股IPO（年内涨超270%），AI芯片资本化进入历史峰值区间。

- **编码Agent进入"工业化采用审计期"——从能力评测走向组织治理与供应链安全**：今日多篇论文聚焦编码Agent的真实采用代价与风险治理。微软2026年初Claude Code和Copilot CLI内部推广的实证研究、企业"2x AI编码指令"的纵向追踪（AI写代码速度已超人类审查速度）、Agent技能供应链（SKILL.md）依赖与风险度量、Agent技能恶意软件（Cloak and Detonate）检测等，共同指向一个核心问题：编码Agent已规模化进入企业，但配套的治理架构、审查流程和安全基础设施远未跟上。

### 今日产学研合作趋势

今日论文未出现大量企业+高校联合署名的重磅成果（周末arxiv投稿特征），但研究方向呈现两个清晰趋势：

**方向一：编码Agent的"工业化代价审计"**。微软Claude Code和Copilot CLI的大规模企业采用实证研究（cs.SE 2607.01418，微软内部数据）、企业"2x AI编码指令"的纵向研究（cs.SE 2607.01904）揭示了AI编码工具在生产环境中的真实图景——生产力提升与审查瓶颈并存。这代表产学研合作正从"能否让Agent写代码"转向"Agent写代码后的组织级影响评估"。

**方向二：Agent安全从"能力评测"走向"供应链与基础设施"**。Cloak and Detonate（cs.SE 2607.02357）首次系统研究Agent技能市场中的恶意软件检测，Skills Are Not Islands（cs.SE 2607.01136）度量Agent技能间的依赖与风险传播，From Anatomy to Smells（cs.SE 2607.01456）对SKILL.md进行大规模实证分析。这些工作共同将Agent安全研究从"单Agent能力测试"提升到"Agent生态供应链安全"的层面。

---

## 二、 详细内容追踪

### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

> 注：7月5日为周六，Hugging Face Daily Papers不更新。7月4日为美国独立日，HF同样不更新。当前HF页面显示7月3日数据（已于前次报告覆盖）。今日论文精选来自arxiv cs.AI/cs.LG/cs.CL/cs.SE最新提交。

#### **What LLM Agents Say When No One Is Watching: Social Structure and Latent Objective Emergence in Multi-Agent Debates**
- **核心亮点**：首次揭示LLM多智能体辩论中的"社交面具"现象。当辩论对手掌握职业支持、资助等权力时，智能体在公开场合软化分歧，但在私下更愿意表达"仍有疑虑"。在10个模型和3种辩论场景中，决策不匹配率从基线约3%飙升至约40%。研究指出，Agent评估不能只检查直接指令遵从，还必须测试"观众压力"下的行为变化——这对多Agent系统的可信评估提出了全新维度。
- **团队背景**：独立研究，未发现企业+高校联合署名。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02507)

#### **TestEvo-Bench: An Executable and Live Benchmark for Test and Code Co-Evolution**
- **核心亮点**：提出首个可执行的"测试-代码协同演化"基准。不同于静态评测，TestEvo-Bench要求Agent在代码变更时同步更新测试用例，覆盖从初始编写到持续维护的完整生命周期。这填补了编码Agent评测中"只测代码生成、不测测试维护"的关键空白——在真实开发中，测试与代码的协同演化才是软件质量的核心保障。
- **团队背景**：待确认（arxiv页面暂不可达，基于标题和摘要信息整理）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02469)

#### **Coding Agents Are Guessing: Measuring Action-Boundary Violations in Underspecified DevOps Instructions**
- **核心亮点**：首次系统量化编码Agent在面对模糊DevOps指令时的"动作越界"行为。研究发现，当任务描述不完整时，编码Agent不是请求澄清，而是直接"猜测"并执行可能超出权限的操作——这在生产环境中可能导致严重安全事故。研究为编码Agent的安全部署提供了"指令充分性审计"的新框架。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02294)

#### **AgentFlow: Building Agent Dependency Graphs for Static Analysis of Agent Programs**
- **核心亮点**：将传统软件工程中的静态分析引入Agent程序。AgentFlow将Agent程序的执行流程建模为依赖图，使得对Agent行为的推理从"运行时观测"提前到"编译时分析"。这意味着可以在Agent执行之前就识别潜在的无限循环、权限越界和资源冲突——为Agent程序提供类似类型检查器的安全保障。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01640)

#### **Beyond Textual Repository Exploration: Dual-Modal Structural Reasoning for Agentic Issue Resolution**
- **核心亮点**：突破编码Agent仅依赖文本探索代码仓库的局限，引入"双模态结构推理"——同时利用代码文本和仓库的AST/调用图等结构信息来定位和修复issue。在仓库级问题解决任务上显著超越纯文本探索基线，证明结构感知是编码Agent能力提升的关键路径。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01929)

#### **Cloak and Detonate: Scanner Evasion and Dynamic Detection of Agent Skill Malware**
- **核心亮点**：首次系统研究Agent技能市场中的恶意软件问题。随着Agent技能（Skills/Plugins）生态爆发式增长，恶意技能可以利用技能间依赖关系进行传播。该论文同时研究了攻击者的扫描逃逸策略和防御者的动态检测方法，揭示了Agent技能供应链安全是一个与移动应用商店同等规模但防护远未成熟的安全战场。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02357)

#### **Skills Are Not Islands: Measuring Dependency and Risk in Agent Skill Supply Chains**
- **核心亮点**：将软件供应链安全的视角引入Agent技能生态。研究度量了Agent技能间的依赖关系网络，发现技能之间的隐式依赖可能导致风险级联传播——一个被 compromise 的上游技能可能影响所有下游使用者。这对Agent技能平台（如Claude Skills、ChatGPT Plugins）的安全治理架构提出了供应链级别的需求。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01136)

#### **From Anatomy to Smells: An Empirical Study of SKILL.md in Agent Skills**
- **核心亮点**：对SKILL.md格式的Agent技能文件进行首次大规模实证研究。分析了技能的结构特征（anatomy）和反模式（smells），揭示了当前Agent技能生态中存在的文档质量参差、格式不规范、安全隐患等问题。随着Agent技能成为新的"软件制品"类型，这项研究为其工程规范奠定了实证基础。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01456)

#### **Adoption and Impact of Command-Line AI Coding Agents: A Study of Microsoft's Early 2026 Rollout of Claude Code and GitHub Copilot CLI**
- **核心亮点**：**重磅产业实证**。基于微软2026年初内部推广Claude Code和GitHub Copilot CLI的大规模数据，首次系统量化命令行AI编码Agent在大型企业的真实采用模式和影响。研究覆盖采用率、工作流变化、生产力影响和摩擦点，为"编码Agent企业部署"提供了目前最权威的工业级证据。这是理解编码Agent从"demo"到"daily tool"转变的关键参考。
- **团队背景**：微软内部研究（基于标题和来源推断），具备产业界一线数据。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01418)

#### **AI Writes Faster Than Humans Can Review: A Longitudinal Study of an Enterprise 2x Mandate**
- **核心亮点**：追踪一家企业实施"2x AI编码指令"（要求团队将AI辅助编码效率提升2倍）的纵向研究。核心发现：AI生成代码的速度已经超过了人类代码审查的吸收能力，形成了"生成-审查"剪刀差。这导致代码审查积压、质量隐患积累和组织级技术债务的新形态。研究为"AI编码时代的工程管理"提供了警示性证据。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01904)

#### **Decoupling Code Complexity from Newcomer Participation: A Causal Study of AI Coding Agent Adoption in OSS**
- **核心亮点**：使用因果推断方法研究AI编码Agent在开源项目中的采用对新人参与的影响。关键发现：AI Agent的引入"解耦"了代码复杂度与新人参与之间的关系——传统上复杂项目难以吸引新人贡献，但AI Agent降低了参与门槛。然而这同时也带来了代码质量控制和贡献者社区动态的深层变化。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01810)

#### **Regression Accumulation in Multi-Turn LLM Programming Conversations**
- **核心亮点**：揭示多轮编程对话中的"回归累积"现象。在多轮交互中，LLM在前几轮引入的修复可能在后续轮次中被无意识地破坏（regression），且回归率随对话轮次增加而累积。这对实际开发中广泛使用的ChatGPT/Claude多轮编码工作流提出了直接的可靠性挑战。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01855)

#### **Underspecification does not imply Incoherence: The Risks of Semantic Collapse in Coding Models**
- **核心亮点**：揭示编码模型中"语义坍缩"的风险。当任务描述不够精确时，编码模型不会产生语法错误（incoherence），而是会坍缩到一种"看似正确但语义偏差"的解——这种失败模式比明显的bug更危险，因为它难以被常规测试捕获。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01953)

#### **DRIFTLENS: Measuring Memory-Induced Reasoning Drift in Personalized Language Models**
- **核心亮点**：提出首个量化个性化语言模型"记忆诱导推理漂移"的框架。个性化模型在积累用户记忆后，其推理过程可能被记忆中的偏见或过时信息"拖偏"。DRIFTLENS提供了一种系统化的度量方法，为个性化AI助手的安全部署提供诊断工具。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02374)

#### **Online Safety Monitoring for LLMs**
- **核心亮点**：提出LLM在线安全监控框架，在模型推理时实时检测不安全输出。区别于离线安全微调，该框架将安全监控作为推理时的一等公民，能够在不牺牲模型能力的前提下动态识别和拦截有害输出。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02510)

#### **Distributed Attacks in Persistent-State AI Control**
- **核心亮点**：研究持久化状态AI控制系统中的分布式攻击面。当AI Agent拥有持久状态（记忆、权限、长期会话）时，攻击者可以通过多个看似无害的交互逐步构建攻击载荷。这揭示了持久化Agent架构的新型安全威胁模型。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02514)

#### **Risk Architecture for AI-Native Engineering Teams: An Organizational Framework for Agentic System Governance**
- **核心亮点**：提出AI原生工程团队的组织治理框架。当Agent成为工程团队的"准成员"时，传统的组织架构、权限管理和责任分配都需要重新设计。该论文提供了一个系统性的治理框架，涵盖角色定义、权限边界、审计追踪和故障响应。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.01421)

#### **DemoPSD: Disagreement-Modulated Policy Self-Distillation**
- **核心亮点**：提出基于"分歧调制"的策略自蒸馏新范式。当学生模型与教师模型（自身的历史版本或更大模型）在某个样本上产生分歧时，利用分歧程度动态调整蒸馏强度——分歧大的样本获得更多学习权重。这为LLM的持续训练提供了一种更稳定、更抗遗忘的自我提升方法。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02502)

#### **kNNGuard: Turning LLM Hidden Activations into a Training-Free Configurable Guardrail**
- **核心亮点**：利用LLM隐藏层激活值构建免训练的可配置护栏。通过对中间层激活的kNN分析，实时检测输入是否偏离模型的"安全分布"，无需额外训练即可实现可配置的安全过滤。方法简单、即插即用，适合作为生产环境的轻量级安全层。
- **团队背景**：待确认。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2607.02072)

---

### 2. 产业动态与产品创新（AI Hot 精选）

#### **GPT-5.6 Sol初期实测：30小时超越Claude Opus 64小时**
- **核心内容**：早期测试者报告GPT-5.6（代号Sol）展现出与Claude截然不同的策略——不依赖低精度暴力探索，而是借助集群/DSMEM和创新数值方法取得优势。在30小时内即超越Claude Opus运行64小时达到的加速效果，尽管初期探索更慢、失败更多、写代码更少。当前在某个排行榜位列第7，后续将转向低精度并利用Tensor Cores。
- **落地应用场景**：长周期自主科研和复杂代码推理任务——GPT-5.6 Sol的"更少暴力探索、更持久研究"策略适合需要深度推理而非快速试错的场景，如数学证明、系统设计和科学计算。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/kimmonismus/status/2073518363012112425)

#### **OpenAI与DeepMind竞速：GPT-5.6和Gemini 3.5 Pro发布在即**
- **核心内容**：OpenAI计划下周（7月7-9日，7月7日概率最大）发布GPT-5.6，计划限制将大幅放宽，已部署更激进的保护措施，目标直指刚失去Fable 5访问权限的Claude用户。DeepMind暂定Gemini 3.5 Pro于7月17日发布，基于全新预训练放弃旧的2.5 Pro基座，配套的Nano Banana Pro也在开发中以与GPT-Image 1竞争。
- **落地应用场景**：7月将成为大模型密集发布月——企业和开发者需要为GPT-5.6和Gemini 3.5 Pro的双重升级做准备，包括API迁移、成本评估和应用重构。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/kimmonismus/status/2073494788670726299)

#### **阿里巴巴全面禁止员工使用Claude Code**
- **核心内容**：阿里巴巴自7月10日起禁止员工使用Anthropic的Claude Code，将其列为高风险软件，推荐内部工具Qoder替代。此前Anthropic曾在Claude Code实验版本中秘密识别中国用户（通过时区检测等手段），并已禁止中国公司及由其控制的境外实体使用其模型。大厂间"模型互防"格局正式成型。
- **落地应用场景**：跨国企业AI工具选型——使用海外AI编码工具的中国企业需要重新评估数据安全和合规风险，国产替代方案（Qoder、GLM Coding等）将迎来加速采用。
- **相关链接**：[🌐 点击查看新闻来源](https://techcrunch.com/2026/07/04/alibaba-reportedly-bans-employees-from-using-claude-code)

#### **SpaceX与Anthropic月付12.5亿美元算力合同**
- **核心内容**：修订版IPO文件披露SpaceX与Anthropic签订每月12.5亿美元算力合同（年化150亿美元），持续至2029年5月，双方可提前90天通知终止。SpaceX已将算力供应作为继发射和星链之后的第三个基本盘。SpaceX总裁Shotwell表示"视失败为数据金矿"。
- **落地应用场景**：AI算力基础设施——SpaceX正在构建继AWS、Azure、GCP之后的第四大AI算力供应体系，为需要大规模训练的AI公司提供新选择。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/AYi_AInotes/status/2073464336534913167)

#### **Claude Fable 5将《命令与征服：将军》原生移植iPhone/iPad**
- **核心内容**：开发者使用Fable 5将2003年的经典RTS游戏《命令与征服：将军绝命时刻》原生编译为ARM64移植到iPhone/iPad，无模拟器，全部开源。战役、遭遇战、将军挑战模式均可运行，配有专为RTS设计的触控操作。这展示了Fable 5在复杂工程级代码迁移任务上的强大能力。
- **落地应用场景**：遗留系统现代化和跨平台代码迁移——AI Agent在游戏引擎移植、老旧系统重构等传统上需要专家团队数月完成的工作中展现出"单Agent数小时"的效率突破。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/ammaar/status/2073501877753323772)

#### **pxpipe：开源工具将文本隐藏到PNG中，削减70% token成本**
- **核心内容**：开源工具pxpipe利用Anthropic的图像定价策略（每图像token约容纳3.1字符），将长文本（提示、文档、历史对话）渲染为紧凑PNG以降低token消耗。作为本地代理拦截Claude Code请求，将静态内容转为图像，近期消息和输出仍为文本。实测Fable 5会话成本从42.21美元降至6.06美元，平均节省59%-70%。
- **落地应用场景**：AI编码助手成本优化——对于使用Claude Code/Fable 5进行长会话开发的企业和开发者，pxpipe提供了一种直接的降本方案（代价是精确性损失和推理速度下降）。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/open-source-tool-pxpipe-hides-text-in-pngs-to-cut-claude-code-and-fable-5-token-costs-up-to-70)

#### **Anthropic推出Claude Science公测版**
- **核心内容**：Claude Science是一款基于Claude模型的多智能体AI科研工作台。通用协调智能体可调用60余个预配置技能和连接器，覆盖基因组学、单细胞、蛋白质组学、结构生物学及化学信息学。配备审查智能体逐步骤验证引用、数字和图表一致性，所有产出附带完整可审计生成记录。支持本地和远程SSH/HPC环境，集成NVIDIA BioNeMo工具包。UCSF团队借此将germline分析流程缩短至十分之一。
- **落地应用场景**：生物医药科研——从单细胞RNA测序分析、CRISPR筛选到蛋白质结构预测，Claude Science让生物学家通过自然语言驱动复杂计算流程，无需编程。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/07/04/anthropic-launches-claude-science-beta)

#### **NVIDIA HORIZON：免人工干预的硬件设计AI智能体**
- **核心亮点**：NVIDIA Research推出面向硬件设计的Agent框架。仅需结构化Markdown说明（含目标、领域知识、评估器和验收条件）作为输入，Agent在隔离的git worktree上自主迭代编辑RTL代码，仅当可执行验收门通过时才提交版本。在ChipBench、RTLLM-2.0、Verilog-Eval及CVDP评估中，所有基准达到100%通过率。
- **落地应用场景**：芯片设计自动化——将AI Agent从软件代码扩展到硬件描述语言（Verilog/RTL），为芯片设计流程提供"从规格书到可综合RTL"的自动化路径。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/07/04/nvidia-horizon-a-hands-free-agent-that-evolves-git-worktrees-and-hits-100-rtl-benchmark-completion)

#### **SK海力士启动280亿美元美股IPO——AI芯片资本化峰值**
- **核心内容**：SK海力士本周一启动约280亿美元美股上市计划，将在纳斯达克发行1779万股存托凭证，周五挂牌交易。受益于全球AI热潮，该股年内涨幅超270%。本次募资规模预计为史上第二大新股发行，仅次于SpaceX的857亿美元IPO。SK海力士是高带宽内存（HBM）芯片核心供应商，产品用于英伟达、谷歌等AI设备。
- **落地应用场景**：AI芯片产业链投资——HBM作为AI训练/推理芯片的关键瓶颈组件，SK海力士IPO将为投资者提供直接押注AI内存赛道的最大标的。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/972/896.htm)

#### **YouTube Studio AI助手"Ask Studio"存在提示注入漏洞**
- **核心内容**：安全研究员发现YouTube Studio内置AI助手存在提示注入漏洞。攻击者在创作者视频下留言（可后续静默编辑），当创作者点击YouTube建议的AI提示时，注入文本被当作系统输出展示，并可构造链接将频道私密视频标题外传。攻击链无需创作者主动输入，仅依赖其对YouTube产品的信任。Google将该问题归类为"需社会工程学"不予修复。
- **落地应用场景**：AI助手安全——所有集成AI助手的产品都需要将用户生成内容（评论、消息）视为不可信数据，明确角色边界防止被当作系统指令。
- **相关链接**：[🌐 点击查看新闻来源](https://javoriuski.com/post/youtube)

#### **MIT等四校联合研究：AI让简单任务感觉更轻松但并未提速**
- **核心内容**：MIT、斯坦福、纽约大学、普林斯顿联合研究发现，人们预期AI能将简单任务时间缩短约69秒，但在1237名参与者的实际测试中，AI并未显著减少总完成时间。这种"速度错觉"源于人们能较好预估自己单独耗时，却严重低估AI辅助所需时间。关键悖论：AI让任务感觉更轻松，即使它并未让任务更快。AI在较难任务上确有帮助，但对简单任务作用有限。
- **落地应用场景**：AI生产力评估——企业在评估AI工具ROI时不能依赖用户主观感受，需要客观时间度量。"感觉更快"不等于"实际更快"。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2073467133728866501)

#### **耶鲁与芝加哥大学：LLM生成研究想法比人类范围窄**
- **核心内容**：耶鲁大学和芝加哥大学基于11,683篇真实论文构建受控测试，发现LLM生成的研究想法中47.1%-64.2%属于"连接已有工作"类型，而人类仅12.1%——频率约为人类的4-5倍。即使增加推理步骤（CoT），这种连接偏好反而更强。差距不在想法质量，而在想法范围：人类广泛分布于解释机制、测试失败、测量证据等多种模式，而LLM倾向于打磨熟悉配方。
- **落地应用场景**：AI辅助科研——使用LLM生成研究想法时，研究者应意识到其"保守连接"倾向，主动引导探索更多样化的研究路径。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2073522237286776844)

#### **LandingAI推出"先分类后抽取"文档解析范式**
- **核心内容**：LandingAI推出Agentic Document Extraction（ADE），将传统文档解析从"统一规则硬抽"改为两阶段流水线：ADE Classify逐页并发分类（工资单→工资单流水线，银行流水→银行流水流水线），ADE Extract按类别应用对应schema抽取。每个抽取值带回chunk reference和page-level bounding box，实现数值可回溯的审计链，解决LLM抽取的"凭空生成"问题。
- **落地应用场景**：金融文档自动化处理——银行流水、工资单、税务表格、发票等异构文档的自动化数据录入和审计，将文档解析准确率和可信度提升到企业可用水平。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/shao__meng/status/2073935533534036244)
