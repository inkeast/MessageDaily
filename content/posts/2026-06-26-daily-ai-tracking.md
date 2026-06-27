---
title: "【每日AI前沿追踪】2026年6月26日 核心技术与产业动态速递"
date: 2026-06-26T09:00:00+08:00
draft: false
tags: ["DailyNews"]
categories: ["AI日报"]
---

## 一、 今日核心洞察与重点摘要

- **美国政府要求OpenAI分阶段发布GPT-5.6，前沿模型进入"逐客户审批"时代**：美国商务部、国家网络总监办公室与白宫科技政策办公室联合要求OpenAI以"有限预览"形式发布GPT-5.6，仅向少数合作伙伴开放，且政府对每个客户的访问权限进行逐个审批。此前Anthropic的Mythos 5和Fable 5已遭遇同样管制，经14天谈判无果后CEO Dario Amodei被联合创始人Tom Brown取代。这标志着AI模型发布已从"公开发布"变为类似军民两用战略资产的管控模式，开发速度不受限制但发布节奏被大幅放缓。

- **Agent RL训练稳定性问题集中爆发，多篇论文揭示"为何RL在Agent场景失效"**：今日HF和Arxiv上至少4篇论文聚焦Agent强化学习的核心痛点——CASIA的OPID提出从已完成on-policy轨迹中提取层次化技能监督（episode级+step级）作为RL的密集信号补充；CASIA另一篇论文揭示多步工具调用RL中控制Token概率骤增导致"灾难性崩溃"的机制；UW-Madison发现RL后训练本身已隐含步骤级评分信号（Progress Advantage），无需额外训练奖励模型；Together AI的RiVER证明无需标准答案的评分制RL可提升LLM编码能力。Agent RL正从"能不能用RL"转向"如何让RL稳定且可泛化"。

- **OpenAI Jalapeño芯片确认台积电3nm工艺，全栈垂直整合加速**：OpenAI与Broadcom合作的定制推理ASIC芯片Jalapeño确认采用台积电3nm工艺，由台积电代工，目标年底实现初步部署。第二代ASIC有望导入A16节点（背面供电技术）。同期IBM发布全球首款0.7nm芯片技术（NanoStack架构，指甲盖大小集成千亿晶体管），苹果跳过M6高端版本直入M7 AI优化芯片——AI芯片竞赛从"买GPU"全面进入"自研+制程突破"阶段。

- **企业AI成本危机加剧，开源模型加速蚕食闭源市场**：AI初创公司Lindy完全弃用Claude转投DeepSeek，节省数百万美元；UBS报告称60%关注AI预算的企业正转向更便宜的模型和中国开源模型；埃森哲内部录音曝光PDF转PPT等琐碎任务消耗巨额Token；H100现货价格较5月峰值下跌40%。与此同时GLM-5.2登顶PostTrainBench（34.29%），大量企业寻求基于GLM-5.2进行内部后训练，开源模型正赢得企业信任。

**今日企业+高校研究合作趋势**：今日论文呈现三大产学研协作方向——（1）**Agent RL训练方法论**：CASIA贡献OPID（从on-policy轨迹提取技能蒸馏信号）和工具调用RL崩溃机制分析，UW-Madison贡献Progress Advantage（RL后训练隐含步骤级评分），Together AI贡献RiVER（无标准答案的评分制RL）。合作模式呈现"企业出真实训练痛点+算力+工程平台，高校出理论分析与训练框架设计"的深度协同；（2）**Agent评测基准共建**：Yale贡献GUI vs. CLI匹配执行层基准（440任务），Oxford+多机构贡献GauntletBench（100视觉密集型任务，SOTA仅19.1%），U Tokyo贡献CoffeeBench（多智能体经济90天模拟），产业界Kuaishou贡献AgentX（推荐系统自迭代工业级部署），表明Agent评测正从"单一任务准确率"走向"长时程+多智能体+经济交互"的复杂生态评测；（3）**LLM推理效率优化**：Microsoft+UCSD联合贡献JetSpec（并行树推测解码，H100上MATH-500达9.64x加速），Meta+CMU贡献Discretizing Reward Models（奖励模型过敏感性的离散化修复），Edinburgh贡献InfoKV（信息论感知KV缓存压缩）。产学研合作重心从"联合训练大模型"走向"Agent系统级可靠性+RL训练稳定性+推理效率极限优化"三线并进的深度融合。

