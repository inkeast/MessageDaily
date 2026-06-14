---
title: "【每日AI前沿追踪】2026年06月13日 核心技术与产业动态速递"
date: 2026-06-13
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "6月13日AI前沿速递：亚马逊CEO与美官员会谈触发Anthropic Fable 5/Mythos 5全球封禁，Karpathy等非美籍用户被限制访问；Meta开始撤销20亿美元收购Manus交易；智谱GLM-5.2全量开放支持1M上下文并下周开源，ZCode 3.0搭载自研Agent内核；EvoArena以118票登顶HF日榜揭示Agent动态环境准确率仅39.6%；清华KEG×智谱AI EurekAgent以11美元发现26圆填充最优解；浙大×清华×MSRA WeaveBench揭示混合接口Agent能力严重不足。"
---

## 【每日AI前沿追踪】2026年06月13日 核心技术与产业动态速递

### 一、 今日核心洞察与重点摘要

- **亚马逊CEO直接触发Anthropic模型全球封禁——AI出口管制从"政府决策"变成"企业游说"**：据《华尔街日报》报道，亚马逊CEO与美国政府官员的会谈直接导致了对Anthropic Fable 5和Mythos 5模型的封禁行动。与此同时，Andrej Karpathy等非美籍顶尖研究者被限制访问顶级模型。美国以国家安全为由要求Anthropic切断所有外国国民（含Anthropic外籍员工）的模型访问，Anthropic当日立即禁用。此前Fable 5被曝"隐形降级"、被某公司越狱、政府试图叫停发布未果等一系列事件已充分发酵。**这是首次由科技公司CEO直接游说政府导致竞争对手模型被禁的案例，标志着AI地缘博弈中"企业-政府-竞争对手"的三角博弈格局成型。**

- **智谱GLM-5.2全量开放+ZCode 3.0——国产最强Agent编码基础设施成型**：在Fable 5被封禁的背景下，智谱宣布GLM-5.2全量开放（支持真实可用1M上下文窗口，下周开源），并发布ZCode 3.0编程工具——全面切换自研ZCode Agent内核，深度适配GLM-5.2，优化长程推理、工具调用及大型工程执行链路。GLM-5.2在长程任务独立完成上保持领先，继续作为最强国产编程模型主引擎。Fable 5封禁客观上为开源模型创造了巨大公关窗口。

- **EvoArena以118票登顶HF日榜——"静态高分≠动态可靠"的Agent评估革命**：EvoArena提出首个面向动态环境中LLM Agent记忆演化的基准，揭示当前最强Agent在动态环境中准确率仅39.6%。与此同时，WeaveBench（浙大×清华×MSRA，94票）揭示了混合接口（CLI+GUI+API）Agent在真实计算机使用场景中的严重能力不足，InterleaveThinker（74票）提出首个多Agent交错生成流水线，FORT-Searcher（71票，人大）提出捷径抗性框架训练深度搜索Agent。**四篇高票论文共同指向一个结论：当前Agent评估体系正在从"单点能力测试"转向"长期可靠性+环境适应性"的系统级评估。**

- **Meta开始撤销20亿美元收购Manus——AI并购的地缘政治刹车**：Meta已开始撤销对Manuis的20亿美元收购交易，此前北京方面要求该交易必须反转。同时，Meta内部AI部门（6500人）面临严重士气危机，被工程师称为"摧残灵魂的集中营"，扎克伯格承认组织调整过快。Meta年度资本支出上调至1250-1450亿美元，但AI转型成效存疑。

**产学研合作趋势观察**：今日产学合作呈现两大突出特征：① **"Agent基础设施"成为产学研合作的主战场**——WeaveBench（浙大×清华×微软亚洲研究院）聚焦混合接口Agent的长期评估，EurekAgent（清华KEG×智谱AI）将Agent环境工程应用于自主科学发现，HarnessBridge（UCLA×Jason Cong）提出可学习的双向harness控制器。研究方向从"让Agent更聪明"转向"让Agent更可靠、更持久、更可维护"。② **合作模式深度绑定、论文产出周期缩短**——智谱AI与清华KEG的合作已形成"论文→产品→开源"闭环（EurekAgent → GLM-5.2 → ZCode 3.0），微软亚洲研究院与浙大的联合（共同第一作者Wanli Li同时隶属于浙大和MSRA）代表了"企业研究员入驻高校课题组"的新型合作范式。

