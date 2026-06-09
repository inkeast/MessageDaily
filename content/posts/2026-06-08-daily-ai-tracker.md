---
title: "【每日AI前沿追踪】2026年06月08日 核心技术与产业动态速递"
date: 2026-06-08
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "6月8日AI前沿速递：苹果WWDC发布第三代AFM基础模型与全新Siri；OpenAI正式提交S-1文件启动IPO进程；微信AI生态重磅内测——美团携程首批接入；谷歌向英特尔订购超300万颗TPU芯片；小米MiMo突破千tokens/s推理速度；Agent自进化论文密集爆发——OpenSkill/Socratic-SWE/HarnessForge/SIA四篇聚焦技能自我迭代；阿里合并通义成立Token Foundry事业部。"
---

## 【每日AI前沿追踪】2026年06月08日 核心技术与产业动态速递

### 一、 今日核心洞察与重点摘要

- **苹果WWDC 2026：第三代AFM基础模型+全新Siri——端侧AI的里程碑时刻**：苹果在WWDC上发布了第三代Apple Foundation Models（AFM），与Google合作定制，包含五个模型覆盖设备端到服务器端。全新Siri集成到灵动岛，具备屏幕感知和跨App操作能力，但端侧模型仅支持iPhone 17 Pro，欧洲和中国暂不可用。这标志着苹果从"AI追赶者"向"端侧隐私AI引领者"的战略转型，但同时也暴露了其市场覆盖策略的保守性。

- **OpenAI正式提交S-1——AI行业最大IPO进入倒计时**：OpenAI已向SEC机密提交S-1草案（首次公开募股注册声明），上市时间未定。同步宣布的还有"Built to Benefit Everyone"普惠AGI计划以及Economic Research Exchange经济研究平台。ChatGPT正在经历自2022年上线以来最大规模改版——从聊天机器人转向超级应用Agent平台，整合Codex、图像生成和第三方应用（Canva、Booking）。高管内部直言"聊天已死"，9亿周活用户和5000万付费用户将成为Agent化的庞大基数。

- **谷歌向英特尔下单300万颗TPU——AI芯片供应链格局巨变**：谷歌已选择英特尔在2028年前制造超过300万颗TPU芯片，这对英特尔代工业务是历史性胜利。英伟达也在评估英特尔作为备选代工厂。台积电产能瓶颈正推动科技巨头加速供应链多元化。同日，英伟达CEO黄仁勋在首尔密集签约——与SK海力士签署多年HBM4内存协议、与现代汽车深化AI机器人合作、与LG共建AI工厂、与斗山扩展物理AI合作——延续"芯片外交"路线巩固AI基础设施霸权。

- **Agent自进化论文集中爆发——从"让人设计Agent"到"让Agent自己进化"**：今日Hugging Face和Arxiv上出现多篇聚焦Agent自我进化的重磅论文——OpenSkill（Lehigh+Salesforce）实现开放世界技能自动构建、Socratic-SWE在SWE-bench Verified达50.40%、HarnessForge（北航+清华）提出harness与policy协同进化、SIA实现脚手架与权重的联合自我改进。研究方向正从"如何设计更好的Agent"转向"如何让Agent自己进化得更好"。

**产学研合作趋势观察**：今日产学合作呈现三大特征：① **Agent自我进化成为产学联合的最热门方向**——四篇核心论文（OpenSkill/Socratic-SWE/HarnessForge/SIA）分别来自Lehigh+Salesforce、北航+清华等团队，产业界（Salesforce、阿里通义）与高校的深度合作成为标配，表明Agent自进化已从学术概念进入工程化阶段。② **Agent评估体系精细化**——ToolMaze（上海AI Lab+百度+华东师大）、SubtleMemory（哈工大+上海AI Lab+港中文）、ResearchClawBench（上海AI Lab+复旦+交大+清华+浙大等10余所高校）三篇基准论文同时出现，标志着研究重心从"跑分"转向"精准诊断"。③ **中国企业深度参与全球学术对话**——阿里通义×上交×复旦×港中深的Agentic ASR、腾讯混元×上交×南洋理工的MMAE、面壁智能OpenBMB的VoxCPM2等论文均体现中国AI力量的国际化产出。