---

## 二、 详细内容追踪

### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

#### **OPID: On-Policy Skill Distillation for Agentic Reinforcement Learning**
- **核心亮点**：提出OPID框架，从已完成的on-policy轨迹中直接提取技能监督信号，无需维护外部技能记忆库。将轨迹后见表示为层次化技能——episode级技能捕捉全局工作流或失败规避规则，step级技能捕捉关键时间步的局部决策知识。关键优先路由机制在识别到关键决策时使用step级技能，否则回退到episode级技能。在ALFWorld、WebShop和搜索QA上显著提升Agent性能、样本效率和鲁棒性。
- **团队背景**：中国科学院自动化研究所（CASIA），11位作者（Shuo Yang, Jinyang Wu, Jianhua Tao等），纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26790)

#### **Qwen-Image-Agent: Bridging the Context Gap in Real-World Image Generation**
- **核心亮点**：首次识别出文本到图像生成的"上下文鸿沟"（Context Gap）——用户输入的上下文与T2I模型所需的充分生成上下文之间的不匹配。提出Qwen-Image-Agent统一智能体框架，集成规划、推理、搜索、记忆和反馈五大能力，通过上下文感知规划和上下文接地逐步构建生成上下文。同步发布IA-Bench基准，覆盖规划、推理、搜索、记忆四大核心图像Agent能力，在IA-Bench、Mindbench和WISE-Verified上达到SOTA。
- **团队背景**：阿里通义千问团队（Qwen），21位作者（含Dongyan Zhao、Chenfei Wu等），企业研究团队主导。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26907)

#### **The Verification Horizon: No Silver Bullet for Coding Agent Rewards**
- **核心亮点**：颠覆"验证比生成容易"的经典直觉——随着基础模型推理能力增强和工程脚手架日益成熟，生成复杂候选方案已不再困难，可靠验证反而成为更难的问题。从可扩展性、忠实性和鲁棒性三个维度刻画验证信号质量，论证三者同时满足是核心挑战。深入研究四种奖励构造方式：通用编码任务的测试验证器、前端任务的评分验证器、真实世界Agent任务的用户验证器、长时程任务的自动化Agent验证器。实验表明针对性验证设计可有效抑制奖励黑客行为，但不存在能随策略能力持续增长而保持有效的固定奖励函数。
- **团队背景**：阿里通义千问团队（Qwen），12位作者，企业研究团队出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26300)

#### **JetSpec: Breaking the Scaling Ceiling of Speculative Decoding with Parallel Tree Drafting**
- **核心亮点**：突破推测解码的扩展天花板——现有head-based方法面临因果性-效率困境，自回归起草器产生路径依赖候选但成本随树深度增长，双向块扩散起草器一次生成但分支无关边际可能产生相互不一致的树。JetSpec训练因果并行起草头，在冻结目标模型的融合隐藏状态上操作，产生与目标模型自回归分解对齐的候选树。在H100上MATH-500达9.64x加速，开放对话达4.58x加速，支持Dense和MoE Qwen3模型，已集成vLLM。
- **团队背景**：**产学研合作典型**——UCSD（Tajana Rosing教授、Hao Zhang教授）+ Microsoft（Daxian Jiang、Yibo Zhu），12位作者。高校提供理论分析与算法设计，企业提供工程平台与真实推理负载验证。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.18394)

#### **GUI vs. CLI: Execution Bottlenecks in Screen-Only and Skill-Mediated Computer-Use Agents**
- **核心亮点**：首次在受控环境下隔离比较GUI和CLI两种Agent交互模态——构建440个桌面任务（18个应用、12种工作流类别），确保屏幕GUI Agent和技能中介CLI Agent获得相同目标、初始状态和验证器。最强GUI Agent达59.1%通过率，超过最强原始CLI Agent的48.2%；但验证器引导的技能增强将CLI提升至69.3%。揭示GUI和CLI暴露不同的执行瓶颈：GUI受限于长时程工作流中的可靠接地交互，CLI受限于技能覆盖率和可扩展性。
- **团队背景**：耶鲁大学（Yilun Zhao、Arman Cohan、Chen Zhao等），7位作者，纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.24551)