---

### 二、 详细内容追踪

#### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

##### 📄 **EvoArena: Tracking Memory Evolution for Robust LLM Agents in Dynamic Environments**（🔥118票，当日最高）
- **核心亮点**：提出首个面向**动态环境中LLM Agent记忆演化**的基准测试套件，将环境变化建模为终端、软件和社会偏好领域的渐进式更新。同时提出EvoMem——一种基于补丁的记忆范式，将记忆演化记录为结构化的更新历史。实验揭示当前Agent在动态环境中表现极差（平均准确率仅**39.6%**），EvoMem可将标准基准GAIA和LoCoMo分别提升6.1%和4.8%。**这项研究首次系统性地揭示了"静态评估高分的Agent在动态环境中可能完全失效"的严峻现实。**
- **团队背景**：**产学合作**——新加坡国立大学×新加坡管理大学×华盛顿大学×伦敦大学学院×宾夕法尼亚大学×南洋理工大学×MIT×Salesforce AI Research（Caiming Xiong），14人跨6国9校联合攻关。通讯作者Zhiyuan Hu同时隶属于NUS和MIT。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13681)

##### 📄 **WeaveBench: A Long-Horizon, Real-World Benchmark for Computer-Use Agents with Hybrid Interfaces**（🔥94票）
- **核心亮点**：提出首个面向**混合接口（CLI+GUI+API）Agent**的长期真实世界基准。当前Agent评估多聚焦单一接口，但真实计算环境中用户往往需要跨CLI、GUI和API完成长周期任务。WeaveBench揭示当前最强的混合接口Agent在真实场景中成功率远低于预期，暴露了模型在接口切换、状态追踪和错误恢复方面的系统性缺陷。
- **团队背景**：**产学合作**——浙江大学（Wanli Li）×清华大学（Bowen Zhou等）×微软亚洲研究院（Yifan Yang、Dongsheng Li、Caihua Shan），第一作者Wanli Li同时隶属于浙大和MSRA，通讯作者Caihua Shan来自MSRA。**典型的"企业研究员入驻高校"合作范式。**
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.09426)

##### 📄 **SpatialClaw: Rethinking Action Interface for Agentic Spatial Reasoning**（🔥80票）
- **核心亮点**：提出**无需训练**的空间推理Agent框架。核心创新在于采用**代码作为动作接口**——维护一个预加载输入帧和感知/几何原语的有状态Python内核，让VLM驱动的Agent逐步编写可执行cell，基于中间结果（文本和视觉）灵活组合操作。在20个空间推理基准上平均准确率达**59.9%**，超过近期空间Agent **11.2个百分点**，且在6个VLM骨干上一致有效，无需任何基准或模型特定适配。
- **团队背景**：11位作者横跨多个机构（页面未列出完整归属），涉及韩国高校和NVIDIA系研究者（Abhishek Badki已知来自NVIDIA）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13673)

##### 📄 **InterleaveThinker: Reinforcing Agentic Interleaved Generation**（🔥74票）
- **核心亮点**：提出**首个多Agent流水线**，为任意现有图像生成器赋予**交错生成能力**（文本-图像序列交替生成）。架构包含Planner Agent（组织图文输入序列，指导图像生成器逐步执行）和Critic Agent（评估输出、识别偏离计划的样本、优化指令重新生成）。通过GRPO强化学习增强逐步指令纠错能力。在交错生成基准上性能可比**Nano Banana和GPT-5**，意外地在推理类基准（WISE、RISE）上也显著提升基础模型。
- **团队背景**：**产学合作**——第一作者Dian Zheng与Hongsheng Li来自**港中文MMLab**（Multi-media Laboratory），Kaituo Feng等来自字节跳动系研究团队。代表了港高校×国产科技企业的编码合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13679)

