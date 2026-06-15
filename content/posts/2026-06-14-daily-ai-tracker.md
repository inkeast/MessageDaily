---
title: "【每日AI前沿追踪】2026年06月14日 核心技术与产业动态速递"
date: 2026-06-14
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "6月14日AI前沿速递：亚马逊CEO Andy Jassy向白宫举报Anthropic Fable 5安全漏洞，美国政府随即实施出口管制，Fable 5/Mythos 5全面下线；智谱GLM-5.2全量开放1M上下文+下周开源，国产最强编码引擎；MiniMax M3开源109B参数块级稀疏注意力实现28.4倍计算削减；Databricks开源Omnigent多智能体元编排框架；OpenRouter Fusion API半价达Fable级智能；EvoArena揭示Agent在动态环境准确率仅39.6%。"
---

## 【每日AI前沿追踪】2026年06月14日 核心技术与产业动态速递

### 一、 今日核心洞察与重点摘要

- **亚马逊CEO举报触发政府出口管制——Anthropic Fable 5/Mythos 5全面下线**：Politico与《华尔街日报》披露完整内幕：亚马逊CEO Andy Jassy率先向白宫报告Fable 5安全护栏可被绕过，随后财政部长Bessent、商务部长Lutnick与Anthropic CEO Amodei进行了三次紧张通话。Amodei要求更多时间未承诺撤下，特朗普政府当晚直接实施出口管制。更戏剧性的是，**至少六家科技公司高管联合向政府报告了该漏洞**，且亚马逊是Anthropic最大投资者之一——这标志着AI行业竞争已从技术竞赛延伸到"行政手段博弈"。同时有人将Fable5以3.4TB上传至Pirate Bay，前沿AI竞争进入混沌阶段。

- **中国开源模型全线出击——GLM-5.2全量开放1M上下文 + MiniMax M3社区生态爆发**：智谱GLM-5.2面向GLM Coding Plan全量用户开放真正可用的100万上下文窗口，下周正式开源，强调"AGI不应被高墙垄断"。MiniMax M3（109B参数）上线HuggingFace并接入InferenceX，发布首日社区即提交解码加速优化，赋能Hermes Agent实现从零自学TouchDesigner创作。**在中国模型面临外部封锁的背景下，"激进开源"正在成为国产大模型的集体战略选择。**

- **多智能体编排成为基础设施层新焦点——Databricks开源Omnigent + OpenRouter Fusion API**：Databricks联合创始人Matei Zaharia开源Omnigent——一个位于Claude Code、Codex、Pi之上的元编排框架（meta-harness），6周建成，支持多智能体协作辩论与实时人工会话共享。OpenRouter同步推出Fusion API，将同一提示词并行发送给多个模型、允许调用工具，再由法官模型比较、合成器生成最终回复，**以一半价格达到Fable级别智能**。这两个项目共同指向一个趋势：**"单模型统治一切"的叙事正在瓦解，"组合调用不同模型"正在成为工程团队的战略方向。**

- **Google重塑搜索20年最大变革 + AI智能体全面进入国民级应用**：Google在AI模式中正式推出搜索智能体功能，可全天候自动监测全网信息并即时推送。蚂蚁集团对支付宝进行重大改版引入AI助手"阿宝"，支持语音点咖啡、买基金等指令。华为HarmonyOS 7.0 Developer Beta 1首创鸿蒙内核应用快启技术，DevEco Code AI编程Agent实现需求到代码闭环。**AI智能体正在从"开发者工具"全面渗透到10亿用户的日常应用场景。**