#### **Why Multi-Step Tool-Use Reinforcement Learning Collapses and How Supervisory Signals Fix It**
- **核心亮点**：揭示Agent RL中的"灾难性崩溃"现象——多步工具调用RL训练中性能骤降且工具调用结构失效。分析发现崩溃源于特定控制Token的意外概率骤增，破坏了结构化执行，但底层工具调用能力并未消失仅被格式遮蔽。系统研究了多种监督信号（off-policy监督、提示引导、错误样本监督等）在同步和交错训练方案下的效果，发现SFT与RL交错训练显著提升稳定性，但在格式和内容OOD评估下性能退化。
- **团队背景**：中国科学院自动化研究所（CASIA），5位作者（Yupu Hao, Zhuoran Jin, Kang Liu, Jun Zhao等），纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26027)

#### **Running the Gauntlet: Re-evaluating Agent Capabilities Beyond Familiar Environments**
- **核心亮点**：引入GauntletBench，一个评估Agent在挑战性场景中泛化能力的Web基准，聚焦三个被忽视的能力维度（时间感知、图形理解、3D推理），覆盖五个少被关注的专业应用（视频编辑器、工作流构建器、3D建模器、航班分析器、电路设计器），每个应用20个视觉密集型任务（共100个）。实验揭示前沿Agent系统远未达到人类水平——最强Agent仅19.1%成功率，而非专家人类标注者达80%以上。
- **团队背景**：牛津大学（Philip Torr、Yarin Gal、Christopher Russell、Christopher Summerfield）+ 多机构（Fazl Barez、Baoyuan Wu等），26位作者，多校联合。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.14397)

#### **Confidence-Aware Tool Orchestration for Robust Video Understanding**
- **核心亮点**：识别视频推理模型的"盲目信任问题"——在运动模糊、眩光、遮挡等真实扰动下，前沿视频推理模型准确率骤降15-30个百分点且不自知。提出Robust-TO框架，将每帧可信度显式集成到推理的每个阶段：统一证据接口组织异构视觉感知工具，可靠性-相关性评分选择可信帧，三级证据综合（高/中/低）+ 置信度-成本GRPO奖励联合优化正确性、证据可靠性和效率。干净输入达56.4%平均准确率（超最强开源基线10.6pp），五种扰动下维持54.3%。
- **团队背景**：南洋理工大学（NTU），3位作者（Yangfan He, Yujin Choi, Jaehong Yoon），纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26904)

#### **CoffeeBench: Benchmarking Long-Horizon LLM Agents in Heterogeneous Multi-Agent Economies**
- **核心亮点**：首个评估LLM Agent在异构多智能体经济中长时程表现的标准——两个农场主、两个烘焙商和两个零售商在90天模拟中自主经营，通过沟通和交易最大化累计净收入。与现有单Agent被动环境基准不同，经济系统本质上是多智能体的，要求Agent在追求自身目标的同时进行沟通、谈判和交易。分析发现高性能模型更积极与其他公司沟通，而Claude Haiku 4.5出现"空闲漂移"失败模式——反复选择不行动。
- **团队背景**：东京大学等日本高校（Issa Sugiura, Takashi Ishida等），8位作者，纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.16613)

#### **Discretizing Reward Models**
- **核心亮点**：揭示奖励模型的核心缺陷——过敏感（对等价好的回答给出不同分数），理论上看似完美的奖励模型也可能高度过敏，实践中导致糟糕策略。提出无需训练的算法：对任意神经奖励模型使用Monte Carlo Dropout产生离散奖励簇，理论上证明存在在最小牺牲判别能力下减少过敏的离散化方案。在受控和自然RL设置中，离散化奖励比原始奖励产生更少奖励黑客行为和更优策略。
- **团队背景**：Meta/FAIR（Tongshuang Wu, Yuning Mao, Graham Neubig等），7位作者。Graham Neubig同时任职于CMU，属产学研合作。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.21795)