##### 📄 **FORT-Searcher: Synthesizing Shortcut-Resistant Search Tasks for Training Deep Search Agents**（🔥71票）
- **核心亮点**：针对深度搜索Agent训练数据中的"捷径坍缩"问题（题目看起来难，但Agent通过便宜路径直接命中答案），提出**捷径感知难度框架**，识别四类捷径风险：证据共覆盖、单线索选择性、暴露常量、先验知识绑定。基于此框架合成捷径抗性训练数据，仅用SFT训练的FORT-Searcher在同等规模开源搜索Agent中取得最优整体性能。**论文系统性解决了"合成训练数据看起来难但实际不够难"的核心痛点。**
- **团队背景**：**高校主导**——中国人民大学AI Box研究组（Wayne Xin Zhao、Ji-Rong Wen），代码开源在RUCAIBox。代表了国内信息检索顶级团队在深度搜索Agent方向的系统化布局。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.12087)

##### 📄 **HYDRA-X: Native Unified Multimodal Models with Holistic Visual Tokenizers**（🔥25票）
- **核心亮点**：提出**首个统一图像和视频tokenization的单一ViT架构**。两大关键发现：①帧级因果时序注意力足够实现视觉重建，全时空注意力反而降低性能；②分层时序压缩大幅优于单步替代方案。在此基础上提出轻量级解压器，在联合图像-视频教师监督下上采样时序压缩特征。7B稠密模型实例化后在图像和视频的理解与生成任务上均取得强性能。**为统一tokenizer UMM铺平了道路。**
- **团队背景**：**产学合作**——14位作者，南京大学（Limin Wang，知名CV研究者）×字节跳动系研究者（Liefeng Bo、Zhao Zhong等已知来自工业界实验室）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13289)

##### 📄 **Demystifying Hidden-State Recurrence: Switchable Latent Reasoning with On-Policy RL**（🔥18票）
- **核心亮点**：提出**SWITCH框架**——通过一对显式边界token（`<swi>`和`</swi>`）实现隐式推理的可切换控制。关键创新在于让隐式推理块兼容标准on-policy强化学习（GRPO策略比率在每一步定义良好），同时暴露隐式步骤供机制分析。训练采用"可见到隐式"的课程学习。机制分析揭示三个发现：①`<swi>`是精确定位的学习切换策略而非风格伪影；②它开启的隐式步骤执行问题特定的因果重要计算；③该计算集中在入口处的单次隐藏状态转换上。
- **团队背景**：9位作者，机构信息需查阅PDF确认，涉及英国和中国高校研究者。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13106)

##### 📄 **HarnessBridge: Learnable Bidirectional Controller for LLM Agent Harness**（🔥9票）
- **核心亮点**：提出**首个可学习的harness控制器**，将Agent-环境接口参数化为双向投影。核心创新在于让harness从"人工设计"变为"端到端学习"——Observation Projection将原始轨迹蒸馏为紧凑的决策相关状态，Action Projection将提议动作转换为可执行转换或基于轨迹的拒绝。在Terminal-Bench 2.0和SWE-bench Verified上匹配或超过强专门化harness，同时大幅减少token使用和轨迹长度，且能从小生成器泛化到更大商用模型。**代表了Agent基础设施从"手工程序"到"学习模块"的范式转变。**
- **团队背景**：**产学合作**——UCLA（Yizhou Sun、Wei Wang、Jason Cong等），Jason Cong为UCLA计算机科学知名教授。代表了系统优化与AI Agent交叉领域的产学探索。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.12882)

##### 📄 **EurekAgent: Agent Environment Engineering is All You Need For Autonomous Scientific Discovery**（🔥21票）
- **核心亮点**：提出"**Agent环境工程**"框架——在动态环境中，Agent的环境设计比Agent自身的推理能力更为关键。以经典数学难题"26圆填充"为例，EurekAgent以仅**11美元**的计算成本发现了已知最优解，证明了精心设计的环境可使Agent实现高效的自主科学发现。**将Agent研究的焦点从"让Agent更聪明"转向"让环境更适合Agent工作"。**
- **团队背景**：**产学合作**——清华大学计算机科学与技术系（6位作者）× 智谱AI/Zhipu AI（2位作者），第一作者来自清华KEG实验室。**典型的清华KEG×智谱AI产学闭环：论文（EurekAgent）→ 产品（GLM-5.2/ZCode 3.0）→ 开源生态。**
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13662)