**产学研合作趋势观察**：今日产学合作呈现三大特征：① **Agent研究从"能力构建"全面转向"环境工程与知识编排"**——EurekAgent（清华KEG）提出"环境工程"框架用于自主科学发现，以不到11美元发现26-circle packing最优结果；EvoArena（NUS×UCL×Salesforce）揭示Agent在动态环境中准确率仅39.6%，表明Agent记忆可靠性远未解决；FORT-Searcher（人大RUCAIBox）提出"捷径感知"框架训练深度搜索Agent。② **搜索Agent成为产学合作最密集的方向**——TreeSeeker（微软研究院MSRA团队）将UCB多臂老虎机思想引入深度搜索，FORT-Searcher（人大）和EvoBrowseComp分别从训练数据合成和演化知识两个维度推进搜索Agent前沿。③ **中国AI企业与高校的"算力+人才"深度绑定模式持续深化**——LabVLA（浙大×上海AI Lab）首次将VLA模型系统应用于科学实验室，InterleaveThinker（港中文LEAP Lab）提出首个多Agent交错生成流水线，均为典型的"企业出算力+高校出人才"合作模式。

---

### 二、 详细内容追踪

#### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

##### 📄 **EvoArena: Tracking Memory Evolution for Robust LLM Agents in Dynamic Environments**（🔥127票，当日最高）

- **核心亮点**：现有Agent评估假设环境是静态的，但真实世界会持续变化（软件更新、社交关系演变、终端配置调整）。EvoArena首次将环境变化建模为跨终端、软件和社交领域的渐进式更新序列，并提出EvoMem基于补丁的记忆范式跟踪演化历史。**关键发现：当前最强Agent在EvoArena上的平均准确率仅为39.6%**，EvoMem在GAIA上提升6.1%、在LoCoMo上提升4.8%，但整体改善有限——说明Agent记忆可靠性远未解决。
- **团队背景**：⚠️ **产学研合作（加分项）**——作者横跨新加坡国立大学（Bryan Hooi）、伦敦大学学院UCL（Jun Wang）、Salesforce AI Research（Caiming Xiong、Zhiyuan Hu）、卡内基梅隆大学CMU（Shuyue Stella Li）、卡迪夫大学等多家顶级机构，是典型的国际产学研大合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13681)

##### 📄 **MiniMax Sparse Attention**（🔥120票）

- **核心亮点**：提出基于GQA的块级稀疏注意力机制MSA，通过轻量级Index Branch对KV块评分、为每个GQA组独立选择Top-k子集，Main Branch仅对选中块执行精确注意力。在109B参数、1M上下文的生产级多模态模型上，**per-token注意力计算量减少28.4倍**，配合定制内核在H800上实现14.2倍prefill加速和7.6倍decoding加速。设计理念简洁可扩展，NVIDIA、Modular、vLLM均在Day 0提供支持。
- **团队背景**：MiniMax-AI（稀宇科技）企业内部研究团队主导，是百亿参数级生产模型中首次验证稀疏注意力实际部署效果的工作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13392)

##### 📄 **WeaveBench: A Long-Horizon, Real-World Benchmark for Computer-Use Agents with Hybrid Interfaces**（🔥97票）

- **核心亮点**：针对计算机使用Agent（Computer-Use Agent）评估的痛点——现有基准要么任务短、要么接口单一。WeaveBench构建了一个长期、真实世界的混合接口基准测试，评估Agent在GUI+CLI+API混合环境中的综合表现。揭示了当前Agent在长周期、多接口场景下能力严重不足的问题。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.09426)

##### 📄 **SpatialClaw: Rethinking Action Interface for Agentic Spatial Reasoning**（🔥86票）

- **核心亮点**：提出无需训练的空间推理框架——采用**代码作为动作接口**，维护有状态的Python内核，预加载感知与几何原语，允许VLM智能体每一步编写可执行单元并基于所有先前输出条件化操作。在20个空间推理基准上平均准确率达59.9%，超越近期空间智能体+11.2个百分点，覆盖静态和动态3D/4D任务，在6个VLM主干上均取得一致提升。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13673)

##### 📄 **InterleaveThinker: Reinforcing Agentic Interleaved Generation**（🔥77票）