#### **Information-Aware KV Cache Compression for Long Reasoning (InfoKV)**
- **核心亮点**：从信息论视角重新审视长推理中的KV缓存压缩——现有方法主要依赖注意力权重估计Token重要性，但忽略了与预测不确定性和Token信息量相关的互补信号。引入"前向影响"（Forward Influence）度量压缩Token对未来上下文的影响，发现高预测不确定性的Token对远期未来上下文影响显著更强。InfoKV结合Token级预测不确定性与层级表示演化，在Llama-3.1、Llama-3.2和DeepSeek-R1上一致超越注意力KV压缩方法。
- **团队背景**：爱丁堡大学（Alexandra Birch等），4位作者，纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26875)

#### **Neglected Free Lunch from Post-training: Progress Advantage for LLM Agents**
- **核心亮点**：发现RL后训练已隐含有效的步骤级评分信号——RL训练策略与参考策略之间的对数概率比值恰好恢复最优优势函数，命名为"Progress Advantage"（进展优势）。该信号无需标注、领域无关、是标准RL后训练的副产品。在测试时扩展、不确定性量化和失败归因三个应用中验证有效性，一致超越基于置信度的基线，且无需任务特定训练即超越专门的训练奖励模型。
- **团队背景**：威斯康星大学麦迪逊分校（UW-Madison，Sharon Li教授），6位作者，纯学术机构出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.26080)

#### **AgentX: Towards Agent-Driven Self-Iteration of Industrial Recommender Systems**
- **核心亮点**：快手提出生产级部署的多智能体系统AgentX，将推荐算法迭代从"工程师手工驱动"重构为"自进化开发引擎"——自主生成、实现、评估并从推荐实验中学习。四阶段闭环：Brainstorm Agent综合历史实验和外部研究生成可执行提案，Developing Agent通过仓库接地生成生产级代码并多维可靠性验证，Evaluation Agent进行安全在线A/B测试，Harness Evolution层（SGPO）将执行轨迹蒸馏为语义梯度更新持续提升Agent自身。
- **团队背景**：快手（Kuaishou），60+位作者（Changxin Lao, Kun Gai等），大型企业研究团队工业级部署。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.26859)

#### **RiVER: Reinforcement Learning without Ground-Truth Solutions can Improve LLMs**
- **核心亮点**：提出RiVER框架，在无标准答案的评分制优化任务上训练LLM——使用确定性执行反馈作为连续值监督。识别组相对RL在连续奖励上的两大挑战：尺度支配（未校准的分数幅度扭曲策略更新）和频率支配（反复采样的次优解淹没稀有强候选）。RiVER通过校准奖励整形和强调顶级排名求解者解决这些问题。在12个AtCoder启发式竞赛任务上训练，Qwen3-8B和GLM-Z1-9B分别提升8.9%和9.4%的ALE评分排名，且无标准答案训练也能迁移到精确解基准。
- **团队背景**：**产学研合作**——Together AI（Yuxiong He, Zhewei Yao, Yi-An Ma）+ 高校研究者，9位作者。企业提供工业级RL训练经验与算力，高校贡献奖励校准理论。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.27369)

#### **When Does Combining Language Models Help? A Co-Failure Ceiling on Routing, Voting, and Mixture-of-Agents Across 67 Frontier Models**
- **核心亮点**：揭示多模型LLM系统的增益上限——对于任何输出为单一模型答案的策略，准确率不能超过1减去所有模型在同一查询上同时错误的概率。在67个模型（21家提供商）上验证：开放数学的观测共失败率为0.052（67模型高斯copula下0.023，低估约2.5倍），执行评分代码为0.079。将GPQA从多选题改为自由回答重新打开共失败尾部（0.127），定位共失败在答案格式而非学科。在可检查任务上，组合模型很少在不具备强查询级路由信号时超越单一最佳模型。
- **团队背景**：Kaikaku（Josef Chen），1位作者，AI初创企业出品。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.27288)

---

### 2. 产业动态与产品创新（AI Hot 精选）