---

### 二、 详细内容追踪

#### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

##### 📄 **OpenSkill: Open-World Self-Evolution for LLM Agents**
- **核心亮点**：提出面向开放世界的Agent自进化框架，仅从任务提示出发，利用文档、代码仓库和网络等开放世界资源，从零构建自身技能和验证信号。通过三阶段流程（开放世界知识获取→无泄漏技能进化→零样本目标评估），在三个基准上均取得最佳自动化通过率，且技能可跨模型迁移。
- **团队背景**：Lehigh University、University of Illinois Chicago、University of British Columbia + **Salesforce AI Research** + **Harvard Medical School**。企业（Salesforce）与多所高校联合的典型产学研成果。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.06741)

##### 📄 **Socratic-SWE: Self-Evolving Coding Agents via Trace-Derived Agent Skills**
- **核心亮点**：提出闭环自我进化框架，将Agent历史求解轨迹蒸馏为结构化的"智能体技能"，总结常见失败模式和有效修复模式。通过基于执行的验证和求解器梯度对齐奖励进行筛选，在**SWE-bench Verified上达到50.40%**解决率（三轮迭代后），超越所有同计算预算下的自我进化基线。
- **团队背景**：论文作者机构信息待进一步确认，但从论文主题判断涉及Coding Agent领域的深度研究。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.07412)

##### 📄 **HarnessForge: Joint Harness and Policy Evolution for Adaptive Agent Systems**
- **核心亮点**：将Agent系统建模为"外部执行框架（harness）—推理策略（policy）"配对，通过故障引导的框架裁剪和框架条件下的策略对齐实现协同进化。在五个跨领域基准上相比最强基线提升最高**12.0%**，证明框架与策略必须协同进化才能弥合外部执行结构与内部推理器之间的兼容性差距。
- **团队背景**：**北京航空航天大学** + **清华大学**——纯学术团队的合作，代表了国内Agent系统工程化研究的高水平。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.01779)

##### 📄 **SIA: Self Improving AI with Harness & Weight Updates**
- **核心亮点**：弥合了"仅更新脚手架"和"仅更新权重"两个孤立研究方向的鸿沟，Feedback-Agent同时更新Agent的脚手架和模型权重。在三个截然不同的领域（法律分类/GPU内核优化/单细胞RNA去噪）均大幅超越纯脚手架迭代方法，分别获得**+56.6%**准确率提升、**91.9%**运行时间缩减和**+502%**去噪性能提升。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2605.27276)

##### 📄 **ToolMaze: When Tools Fail — Benchmarking Dynamic Replanning and Anomaly Recovery in LLM Agents**
- **核心亮点**：提出首个系统性评估Agent异常恢复能力的基准，采用基于DAG的拓扑复杂度和工具扰动分类体系。实验揭示关键发现：隐式语义故障导致恢复率骤降约37%，容错能力随模型规模增长的速率比基本任务完成能力**慢3.66倍**——动态重新规划是一种独立的、未被规模定律解决的能力。
- **团队背景**：**上海人工智能实验室** + **华东师范大学** + **苏州大学** + **山东大学** + **百度**——百度与多所高校联合的产学研典范。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.05806)

##### 📄 **SubtleMemory: A Benchmark for Fine-Grained Relational Memory Discrimination**
- **核心亮点**：针对长期运行Agent中细粒度关系记忆辨别能力的新型基准，构建具有互补、微妙差异和矛盾关系的受控语义变体。包含1,522个评估实例，覆盖10个长期历史。评估揭示当前系统在细粒度关系记忆辨别方面**严重不足**，尤其在矛盾记忆处理上存在显著困难。
- **团队背景**：**哈尔滨工业大学** + **上海人工智能实验室** + **同济大学** + **厦门大学** + **复旦大学** + **香港中文大学** + **上海交通大学**——七校联合的跨机构大合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.05761)