- **核心亮点**：首个多智能体交错生成流水线，赋予任何现有图像生成器交错生成（文本-图像序列）能力。由Planner Agent组织序列、Critic Agent评估并优化重生成指令，通过GRPO强化逐步纠正。创新性地提出accuracy reward和step-wise reward，使单步RL能有效指导超25次生成器调用的完整轨迹。**性能可与Nano Banana和GPT-5媲美**，且意外在推理类基准上也显著提升基础模型性能。
- **团队背景**：⚠️ **产学研合作（加分项）**——通讯作者Dian Zheng及Hongsheng Li通常关联于香港中文大学（CUHK），是港中文LEAP Lab的多智能体生成研究。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13679)

##### 📄 **FORT-Searcher: Synthesizing Shortcut-Resistant Search Tasks for Training Deep Search Agents**（🔥71票）

- **核心亮点**：现有深度搜索Agent训练数据合成存在"捷径"问题——看似复杂的图结构，实际搜索可通过捷径崩塌。FORT框架形式化定义了结构复杂度与实际搜索难度之间的差距，识别出四类可操作捷径风险（证据共覆盖、单线索选择性、常数暴露、先验知识绑定），并用轨迹签名诊断捷径效果。仅用SFT训练的FORT-Searcher在同等规模开源搜索Agent中取得**最佳整体性能**。
- **团队背景**：⚠️ **产学研合作（加分项）**——来自中国人民大学（RUC）AI实验室RUCAIBox，由赵鑫教授和文继荣教授领衔（文继荣教授此前为微软亚洲研究院高级研究员）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.12087)

##### 📄 **LabVLA: Grounding Vision-Language-Action Models in Scientific Laboratories**（🔥52票）

- **核心亮点**：首次将VLA模型系统应用于科学实验室场景。提出了RoboGenesis数据与仿真引擎（从原子技能组合配置实验室工作流），以及LabVLA两阶段策略（FAST action token预训练+flow matching后训练附加DiT action expert）。在LabUtopia基准上，分布内和分布外设置下均取得所有基线方法中**最高的平均成功率**。
- **团队背景**：⚠️ **产学研合作（加分项）**——浙江大学知识引擎实验室（zjunlp）×上海人工智能实验室（Lei Bai、Dongzhan Zhou），18人大型合作团队。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13578)

##### 📄 **MaxProof: Scaling Mathematical Proof with Generative-Verifier RL and Population-Level Test-Time Scaling**（🔥76票）

- **核心亮点**：提出种群级测试时间扩展框架，应用于MiniMax-M3系列模型。M3同时训练了证明生成、验证、修复三种能力，使用纵深防御生成验证器。测试时模型同时充当生成器、验证器、精炼器和排序器，通过锦标赛选择返回最终证明。**IMO 2025达到35/42（金牌线）、USAMO 2026达到36/42（金牌线）**，数学定理自动证明达到或超越人类金牌选手水平。
- **团队背景**：MiniMax公司团队（23位作者），摘要中明确引用MiniMax-M3系列模型。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13473)

##### 📄 **EurekAgent: Agent Environment Engineering is All You Need For Autonomous Scientific Discovery**（🔥26票）

- **核心亮点**：提出"环境工程"（Environment Engineering）概念——自主科学发现的瓶颈正从"设计Agent工作流"转向"设计Agent环境"。沿四个维度（权限工程、制品工程、预算工程、人在回路工程）构建EurekAgent系统，以**不到11美元**的总API成本发现了新的**26-circle packing最优结果**，并在数学、内核工程、机器学习任务上创造多项SOTA。
- **团队背景**：⚠️ **产学研合作（加分项）**——清华大学知识工程实验室（KEG），Juanzi Li、Lei Hou等教授领衔，与智谱AI有紧密合作关系。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13662)

##### 📄 **Demystifying Hidden-State Recurrence: Switchable Latent Reasoning with On-Policy RL (SWITCH)**（🔥19票）

- **核心亮点**：现有latent CoT方法难以用标准on-policy RL优化且难以因果解释。SWITCH框架使用一对明确离散边界token（`<swi>`进入/`</swi>`退出潜在模式），使潜在推理块与GRPO兼容，并提出visible-to-latent curriculum训练策略和Switch-GRPO目标函数。机制分析揭示：`<swi>`是精确定位的学习到的切换策略，潜在步骤执行的是特定于问题的因果重要计算。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13106)