#### **美国政府要求OpenAI分阶段发布GPT-5.6，逐客户审批**
- **核心内容**：美国商务部、国家网络总监办公室与白宫科技政策办公室联合要求OpenAI以"有限预览"形式发布GPT-5.6，仅向少数合作伙伴和企业客户开放，政府对每个客户的访问权限逐个审批。CEO Altman在内部Q&A中透露此消息，称希望数周后扩大发布但承认这不是OpenAI偏好的长期模式。GPT-5.6系列涵盖mini、标准版和Pro版，上下文窗口扩至150万Token。同期Anthropic的Mythos危机持续恶化——经14天谈判无果，联合创始人Tom Brown已取代CEO Dario Amodei牵头与政府协商。
- **落地应用场景**：前沿AI模型发布进入"许可证管制"时代，企业获取SOTA模型需通过政府安全审查，将直接影响AI驱动的代码生成、自动化办公和智能客服等企业级应用的部署节奏。
- **相关链接**：[🌐 点击查看新闻来源](https://www.theverge.com/ai-artificial-intelligence/957372/openai-will-delay-gpt-5-6-after-trump-administration-request)

#### **OpenAI Jalapeño芯片确认台积电3nm工艺**
- **核心内容**：OpenAI与Broadcom合作开发的LLM优化AI推理ASIC芯片Jalapeño确认采用台积电3nm工艺，由台积电负责晶圆代工，目标今年底实现初步部署。双方第二代AI ASIC项目有望导入台积电A16节点，利用背面供电技术提升密度与性能。Jalapeño专为LLM推理负载优化，旨在减少对NVIDIA的单一供应商依赖。
- **落地应用场景**：自研推理芯片可降低30%-50%推理成本，直接影响ChatGPT、Codex等产品的服务成本结构，使AI编程助手和长上下文推理服务更经济可行。
- **相关链接**：[🌐 点击查看新闻来源](https://techcrunch.com/podcast/openais-jalapeno-chip-is-big-techs-spiciest-move-away-from-nvidia)

#### **IBM发布全球首款0.7nm芯片技术（NanoStack）**
- **核心内容**：IBM推出0.7nm芯片技术，采用新型NanoStack架构将晶体管垂直堆叠，取代传统平面缩放。指甲盖大小面积可容纳近1000亿个晶体管，性能较2nm节点提升50%，能效提升70%，SRAM缩小40%。突破原子尺度工程极限，有望让AI芯片、手机、服务器等更快更省电。
- **落地应用场景**：为下一代AI推理芯片提供制程基础，可显著降低数据中心AI推理的功耗和成本，同时为端侧AI（手机、PC）提供更强算力支持。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2070197166908600645)

#### **苹果跳过M6高端芯片，转向AI优化的M7系列**
- **核心内容**：据彭博社报道，苹果计划最早今年为入门级Mac推出基础版M6处理器，但跳过M6 Pro和M6 Max，直接于2027年进入M7代。M6将带来更高内存带宽（约200 GB/s，M5约153 GB/s）、升级神经引擎、改进CPU核心、增强视频编解码及全新GPU（最多12核心）。M7内存带宽或超300 GB/s，主因是加速设备端AI和图形能力。
- **落地应用场景**：设备端AI推理（本地LLM运行、实时图像/视频生成、Apple Intelligence功能）将获得更强硬件支撑，减少对云端API的依赖。
- **相关链接**：[🌐 点击查看新闻来源](https://www.bloomberg.com/news/articles/2026-06-25/apple-to-skip-high-end-m6-mac-chips-to-launch-m7-pro-m7-max-m7-ultra-instead)

#### **AI初创公司Lindy弃用Claude全面改用DeepSeek**
- **核心内容**：AI初创公司Lindy已完全弃用Anthropic的Claude，转而使用DeepSeek的模型（在美国境内托管）。CEO Flo Crivello表示其25人公司的AI成本此前"不可持续"甚至超过人员开支，切换后成本曲线"直接跌到地面"，节省了数百万美元。若DeepSeek出现可靠性问题，Lindy将切换至GLM而非回到Claude。同期UBS报告称60%关注AI预算的企业正转向更便宜的模型和中国开源模型。
- **落地应用场景**：AI智能客服、自动化工作流编排、邮件处理等企业级AI应用可通过模型路由策略将简单任务分配给低成本模型，复杂推理保留给高端模型，实现成本与质量的平衡。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/ai-startup-lindy-ditched-claude-entirely-for-deepseek-saving-millions-as-cost-pressure-mounts-on-anthropic)

#### **OpenAI Codex非开发者用量激增137倍**
- **核心内容**：OpenAI在论文《向智能人工智能的转变：来自Codex的证据》中披露，自2025年8月以来非开发者对Codex的使用量激增：个人用户增长137倍，组织用户增长189倍，内部用户增长12倍。2026年上半年智能体AI活跃用户增长超5倍，增速最快的是非软件开发人员。目前OpenAI内部97.9%员工使用Codex，内部Codex Token占比从不到10%增长到超过50%。约24%请求对应人类需1小时以上工作。
- **落地应用场景**：AI编程助手正从开发者工具扩展为全职能工作平台——市场、运营、财务等非技术岗位使用Codex进行数据分析、文档生成、自动化流程设计，企业知识工作正全面转向长周期Agent任务。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/968/961.htm)