##### 📄 **Agents-K1: Towards Agent-native Knowledge Orchestration**（Arxiv 精选）
- **核心亮点**：提出端到端的**知识编排流水线**，将原始文档转换为"Agent原生的科学知识图谱"。集成三个核心组件：①多模态解析器（五模块模式捕获实体、多模态证据、引用和有类型实体间关系）；②4B信息提取骨干模型（使用GRPO基于规则奖励训练）；③graphanything CLI（三源Agent接口统一网络搜索、多模态图谱检索和跨文档遍历）。处理**246万篇科学论文**生成Scholar-KG知识图谱，已发布100万篇子集。**将Agent研究从"任务执行"提升到"科学知识基础设施构建"。**
- **团队背景**：**产学合作**——25位大型团队，上海人工智能实验室（Lei Bai、Peng Ye）×华东师范大学（Liang He）等多家机构联合，代表了Agent×科学发现方向的最大规模产学合作之一。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13669)

##### 📄 **Getting Better at Working With You: Compiling User Corrections into Runtime Enforcement for Coding Agents**（🔥2票，代码生成方向）
- **核心亮点**：针对LLM编码Agent"记住纠正但下次仍违反"的问题，提出**TRACE**——将用户聊天中的纠正挖掘→重写为原子规则→编译为运行时强制检查。与Mem0记忆对比，TRACE在OOD任务上将偏好违规率从100%降至**2.0%**，在分布内任务上从100%降至37.6%。**证明了"编译为运行时规则"远比"存入记忆"更可靠地确保偏好合规。**
- **团队背景**：**产学合作**——University of Notre Dame（Yujun Zhou、Nitesh V. Chawla、Xiangliang Zhang等）× **IBM Research**（Pin-Yu Chen、Tian Gao），是编码Agent安全合规方向的经典产学合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2606.13174)

---

#### 2. 产业动态与产品创新（AI Hot Skill 精选）

##### 🌐 **亚马逊CEO直接触发Anthropic模型全球封禁**
- **核心内容**：据《华尔街日报》报道，亚马逊CEO与美国政府官员的会谈直接导致了对Anthropic旗下AI模型的整治行动。美国以国家安全为由要求Anthropic暂停所有外国国民（包括外籍员工）对Fable 5和Mythos 5的访问。Andrej Karpathy（斯洛伐克出生、加拿大长大、持美国绿卡但非公民）等顶尖研究者被限制访问。此前Fable 5因"隐形降级"引发争议、被越狱、政府试图叫停发布未果，此次出口管制是事态升级的最终结果。
- **落地应用场景**：**AI出口管制与企业竞争博弈**——此事将深远影响跨国AI团队协作（外籍员工无法使用公司顶级模型）、AI研究的国际化进程，以及云计算巨头与AI模型公司的竞合关系。
- **相关链接**：[🌐 点击查看新闻来源](https://www.wsj.com/tech/ai/amazon-ceos-talks-with-u-s-officials-triggered-crackdown-on-anthropic-models-dcc90578)

##### 🌐 **Meta开始撤销20亿美元收购Manus交易**
- **核心内容**：Meta已开始撤销对Manus的20亿美元收购交易，此前北京方面要求该交易必须反转。收购解除程序已启动。同时，Meta成立仅数月的AI部门（6500人）被内部工程师称为"摧残灵魂的集中营"，员工濒临集体反抗。扎克伯格承认组织调整过快，10%员工被裁、7000人转岗后可能需将部分员工调回。
- **落地应用场景**：**AI跨国并购的地缘政治风险**——此次撤销表明，AI领域的跨国并购已不仅是商业决策，更受制于中美地缘博弈。Meta年度资本支出上调至1250-1450亿美元但AI转型成效存疑，折射出"大厂All in AI"的组织管理困境。
- **相关链接**：[🌐 点击查看新闻来源](https://techcrunch.com/2026/06/13/meta-reportedly-moves-to-unwind-2b-manus-deal-after-beijings-demand)

##### 🌐 **智谱GLM-5.2全量开放+ZCode 3.0发布**
- **核心内容**：智谱宣布GLM-5.2全量开放，支持**真实可用1M上下文窗口**，下周完全开源。GLM-5.2在长程任务独立完成上保持领先，可为复杂智能体应用提供基础支持，继续作为最强国产编程模型主引擎。同日发布ZCode 3.0编程工具——全面切换自研ZCode Agent内核，深度适配GLM-5.2，优化长程推理、工具调用及大型工程执行链路，后续版本不再维护第三方Agent。Fable 5被封禁客观上成为开源模型和公司的最大公关助推。
- **落地应用场景**：**国产Agent编码基础设施**——开发者可用GLM-5.2+ZCode 3.0构建端到端的编程智能体，覆盖代码生成、长程推理、工具调用全链路，降低对国外闭源模型的依赖。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/963/985.htm)

##### 🌐 **OpenRouter推出Fusion API——半价达Fable级智能**
- **核心内容**：OpenRouter推出Fusion API——市场上最智能的复合模型，以一半价格实现Fable级别的智能。核心创新是`/architect`模式：减少80%的Fable token使用，Fable负责协调/审核，Codex负责构建。这是复合模型（Compound Model）在成本效率上的重大突破。
- **落地应用场景**：**低成本高性能AI推理**——开发者和企业可在Fable 5被封禁的背景下，以半价获得接近Fable级别的推理能力，特别适用于需要大量token的编码和Agent任务。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/OpenRouter/status/2065856853989270011)