##### 📄 **TreeSeeker: Tree-Structured Trial, Error, and Return in Deep Search**（🔥10票）

- **核心亮点**：将经典UCB多臂老虎机思想引入大语言模型深度搜索，提出TreeSearch（树状分支与返回控制）和TreeMem（树状记忆），通过文本化UCB信号在"利用-探索-剪枝"三类决策间动态切换。在XBench-DeepSearch、BrowseComp、BrowseComp-ZH三个基准上均优于强基线，**在"贪婪利用"和"无序探索"之间实现了有纪律的试错平衡。**
- **团队背景**：⚠️ **产学研合作（加分项）**——微软研究院（MSRA）团队，Qingwei Lin、Dongmei Zhang、Fangkai Yang、Pu Zhao等均为MSRA知名研究员，Saravan Rajmohan来自微软研究院。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.11662)

##### 📄 **HarnessBridge: Learnable Bidirectional Controller for LLM Agent Harness**（🔥10票）

- **核心亮点**：针对Agent harness（中介层）手动设计难以扩展的问题，提出轻量级可学习的harness控制器——将Agent与环境接口参数化为双向投影（Observation Projection提炼状态、Action Projection转换动作或拒绝）。在Terminal-Bench 2.0和SWE-bench Verified上匹配或超越强专门化harness，**大幅减少token使用量和轨迹长度**，且从较小生成器训练的模型可迁移至更大商业模型。
- **团队背景**：⚠️ **产学研合作（加分项）**——UCLA团队，Wei Wang、Yizhou Sun教授等，Jason Cong为FPGA领域知名学者。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.12882)

##### 📄 **Getting Better at Working With You: Compiling User Corrections into Runtime Enforcement (TRACE)**（🔥4票）

- **核心亮点**：解决编码Agent"记住纠正在本次但下次仍违反"的问题。TRACE是一个直接插入编码Agent运行时的技能层流水线，挖掘用户聊天中的纠正内容，重写为原子规则，编译成运行时必须通过的门控检查。在ClawArena上，分布外偏好违规从100%降至2.0%，分布内降至37.6%。**从根本上改变了Agent处理历史偏好的方式，用户无需跨会话重复纠正同一问题。**
- **团队背景**：⚠️ **产学研合作（加分项）**——IBM Research（Pin-Yu Chen、Tian Gao）×University of Notre Dame（Nitesh V. Chawla、Xiangliang Zhang），典型企业+高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13174)

##### 📄 **Brick: Spatial Capability Routing for the Mixture-of-Models (MoM) Paradigm**（Arxiv精选）

- **核心亮点**：提出多模态路由器Brick——在6个能力维度上对每个模型评分，结合查询难度估计，通过成本惩罚几何规则调度，提供连续偏好旋钮在"最大质量"和"最大节省"间灵活调整。在5,504个查询上，最大质量模式准确率76.98%超越最佳单一模型；中性模式实现4.71倍成本降低；最小成本模式实现22.15倍成本降低，中位延迟从51.2秒降至22.8秒。
- **团队背景**：Francesco Massa（PyTorch核心贡献者，曾就职Meta AI/FAIR），工业界研究。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13241)

##### 📄 **语言模型需要"睡眠"：通过暂停巩固记忆提升长程推理性能**（AI Hot论文精选）

- **核心亮点**：针对Transformer Agent随上下文增长变慢变贵的问题，提出"睡眠阶段"——模型暂停，多次重读近期上下文，将有用信息通过状态空间块的fast weights写入固定大小的记忆层，然后清空注意力缓存。额外计算在睡眠时完成，正常预测仍只需一次前向传播。在元胞自动机、图查找、GSM-Infinite数学问题上测试表明，**更长的睡眠提升性能，核心启示是长程Agent无需无限扩大原始上下文，可通过巩固重要部分、遗忘原始token来解决。**
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2066099074911359480)

---

#### 2. 产业动态与产品创新（AI Hot 精选）