##### 📄 **ResearchClawBench: End-to-End Autonomous Scientific Research**
- **核心亮点**：覆盖**10个科学领域40项真实论文驱动研究任务**的端到端科研Agent基准。当前最强系统（Claude Code 21.5分/100满分，50分代表达到目标论文水平）仍远未实现可靠科学再发现，错误集中在实验方案不匹配、证据不匹配和科学核心缺失。
- **团队背景**：**上海人工智能实验室**牵头，联合**复旦、上交、清华、浙大、港中文、港科技、港理工**等十余所顶尖高校——超大规模产学研联合体。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.07591)

##### 📄 **Thinking with Imagination (Astra): Agentic Visual Spatial Reasoning with World Simulators**
- **核心亮点**：提出Astra框架，让VLM在推理过程中主动与世界模拟器交互获取"想象的"新视角视觉证据。世界模拟器Astra-WM将Gemini-3-Flash从45.1提升至49.5，Agent策略Astra-VL将Qwen3-VL从29.8提升至38.8——证明"想象力"不仅是访问生成器，更是一种需要学习到的交互策略。
- **团队背景**：**香港大学** + **上海人工智能实验室** + **上海交通大学** + **复旦大学** + **北京航空航天大学**——多校联合的跨机构合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.06476)

##### 📄 **LayerRoute: Input-Conditioned Adaptive Layer Skipping for Agentic Language Models**
- **核心亮点**：根据输入类型（结构化工具调用 vs. 开放式规划推理）自适应跳过Transformer层，仅用**1.10M可训练参数**（占骨干网络0.22%）在6.4分钟训练后，实现工具调用步骤跳过**15.25%** FLOPs而困惑度反而改善。为Agent场景下的推理效率优化提供了极轻量级解决方案。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.01838)

##### 📄 **Agentic ASR: Human-Like Interactive Speech Recognition**
- **核心亮点**：首次将语音识别定义为多轮迭代精炼的Agent过程，提出Agentic ASR闭环系统（ASR前端+语义纠正+意图路由+推理编辑）和S²ER语义评估指标。在多语言、命名实体密集和语码转换基准上，迭代交互持续降低语义错误。
- **团队背景**：**阿里巴巴通义团队** + **上海交通大学X-LANCE Lab** + **西安交通大学** + **复旦大学** + **香港中文大学（深圳）**——阿里通义与四所高校的深度产学研合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2605.29430)

##### 📄 **Agentopia: Long-Term Life Simulation and Learning in Agent Societies**
- **核心亮点**：100个智能体在10个模拟年中自主追求个人成长、发展社会关系的框架。引入"life reward"反映人类幸福感，经训练的LLM在模拟中提升Agent幸福感，并在下游角色扮演基准上取得**+15.6%**改进。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.07513)

##### 📄 **OPDLM: Data-Efficient Autoregressive-to-Diffusion Language Models**
- **核心亮点**：提出在线策略蒸馏方法，将自回归语言模型高效转化为扩散语言模型，所需训练token数减少**15倍至7000倍**。将DLM转化定位为ARLM后训练方式，避免了从头预训练DLM的巨大成本。
- **团队背景**：**Texas A&M University DIVE Lab**
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.06712)

##### 📄 **Augmenting Attention with Exponentially Decaying Memory**
- **核心亮点**：将指数衰减记忆模块（源自RAT+架构）集成到Quest、MoBA和SnapKV等稀疏注意力方法中，在八个needle-in-a-haystack任务上一致性提升准确率——SnapKV在1/4和1/8预算下分别平均提升**34.11和40.03个百分点**。为长上下文推理的KV缓存优化提供了通用增强方案。
- **团队背景**：**EPFL（瑞士洛桑联邦理工学院）CLAIRE实验室**
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2605.28640)

---

#### 2. 产业动态与产品创新（AI Hot 精选）