#### **Notion关闭Mail服务，全力投入AI智能体**
- **核心内容**：Notion宣布将于2026年9月22日停止运营Notion Mail（2025年4月上线，生命周期约17个月）。原因是超过半数用户通过AI智能体管理邮件而无需打开收件箱，Notion决定从"AI辅助邮箱客户端"转向"由智能体直接运行邮箱"。用户邮件历史保留在Gmail，但须在9月21日前导出草稿和定时邮件。
- **落地应用场景**：邮件管理从"人工收发"转向"AI智能体代管"——Agent自动筛选、分类、回复邮件，用户只需处理高优先级事项，大幅降低信息过载。
- **相关链接**：[🌐 点击查看新闻来源](https://arstechnica.com/gadgets/2026/06/notion-killing-skiff-influenced-email-app-since-most-users-use-ai-agents-instead)

#### **DeepSeek融资74亿美元并计划全员翻倍**
- **核心内容**：Anthropic的Mythos预览版让DeepSeek感到震惊，CEO梁文锋意识到需要更大现金储备来竞争。DeepSeek随即启动74亿美元融资（此前靠CEO个人财富运营），并计划将所有部门员工数量翻倍，招聘覆盖AI核心研发、算法、深度学习、全栈开发和产品岗位。DeepSeek正从仅调模型转向围绕模型构建整个系统，同步招聘多模态方向工程师和研究员。
- **落地应用场景**：DeepSeek将加速多模态模型和Agent生态建设，其开源模型策略将持续降低企业AI部署成本，在代码生成、多模态理解和Agent推理等场景提供更多选择。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2070521519206482257)

#### **中国发布《人工智能 智能体互联》系列7项国家标准**
- **核心内容**：国家市场监管总局发布《人工智能 智能体互联》系列7项国家标准，覆盖总体架构、身份码、身份管理、智能体描述、发现、交互及工具调用全流程，旨在解决智能体产业通信接口不统一、身份管理缺失、协同规则混乱等"信息孤岛"问题。编制汇聚70余家机构超百位专家，小米、联想等百余家企业参与试点应用，兼容多条技术路线。
- **落地应用场景**：跨平台Agent协作将成为可能——不同厂商的智能体可通过统一身份认证和交互协议直接通信协作，应用于跨企业供应链协同、政务一网通办、多平台智能客服等场景。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/969/096.htm)

#### **Linux基金会联合20家科技巨头推出Akrites项目**
- **核心内容**：Linux Foundation与约20家科技企业、AI实验室和银行共同发起Akrites倡议，旨在AI工具利用漏洞前修补关键开源软件的安全缺陷。创始成员包括AWS、Anthropic、Cisco、Google、Microsoft、NVIDIA、OpenAI等。项目采用统一CVD披露流程，保密优先，漏洞由原维护团队按自身节奏修复；无活跃维护者的项目由最后维护者接手。
- **落地应用场景**：企业AI安全运维——在AI驱动的自动化漏洞挖掘工具大规模部署前，系统性修补开源依赖库中的已知缺陷，防止AI编程助手和自动化Agent被利用进行供应链攻击。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/linux-foundation-and-20-tech-giants-launch-akrites-to-fix-open-source-flaws-before-ai-powered-attacks-hit)