##### 🌐 **华为发布DevEco Code——鸿蒙开发AI Agent工具**
- **核心内容**：华为在HDC 2026期间发布DevEco Code，这是一款面向HarmonyOS开发场景的AI Agent工具，支持代码编写、编译构建、设备运行、文档查阅、运行时调试及ArkTS问题修复。DevEco Code基于开源项目OpenCode扩展，保留了其终端交互、配置体系和插件生态。配合HarmonyOS 7的Agent时代战略，形成鸿蒙原生AI开发闭环。
- **落地应用场景**：**鸿蒙生态AI原生开发**——鸿蒙开发者可使用DevEco Code完成从编码到调试的全流程AI辅助，大幅降低鸿蒙应用开发门槛，加速鸿蒙生态繁荣。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/963/943.htm)

##### 🌐 **NVIDIA Cosmos 3——全模态物理AI世界模型**
- **核心内容**：NVIDIA发布Cosmos 3——一种全模态世界模型，将语言、图像、视频、音频和动作整合到同一系统，使物理AI能跨越"理解、模拟、行动"三大任务。核心创新是将**动作视为世界的第一类语言**，通过动作token设计让模型可基于视频推断动作，或同时生成未来场景及对应运动。使机器人从"识别物体"升级为预测"移动、操作"的物理行为。
- **落地应用场景**：**机器人与具身智能**——Cosmos 3可直接用于机器人训练（从视频学习动作）、自动驾驶场景预测、工业自动化模拟，是NVIDIA物理AI战略的核心基础设施。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2065797839507312877)

##### 🌐 **Google Gemini-SQL2——Text-to-SQL新SOTA**
- **核心内容**：Google Research发布Gemini-SQL2，基于Gemini 3.1 Pro，在BIRD单模型排行榜上达到**80.04%**的执行准确率，大幅领先此前SOTA。能将自然语言翻译为可直接执行的SQL查询，应对现实世界数据的复杂性和混乱性。
- **落地应用场景**：**企业数据分析民主化**——非技术人员可通过自然语言直接查询企业数据库，降低BI分析门槛，特别适用于金融、零售、物流等数据密集型行业。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/google-researchs-gemini-sql2-tops-text-to-sql-benchmarks-by-a-wide-margin)