##### 🌐 **苹果WWDC 2026：第三代AFM基础模型+全新Siri**
- **核心内容**：苹果发布第三代Apple Foundation Models（AFM），与Google合作定制，包含五个模型覆盖设备端到Private Cloud Compute服务器端。全新Siri集成到灵动岛，支持屏幕感知和跨App操作，但端侧模型仅支持iPhone 17 Pro。同时发布iOS 27、macOS 27等系统更新。
- **落地应用场景**：端侧隐私AI——在不牺牲用户隐私的前提下实现个性化智能助手体验，适用于短信智能回复、照片编辑、跨应用任务自动化等场景。
- **相关链接**：[🌐 点击查看新闻来源](https://machinelearning.apple.com/research/introducing-third-generation-of-apple-foundation-models)

##### 🌐 **OpenAI正式提交S-1文件，启动IPO进程**
- **核心内容**：OpenAI已向SEC机密提交S-1草案，上市时间未定。同步发布"Built to Benefit Everyone"普惠AGI计划和经济研究交流平台。ChatGPT正经历最大规模改版，从聊天机器人转向超级应用Agent平台。
- **落地应用场景**：AI行业最大IPO将重塑整个科技投资格局，Agent平台化转型将使ChatGPT从对话工具升级为个人AI助手入口，整合外卖、旅游、办公等全场景。
- **相关链接**：[🌐 点击查看新闻来源](https://openai.com/index/openai-submits-confidential-s-1)

##### 🌐 **微信AI生态重磅内测：两种接入模式，美团携程首批接入**
- **核心内容**：微信开放平台发布开发者接入指引，确认微信AI正在内测。提供两种接入模式：自动模式（授权平台读取小程序源码，无需额外开发）和开发模式（开发者自主开发技能，审核后调用）。美团、携程作为首批内测团队已完成初步适配，京东传闻也已接入。微信AI嵌入主界面右滑唤出对话窗口，通过自然语言调用小程序。
- **落地应用场景**：用户可在微信内通过自然语言直接点外卖、订酒店、查机票，微信正在成为Agent时代的超级入口操作系统。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/961/480.htm)

##### 🌐 **谷歌向英特尔订购超300万颗TPU芯片，2028年交付**
- **核心内容**：Alphabet旗下Google已选择Intel在2028年前制造超过300万颗TPU芯片。Intel代工的目标是成为这些芯片的第二来源，缓解台积电产能瓶颈。英伟达也在评估Intel的制造技术。受消息刺激，英特尔股价盘前一度上涨13%。
- **落地应用场景**：AI芯片供应链多元化将降低全球科技产业对台积电的单点依赖，为AI基础设施的长期可持续扩展提供保障。
- **相关链接**：[🌐 点击查看新闻来源](https://www.bloomberg.com/news/articles/2026-06-08/google-tapped-intel-for-over-3-million-chips-information-says)

##### 🌐 **小米MiMo-V2.5-Pro-UltraSpeed：万亿参数MoE模型破千tokens/s**
- **核心内容**：小米MiMo联合TileRT发布MiMo-V2.5-Pro-UltraSpeed，首次在1万亿参数MoE模型上实现超过1,000 tokens/s输出速度，仅用单台标准8-GPGPU节点（非Cerebras/Groq方案）。提供限时免费体验（6月8日-23日），UltraSpeed API价格为标准版3倍。
- **落地应用场景**：超高速推理使万亿参数模型的实时对话成为可能，适用于需要即时响应的企业级AI应用场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/XiaomiMiMo/status/2063993790587904362)

##### 🌐 **Kimi Work桌面AI智能体：支持300智能体并行**
- **核心内容**：月之暗面发布Kimi Work本地桌面AI智能体，支持最多300个AI智能体同时在本地机器并行运行。配合WebBridge浏览器扩展实现网页自动操作，内置全球市场数据工具，具备记忆系统。支持macOS（Apple Silicon）和Windows。
- **落地应用场景**：企业级AI工作流自动化——批量数据处理、市场调研、竞品分析等需要多Agent并行协作的复杂任务场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/Kimi_Moonshot/status/2063990409903112344)