##### 🌐 **亚马逊CEO举报触发政府出口管制——Fable 5/Mythos 5全面下线**

- **核心内容**：Politico与《华尔街日报》披露完整内幕链条：亚马逊CEO Andy Jassy率先向白宫报告Fable 5安全护栏可被绕过；周五上午白宫官员与Anthropic CEO Amodei三次紧张通话要求撤下；Amodei要求更多时间未承诺；当晚特朗普政府直接实施出口管制。至少六家科技公司高管联合举报，且亚马逊是Anthropic最大投资者。国防部三个月前已将Anthropic永久逐出大楼。有人将Fable5以3.4TB上传至Pirate Bay。Bloomberg纪录片揭示：Anthropic拒绝国防部无护栏要求被拉黑，Claude Code团队6个月100%代码由AI编写，Cowork发布致单日2850亿美元软件股市值蒸发。
- **落地应用场景**：此事件深刻影响AI企业的合规策略——所有使用前沿模型的API服务必须准备"出口管制应急预案"；KYC（了解你的客户）、反代币洗钱、提示词与数据保留功能将成为企业级AI服务的标配（Fable数周后回归时将附带这些功能）。同时，模型安全的定义权正从开发者转移到政府和企业竞争对手手中。
- **相关链接**：[🌐 点击查看新闻来源](https://www.theverge.com/ai-artificial-intelligence/949601/amazon-anthropic-fablemythos-government-ban)

##### 🌐 **智谱GLM-5.2全量开放1M上下文+下周开源**

- **核心内容**：智谱发布迄今能力最强的开源模型GLM-5.2，面向所有GLM Coding Plan用户（Lite/Pro/Max）开放，支持真正可用的100万上下文窗口，在长程任务独立完成方面保持领先，适合构建复杂AI智能体应用。面对外部封锁限制，智谱强调"科学全球性、AGI不应被高墙垄断"，采取激进开源态度。GLM-5.2开源与API预计下周同步上线。MiniMax M3同步赋能Hermes Agent实现从零自学TouchDesigner创作——Agent操控桌面、连接软件、读取参考图像、迭代艺术作品，并将学到的内容保存为可复用技能。
- **落地应用场景**：超长上下文使复杂代码库分析、长文档法律审查、跨会话持续记忆的AI智能体应用成为可能；Hermes Agent的自我学习创作展示了创意工作者（设计师、视频艺术家）与AI协作的新模式——AI从工具进化为"学习伙伴"。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/963/997.htm)

##### 🌐 **Databricks开源Omnigent——多智能体元编排框架**

- **核心内容**：Databricks联合创始人Matei Zaharia开源Omnigent——一个位于Claude Code、Codex、Pi等编码智能体之上的元编排框架（meta-harness），支持组合、上下文策略和实时会话共享，可在终端、网页、桌面和移动端使用。让多个AI智能体协作、辩论并收敛出更优结果，同时支持实时人工协作——可邀请他人加入会话观察、引导和发送命令。6周建成，Apache 2.0许可。
- **落地应用场景**：解决企业"模型锁定"焦虑——不再依赖单一编码Agent，而是让多个Agent协作分工。适用于复杂软件工程项目（不同Agent负责前端/后端/测试/审查）、跨团队AI协作工作流（开发者+设计师+PM通过共享会话协同指导Agent）。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/06/13/databricks-open-sources-omnigent-a-meta-harness-that-composes-governs-and-shares-ai-agents-across-claude-code-codex-and-pi)

##### 🌐 **OpenRouter Fusion API——半价达Fable级智能**

- **核心内容**：OpenRouter推出Fusion API，一种服务器端复合模型，将同一提示词并行发送给多个模型，允许它们调用网络搜索和bash工具。系统通过法官模型比较各模型回答，再由合成器生成最终回复。官方声称Fusion在Perplexity的DRACO深度研究基准上击败前沿模型，**以一半价格达到Fable级别的智能**。在前沿模型选择性开放的趋势下，"组合调用不同模型"正成为工程团队的战略方向。
- **落地应用场景**：深度研究、复杂推理、多视角分析场景——当不确定哪个模型最擅长某个任务时，Fusion自动让多个模型并行尝试并择优。特别适用于AI辅助决策（投资分析、技术选型、法律判断）和知识工作（学术综述、竞品对比）。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/OpenRouter/status/2065856853989270011)