##### 🌐 **OpenAI Codex灵活速率限制重置——开启AI价格战**
- **核心内容**：OpenAI改变Codex编程智能体的速率限制策略——现在允许用户存储速率限制重置次数并手动触发，而非按固定时间到期。用户在使用中达到上限时可立即使用已保存的重置。Go、Plus、Pro和Business订阅计划用户各获得一次免费重置，Plus和Pro用户还可通过邀请好友解锁额外重置。
- **落地应用场景**：**编程Agent灵活计费**——开发者可根据项目周期灵活安排AI编程Agent使用量，避免固定重置周期造成的资源浪费，特别适用于赶deadline的团队。
- **相关链接**：[🌐 点击查看新闻来源](https://the-decoder.com/openai-kicks-off-the-ai-price-wars-with-flexible-rate-limit-resets-for-its-codex-coding-agent)

##### 🌐 **Higgsfield Games——从一条提示词到多人游戏**
- **核心内容**：Higgsfield推出Higgsfield Games，可从一条提示词直接构建并部署任意类型2D或3D多人游戏，自动生成角色、道具和场景。该产品由Claude Fable 5推理游戏创意，并通过Higgsfield MCP调用工具完成资产和物理逻辑构建。
- **落地应用场景**：**游戏创作平民化**——无编程经验的创作者可通过自然语言描述快速构建可玩的多人游戏原型，适用于游戏jam、教育游戏、品牌营销互动等场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2065790684188328077)

##### 🌐 **Artificial Analysis发布AA-AgentPerf基准**
- **核心内容**：Artificial Analysis发布新基准AA-AgentPerf，首批结果覆盖DeepSeek V4 Pro在NVIDIA Blackwell（GB300、B300）、Hopper（H200）及AMD MI355X上的推理能效。核心指标为**每兆瓦承载的并发智能体数**（要求20 token/s），为AI Agent部署提供首个标准化的能效评估框架。
- **落地应用场景**：**AI Agent部署的成本优化**——企业可基于AA-AgentPerf选择最具能效比的硬件平台部署Agent工作负载，优化每瓦特算力的Agent密度。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/ArtificialAnlys/status/2065559824230957190)

##### 🌐 **Sony AI Ace机器人击败专业乒乓球选手**（Nature论文）
- **核心内容**：Sony AI的Ace机器人在官方ITTF规则下击败了专业选手Miyuu Kihara，相关研究以"用自主机器人超越精英乒乓球选手"为题发表于Nature。这标志着具身机器人在高速动态对抗运动中达到人类精英水平。
- **落地应用场景**：**高速动态机器人**——该技术栈（高速感知+实时决策+精准执行）可迁移至工业分拣、运动训练辅助、服务机器人等需要实时物理交互的场景。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/rohanpaul_ai/status/2065801362680795407)

##### 🌐 **Jeff Bezos新创公司Prometheus聚焦物理AI**
- **核心内容**：Jeff Bezos创立的新公司Prometheus瞄准物理AI领域，是资金最充足的物理AI初创企业之一。结合同期NVIDIA Cosmos 3的发布，物理AI赛道正吸引顶级资本涌入。
- **落地应用场景**：**物理AI商业化**——Prometheus可能聚焦机器人、自动化制造或自主系统领域，将Bezos的亚马逊物流经验与物理AI技术结合。
- **相关链接**：[🌐 点击查看新闻来源](https://arstechnica.com/ai/2026/06/heres-what-jeff-bezos-new-startup-prometheus-will-do)

##### 🌐 **Google AI本周多项更新：Live Translate+NotebookLM+Project Genie**
- **核心内容**：Google AI本周推出多项更新：①Gemini 3.5 Live Translate——实时语音到语音翻译最新音频模型，支持70+语言；②NotebookLM获重大升级，加入智能体对话能力、更高级推理及新输出格式；③Project Genie向Google AI Ultra用户开放。
- **落地应用场景**：**跨语言实时沟通+AI知识管理**——Live Translate可集成到广播、会议、客服系统实现实时多语言沟通；NotebookLM升级让用户拥有AI研究助手处理复杂知识任务。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/GoogleAI/status/2065478191247130703)

---

*数据来源：Hugging Face Daily Papers、arXiv (cs.AI)、AI Hot (aihot.virxact.com)*
*文章由每日AI日报自动化生成，如需转载请注明来源。*