##### 🌐 **Kimi Code焕新升级：开源Coding Agent大版本更新**
- **核心内容**：Kimi Code开源Coding Agent迎来大版本升级：毫秒级启动、新增视频理解能力（提取风格生成LUT、长视频切片、录屏生成代码）、集成同花顺/天眼查等数据源、支持ACP协议在JetBrains/Zed中使用、丰富hook生态。底层视觉推理由Kimi K2.6提供。
- **落地应用场景**：开发者效率工具——从录屏自动生成代码、视频内容到代码的转换、金融数据查询等开发场景。
- **相关链接**：[🌐 点击查看新闻来源](https://mp.weixin.qq.com/s?__biz=MzkzMTY4NTIyNA%3D%3D&mid=2247484250&idx=1&sn=d0a07f5358250f3a54df8fbabe61f09a)

##### 🌐 **阿里合并通义成立Token Foundry事业部**
- **核心内容**：阿里巴巴合并通义大模型事业部和未来生活实验室，成立Token Foundry事业部，由集团CEO吴泳铭直接负责。周靖人任首席科学家并牵头成立AI未来研究院。Qwen3.7-Plus同步发布限时八折优惠。
- **落地应用场景**：组织整合标志着阿里AI战略进入"模型即产品"阶段，从基础模型研发转向全栈AI产品化。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/961/538.htm)

##### 🌐 **阿里云发布AgentScope Java 2.0**
- **核心内容**：面向企业级AI智能体开发的新版本。核心特性：分布式无状态架构支持K8s弹性扩缩容、多租户隔离通过Workspace抽象实现安全数据分离、HarnessAgent负责上下文管理与容错、细粒度权限控制和Human-in-the-Loop支持。
- **落地应用场景**：企业级Java生产环境中的Agent系统部署——金融风控、客服自动化、企业知识库等需要高可用和安全隔离的场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/alibaba_cloud/status/2063923881857945815)

##### 🌐 **高德发布全球首个3D原生城市世界模型ABot-Earth0.5**
- **核心内容**：阿里巴巴旗下高德发布全球首个3D原生城市世界模型，已建成覆盖190多个国家和地区的3D地图。用户输入卫星图或文字描述，10分钟即可在消费级GPU上生成公里级3D城市，输出可编辑3DGS格式。制图成本为传统百分之一，效率提升约千倍。
- **落地应用场景**：具身智能训练环境、低空经济航路规划、应急救援模拟、城市规划与仿真、元宇宙场景构建。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/961/378.htm)

##### 🌐 **OpenRouter成本削减月+Advisor新功能**
- **核心内容**：OpenRouter宣布进入成本削减月，每周至少推出一次主要功能帮助降低推理成本。同时发布Advisor功能——让较小模型咨询更高智能的"顾问"模型，帮助逃出困境循环并迁移到更便宜的模型。
- **落地应用场景**：AI应用开发者的成本优化——在保持服务质量的前提下通过模型路由和层级咨询降低API调用成本。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/OpenRouter/status/2064011848823816419)

##### 🌐 **Runway Aleph 2.0编辑模型：一键适配任意视频格式**
- **核心内容**：Runway发布Aleph 2.0编辑模型，上传现有视频后选择目标宽高比，模型自动填充场景其余部分，实现一个视频适配多个信息流和格式。
- **落地应用场景**：社交媒体内容创作——一条视频自动适配TikTok竖屏、YouTube横屏、Instagram方形等多种格式。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/runwayml/status/2064012425884569627)

##### 🌐 **面壁智能发布VoxCPM2语音生成模型**
- **核心内容**：面壁智能OpenBMB发布2B参数语音生成模型VoxCPM2，基于超200万小时多语言语音数据训练，支持30种语言和9种中文方言。具备自然语言语音设计、可控及高保真延续性语音克隆能力。
- **落地应用场景**：多语言内容本地化、有声读物生成、虚拟人语音、播客自动配音。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/OpenBMB/status/2063991963133903317)