##### 🌐 **Google搜索智能体功能上线——可全天候自动盯全网信息**

- **核心内容**：Google在AI模式中正式推出搜索智能体功能，首批上线信息智能体，可全天候自动监测博客、新闻、社交媒体及实时数据库，覆盖金融行情、商品库存、体育赛事等。用户只需输入"持续为我关注"等句式并补充条件即可设置。相比此前Gemini应用的定时任务（每日或每15分钟一次），新智能体实现即时推送。同时Google对搜索引擎进行20多年来最大变革，将AI生成答案、对话式搜索和推理工具直接集成到核心搜索产品。Google Cloud同步推出Open Knowledge Format（OKF），将散乱文档转为带YAML frontmatter的Markdown文件供AI智能体使用。
- **落地应用场景**：金融从业者实时监控市场动态、电商卖家追踪竞品库存与价格、体育爱好者获取即时赛况。OKF解决了企业知识管理的核心痛点——将PDF、Word、网页等异构文档统一为AI可读的Markdown格式，直接接入Agent工作流。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/963/998.htm)

##### 🌐 **蚂蚁集团为支付宝引入AI助手"阿宝"——超级应用Agent化**

- **核心内容**：蚂蚁集团对支付宝进行重大改版，引入AI Agent交互界面。用户可通过文字或语音向AI助手"阿宝"发出叫网约车、点咖啡、点外卖等指令；在获得授权后，阿宝还能执行买基金、管理投资账户等理财任务。此举标志着与劲敌微信的用户争夺战进一步升级。
- **落地应用场景**：10亿用户的日常生活管理——语音一句话完成出行、餐饮、购物的全链路操作，以及理财基金等高价值金融操作。超级App从"功能入口集合"进化为"自然语言驱动的Agent平台"。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/964/141.htm)

##### 🌐 **华为HarmonyOS 7.0 Developer Beta 1——AI编程Agent实现需求到代码闭环**

- **核心内容**：华为于HDC 2026发布HarmonyOS 7.0（API26）Developer Beta1版，核心特性包括首创鸿蒙内核应用快启技术、沉浸光感组件升级、3DGS空间渲染与空间重建。开发工具方面，DevEco CLI集成鸿蒙知识库与Skills，AI编程Agent DevEco Code实现需求到代码闭环。代码索引效率提升70%，编译构建效率提升40%，内存占用降低30%。首批支持Mate 80 Pro、Mate X7等设备。
- **落地应用场景**：鸿蒙开发者的全流程AI辅助——从需求描述直接生成可运行代码，大幅降低鸿蒙生态应用开发门槛，加速鸿蒙原生应用生态建设。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/964/029.htm)

##### 🌐 **Adaline 2.0——AI智能体自我改进层**

- **核心内容**：Adaline 2.0推出AI智能体自我改进层，将生产流量和用户反馈痕迹自动转化为行为聚类，进而生成评估（Evals）、合成边缘场景数据，并基于此产出新的智能体候选版本。每天可编写数百条新eval，自动发现人类难以想到的评估用例。开发者只需审核胜出版本即可上线，保留人工最终审批权。
- **落地应用场景**：解决AI产品上线后持续优化的核心痛点——无需人工逐条检查异常对话，自动从真实用户交互中发现问题并迭代改进。适用于客服Agent、编码助手、数据分析Agent等所有需要长期运营的AI产品。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2065839865347092502)

##### 🌐 **SK海力士拟向客户送样HBM4E——AI芯片供应链竞速升级**