#### **高通进入数据中心市场，专为中国定制AI芯片**
- **核心内容**：高通发布数据中心芯片产品线Dragonfly C1000，正式挑战英伟达。CEO安蒙表示将把全部四条数据中心产品线引入中国，包括专为中国市场定制且符合美国出口管制规定的AI加速器。安蒙称与中国智能手机和汽车公司的合作也将为数据中心业务带来优势。同期高通与Hugging Face扩大合作，构建端到云AI开发生态，HF的AI存储和推理服务将适配高通Dragonfly方案。
- **落地应用场景**：企业AI推理部署获得除NVIDIA之外的芯片选择，中国客户可获得合规定制版AI加速器，降低数据中心AI推理硬件成本和供应链风险。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/969/000.htm)

#### **商汤SenseNova U1完整训练代码开源**
- **核心内容**：商汤开源SenseNova U1完整训练代码，提供可检查、可修改、可重建的完整训练栈。同步发布smoke-test数据集，覆盖t2i、it2i、多图输入、交错生成、多模态理解、视频理解、纯语言续写7种任务类型。用户可基于smoke-test验证模型完整性、调试训练流程、确保修改不引入回归。
- **落地应用场景**：研究者和企业可基于完整训练代码复现、修改和扩展商汤多模态模型，应用于内容创作（文生图/图生图）、视频理解、多模态对话等场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/SenseTime_AI/status/2070516171586273652)

#### **Ornith-1.0开源编码模型族发布**
- **核心内容**：DeepReinforce发布Ornith-1.0开源编码模型家族，提供9B Dense、31B Dense、35B MoE和397B MoE四种尺寸，均以MIT许可开源。基于Gemma 4和Qwen 3.5后训练，采用强化学习联合优化任务脚手架与解决方案，SWE-Bench Verified达82.4%超越Claude Opus 4.7。模型学会自动构建和优化自身的RL脚手架（如测试生成、错误分析），实现"学会学习"的编码Agent能力。
- **落地应用场景**：开源编码Agent可用于自动化代码审查、Bug修复、测试生成和持续集成，企业可本地部署避免代码泄露风险。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/06/25/deepreinforce-releases-ornith-1-0-an-open-source-coding-model-family-that-learns-its-own-rl-scaffolds)

#### **General Intuition 3.2亿美元融资：用游戏数据训练通用AI智能体**
- **核心内容**：General Intuition以23亿美元估值完成3.2亿美元融资，累计融资4.54亿美元。公司从旗下游戏剪辑平台Medal获取数亿小时含精确按键动作标签的游戏操作数据，训练单一模型同时驾驭Fortnite等虚拟环境和四足机器人。演示中AI智能体在游戏中连续运行100+小时，仅用8分钟真实机器人数据微调即可控制四足机器人自主导航。计划夏季末开放API。
- **落地应用场景**：游戏数据训练的时空推理能力可迁移到机器人控制、自动驾驶仿真、工业自动化等物理世界场景，降低具身AI的数据获取成本。
- **相关链接**：[🌐 点击查看新闻来源](https://techcrunch.com/2026/06/25/general-intuitions-2-3b-bet-that-video-games-can-train-ai-agents-for-the-real-world)

#### **OpenAI扩展Codex计算机使用至PowerPoint和Excel**
- **核心内容**：OpenAI正在通过插件增强Codex在PowerPoint和Excel上的计算机使用能力，使Codex能够直接操作桌面办公软件。同期OpenAI已在付费程序中投放广告（Financial Times、Shein、Amazon Prime day），最低档付费计划标注"可能包含广告"。
- **落地应用场景**：AI编程助手可直接操作PPT和Excel完成演示文稿生成、数据分析、报表自动化等办公任务，无需用户手动在多个工具间切换。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/testingcatalog/status/2070521930671661289)

#### **华为与湖北移动完成全国运营商首个AI推理加速方案现网测试**
- **核心内容**：华为与湖北移动基于OceanStor A800存储与昇腾A3超节点架构，搭载UCM（推理记忆数据管理）技术，完成全国运营商首个AI推理加速方案现网测试。针对MiniMax M2.5、GLM-5.1等模型，在8K至190K长序列场景下，Token吞吐率最高提升372%。MiniMax M2.5下首Token延迟显著降低，GLM-5.1在长序列推理中吞吐率大幅提升。
- **落地应用场景**：电信运营商可基于国产算力（昇腾）和存储（OceanStor）提供高性能AI推理服务，应用于智能客服、网络运维自动化、实时内容审核等大规模推理场景。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/968/730.htm)