##### 🌐 **微软AI推出MAI-Transcribe-1.5语音转文本模型**
- **核心内容**：微软AI发布MAI-Transcribe-1.5，支持43种语言，新增关键词偏置功能优化领域术语。在Artificial Analysis排行榜上WER为2.4%，FLEURS基准最佳准确率。转录一小时音频不到15秒。
- **落地应用场景**：企业会议记录、医疗/法律等垂直领域转录、多语言客服记录自动化。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/06/08/microsoft-ai-introduces-mai-transcribe-1-5-2-4-wer-on-artificial-analysis-best-in-class-fleurs-accuracy-and-up-to-5x-faster-long-audio-transcription)

##### 🌐 **Hugging Face OpenEnv：标准化Agent强化学习环境**
- **核心内容**：Hugging Face宣布OpenEnv项目进一步开放，由Meta-PyTorch、Reflection、Unsloth等组成的委员会协调，获得PyTorch Foundation、vLLM、SkyRL（UCB）等机构支持。定位为训练器与环境间的互操作层，标准化环境的发布、部署和消费。
- **落地应用场景**：Agent训练基础设施——为终端、浏览器等Agent执行环境提供统一标准，降低训练环境搭建成本。
- **相关链接**：[🌐 点击查看新闻来源](https://huggingface.co/blog/openenv-agentic-rl)

##### 🌐 **Cognition推出FrontierCode代码评估基准**
- **核心内容**：Cognition（Devin母公司）发布FrontierCode，含150个任务（来自36个开源仓库），按难度分Extended/Main/Diamond三层。Diamond子集最高分：Claude Opus 4.8达13.4%，GPT-5.5为6.3%，Gemini 3.1 Pro 4.7%。
- **落地应用场景**：企业级代码质量评估——从"代码能否运行"提升到"代码是否可合并到生产仓库"的更高标准。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/shao__meng/status/2064150967680127316)

##### 🌐 **月之暗面寻求300亿美元估值融资**
- **核心内容**：Kimi母公司月之暗面正寻求新一轮融资，最高募资20亿美元，目标估值300亿美元。这是该公司六个月内第三次融资，若达成估值较去年12月的40亿美元暴涨七倍。
- **落地应用场景**：中国AI赛道估值狂飙的标志性事件——Kimi Work+Kimi Code的产品矩阵正从消费者工具向企业级AI平台转型。
- **相关链接**：[🌐 点击查看新闻来源](https://www.bloomberg.com/news/articles/2026-06-08/china-s-moonshot-ai-seeks-30-billion-value-in-new-funding-talks)

##### 🌐 **英伟达首尔签约潮：SK海力士/现代/LG/斗山/Naver**
- **核心内容**：黄仁勋在首尔密集展开合作——①与SK海力士签署多年HBM4内存协议为Vera Rubin供货；②与现代深化AI机器人和物理AI合作；③与LG共建AI工厂覆盖机器人/自动驾驶；④与斗山扩展物理AI到建筑和制造业；⑤Naver将使用英伟达AI模型巩固韩国市场。三星HBM4获英伟达认证投入量产。
- **落地应用场景**：AI基础设施全球化布局——从数据中心到工厂到机器人全链条的芯片+生态锁定。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/961/225.htm)

##### 🌐 **Mac-1模型：6.6B本地运行，487个MacOS原生工具**
- **核心内容**：CJ Zafir团队发布Mac-1模型（6.6B参数），仅需7GB内存可在任何Mac本地运行，支持487个MacOS原生工具的多工具链式调用。输出速度约65 tok/s，直接挑战云端SaaS Agent。
- **落地应用场景**：本地隐私优先的桌面自动化——文件管理、应用操作、系统配置等MacOS场景的全本地AI助手。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/berryxia/status/2063781016230297676)

##### 🌐 **Hivemind发布AI编程智能体持续学习功能**
- **核心内容**：Hivemind发布面向AI编程智能体的持续学习功能，收集团队运行的每个智能体（Claude Code、Codex、Cursor、Hermes、Pi）的轨迹，转化为可复用技能并推送到所有智能体。内置SkillOpt使Claude Code准确率提升+19.1分，Codex提升+24.8分。
- **落地应用场景**：开发团队的知识积累——将个人编码经验自动转化为团队共享的AI技能库。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/kimmonismus/status/2064001045391462907)