- **核心内容**：SK海力士正筹备向主要客户送样第七代HBM产品HBM4E，首批样品最快本月出货。HBM4E计划明年正式量产，预计用于英伟达下一代AI加速器Rubin Ultra。此前三星已于5月29日率先向英伟达交付业界首批12层HBM4E样品。COMPUTEX上黄仁勋参观SK展台并留言"请多生产一些"。同时韩国启动5000亿韩元研发计划攻关下一代功率半导体。
- **落地应用场景**：AI数据中心的算力基础设施——HBM4E将支撑下一代万亿参数模型训练和超长上下文推理，功率半导体则为AI数据中心提供稳定供电、提高能效。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/964/164.htm)

##### 🌐 **SpaceX上市市值超2万亿美元——美股"七大巨头"格局重塑**

- **核心内容**：SpaceX于6月14日上市，市值超2万亿美元，超越特斯拉和Meta。社交平台X上流行新缩写MANGOS（Meta、Anthropic、英伟达、Alphabet、OpenAI、SpaceX），另有"Magna Atoms"等提案。美银5月研报提出"人工智能十大巨头"，在原七家基础上新增博通、美光科技与AMD，这些企业在标普500中权重超40%。
- **落地应用场景**：AI资本格局重塑——SpaceX的星互联网（Starlink）+ 可回收火箭为全球AI基础设施提供低延迟网络和低成本卫星通信，AI与太空经济的融合正在创造新的投资范式。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/964/057.htm)

##### 🌐 **Rio 3.5 Open 397B——巴西市政府模型超越Qwen 3.7达SOTA**

- **核心内容**：里约热内卢市政府IT公司开发的Rio 3.5 Open 397B开源模型达到SOTA，性能超过阿里Qwen 3.7。基于Qwen基础模型，添加了SwiReasoning框架——在标准链式推理与隐空间推理之间动态切换，由基于熵的置信信号引导，使模型仅在必要时"出声思考"，其余时间在隐藏空间内静默推理以提高token效率。已上传HuggingFace。
- **落地应用场景**：政府公共服务的AI赋能——一个市政府后训练的开源模型达到SOTA，标志着AI民主化进入新阶段。隐空间推理框架可显著降低推理成本，适用于资源受限的公共服务部署场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/kimmonismus/status/2065911865390063791)

##### 🌐 **昆仑万维Matrix-Game 3.5——状态与动作联合训练框架**

- **核心内容**：昆仑万维Skywork首席科学家刘扬在智源大会公布Matrix-Game 3.5核心技术：从游戏场景向真实场景扩展，支持多风格动态切换、指令控制及NPC交互。记忆机制采用三维空间块匹配替代历史帧拼接，用PRoPE机制替代额外参数注入。Matrix-Game 3.0已实现5B参数蒸馏模型在720P下40FPS实时生成。构建了包含500万+视频切片、1万+训练小时的数据引擎。3.5计划2026年7月发布。
- **落地应用场景**：游戏开发（AI生成动态游戏世界与NPC交互）、虚拟制片（实时场景生成与风格切换）、机器人仿真训练环境（物理世界模拟）。
- **相关链接**：[🌐 点击查看新闻来源](https://mp.weixin.qq.com/s/y7ijSLj0GbUNe3qtT_Lrzg)

##### 🌐 **微软研究院Mirage——视频生成的持久空间记忆**

- **核心内容**：微软研究院与多所高校联合开发的视频世界模型Mirage，将场景信息直接存储在潜在空间中，而非基于像素的点云。大幅降低计算时间和图形显存消耗，同时能在长镜头移动中保持场景空间一致性——不遗忘"转角后的场景"。目前仍无法可靠地跨片段跟踪运动物体。
- **落地应用场景**：长视频生成、虚拟现实（VR）环境构建、自动驾驶场景模拟——解决视频生成中场景一致性这一核心难题。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/microsoft-researchs-mirage-gives-video-generation-a-persistent-spatial-memory-that-doesnt-forget-whats-around-the-corner)

---

*数据来源：Hugging Face Daily Papers、Arxiv cs.AI/cs.CL 最新提交、AI HOT (aihot.virxact.com)*
*文章由自动化系统生成，如需引用请核实原文链接。*
