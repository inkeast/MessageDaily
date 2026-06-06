---
title: "【每日AI前沿追踪】2026年06月05日 核心技术与产业动态速递"
date: 2026-06-05
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "6月5日AI前沿速递：OpenAI发布ChatGPT Dreaming V3记忆系统（准确率翻倍至82.8%）；Anthropic Mythos/Oceanus模型曝光+递归自我改进引发AI安全热议；腾讯混元开源PlanningBench+Stem稀疏注意力；Kimi Work发布300 Agent协作办公系统；小米机器人团队拿下CVPR 2026和ICRA 2026双料冠军；美团×人大发布Agent自进化框架；腾讯×中科大发布Agent元认知记忆优化。"
---

## 【每日AI前沿追踪】2026年06月05日 核心技术与产业动态速递

### 一、 今日核心洞察与重点摘要

- **OpenAI ChatGPT Dreaming V3记忆系统重大升级——从"记笔记"到"自主回忆"的范式转移**：OpenAI为ChatGPT推出全新"Dreaming"记忆架构（V3），从被动存储用户明确要求的信息，转变为后台自动扫描历史对话、构建按工作/爱好/旅行偏好分类的连贯用户画像。事实记忆准确率从41.5%飙升至82.8%，信息更新成功率从52.2%提升至84.1%，而所需算力仅为旧系统的1/5。Sam Altman称之为"ChatGPT记忆的里程碑"。同日，OpenAI Codex新增"Build iOS Apps"插件，Codex内可直接运行、测试和热重载iOS应用。

- **Anthropic Mythos/Oceanus模型曝光——递归自我改进引发全球AI安全辩论**：Anthropic下一代模型Mythos（内部代号Oceanus）被曝光，定价$16/$80每百万token，零样本输出惊艳。Anthropic同日发布重磅研究《AI递归自我改进》，称Claude正在深度参与开发下一代AI，模型可能已接近递归自我改进能力。Anthropic联合创始人Daniela Amodei呼吁全球建立"AI暂停按钮"。纳德拉公开抨击微软AI智能体Scout的致瘾计划。AI安全议题在产业和学术界同时升温。

- **Agent自进化与技能发现成为学术最热方向——多篇高质量论文集中涌现**：今日Agent相关论文呈现三大热点：① **Agent自进化与持续学习**——美团×人大发布"Rethinking Continual Experience Internalization"框架；腾讯×中科大发布"Meta-Cognitive Memory Policy Optimization"（元认知记忆策略优化）；港科大广州发布EvoDS数据科学Agent。② **Agent技能发现**——浙大发布"Unsupervised Skill Discovery for Agentic Data Analysis"；OpenSquilla发布MetaSkill自组织技能协议。③ **Agent安全与评测**——KAIST×DeepAuto发布TIDE主动问题发现框架；港大×山大×CMU发布SABER编码Agent安全基准。

- **国产AI生态多线出击——Kimi Work、腾讯混元、华为云、阿里千问齐发力**：月之暗面发布Kimi Work（300 Agent协作，面向办公场景，打通金融/科研/法律数据库）；腾讯混元开源PlanningBench LLM规划能力评测框架并发布Stem稀疏注意力算法（首字延迟降低3.6倍）；华为云联合20余家模型厂商发布"百模千态，云聚共赢"计划；阿里千问打造NBA Chat（NBA中国首个官方大模型）；深圳团队依托华为昇腾910C成功训练1.6万亿参数DeepSeek-V4-Pro；小米机器人团队拿下CVPR 2026和ICRA 2026双料冠军。

**产学研合作趋势观察**：今日产学研合作呈现以下特征：① **国内大厂×高校联合Agent自进化研究密集**——美团×中国人民大学（5位美团工业界作者）发布Agent持续经验内化框架，腾讯×中科大/浙大（3位腾讯作者，含通讯作者）发布Agent元认知记忆优化，是目前国内产学联合Agent研究的主力军。② **Agent技能发现进入"自动化"阶段**——浙大独立发表无监督技能发现论文，OpenSquilla发布自组织技能协议（工业界），表明Agent能力获取正从"人工设计"转向"自动涌现"。③ **国际产学合作聚焦Agent安全**——KAIST×DeepAuto（韩国×美国）、港大×山大×CMU（中国×美国）的多校/跨国合作在Agent安全评测领域成果显著。④ **编码Agent安全成为新焦点**——SABER基准（港大×山大×CMU×NUS×HKUST）首次系统评估编码Agent在有状态项目工作空间中的操作安全，填补了该领域空白。今日Agent相关论文超过80%涉及产学联合或跨机构合作。

---

### 二、 详细内容追踪

#### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

---

**论文名称**：**Rethinking Continual Experience Internalization for Self-Evolving LLM Agents**（重新思考自进化LLM Agent的持续经验内化）

- **核心亮点**：美团×中国人民大学提出一种全新的Agent自进化框架，解决LLM Agent如何从持续交互经验中有效内化知识的关键问题。该研究深入分析了现有经验内化方法的局限性，提出更高效的持续学习策略，使Agent能够在不断积累经验的同时避免灾难性遗忘。
- **团队背景**：**美团×中国人民大学×北航 强强联合**——第一作者来自中国人民大学高瓴人工智能学院，5位美团工业界作者参与（包括Chenxing Sun、Shaodong Zheng、Yangen Hu、Lu Pan、Ke Zeng），通讯作者为中国人民大学Yankai Lin。这是今日最具代表性的产学合作论文。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.04703)

---

**论文名称**：**Meta-Cognitive Memory Policy Optimization for Long-Horizon LLM Agents**（长周期LLM Agent的元认知记忆策略优化）

- **核心亮点**：腾讯×中科大提出元认知记忆策略优化框架，针对长周期Agent任务中记忆管理的挑战，引入"元认知"机制让Agent自主决定何时记忆、何时遗忘、如何整合信息。该框架显著提升了Agent在复杂多步骤任务中的长期表现。
- **团队背景**：**腾讯×中国科学技术大学×浙江大学 强强联合**——通讯作者Wei Xia来自腾讯，3位腾讯工业界作者参与（Wence Ji、Wei Xia、Feng Liu），学术方包括中科大和浙大。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2605.30159)

---

**论文名称**：**Unsupervised Skill Discovery for Agentic Data Analysis**（面向智能体数据分析的无监督技能发现）

- **核心亮点**：浙江大学提出一种无监督技能发现方法，让Agent自动从数据分析任务中发现、学习可复用的原子技能，而无需人工标注或预定义。该方法使Agent能够自主构建技能库，显著提升在多样化数据分析场景中的泛化能力。
- **团队背景**：浙江大学全作者团队，通讯作者Shumin Deng。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.06416)

---

**论文名称**：**TIDE: Proactive Multi-Problem Discovery via Template-Guided Iteration**（TIDE：通过模板引导迭代的主动多问题发现）

- **核心亮点**：KAIST提出TIDE框架，使Agent能够主动发现用户尚未意识到的潜在问题，而非被动等待用户提问。通过模板引导的迭代策略，Agent可以系统性地探索问题空间，在对话中主动引导用户发现和解决更多隐藏需求。
- **团队背景**：**KAIST×DeepAuto.ai 产学联合**——作者Sung Ju Hwang同时隶属KAIST和DeepAuto.ai。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.04743)

---

**论文名称**：**SABER: Benchmarking Operational Safety of LLM Coding Agents in Stateful Project Workspaces**（SABER：有状态项目工作空间中LLM编码Agent的操作安全基准）

- **核心亮点**：港大×山大×CMU×NUS×HKUST联合发布SABER，首个系统评估编码Agent在真实有状态项目工作空间中操作安全性的基准。该研究揭示编码Agent在编辑、删除、测试等操作中可能引入的安全风险，为构建更安全的AI编程工具提供了评估标准和改进方向。
- **团队背景**：多校国际合作——香港大学、山东大学、卡内基梅隆大学、新加坡国立大学、香港科技大学。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.01317)

---

**论文名称**：**EvoDS: Self-Evolving Autonomous Data Science Agent with Skill Learning and Context Management**（EvoDS：具备技能学习与上下文管理的自进化自主数据科学Agent）

- **核心亮点**：港科大广州发布EvoDS，一个具备自进化能力的自主数据科学Agent，通过技能学习和上下文管理实现端到端的数据分析自动化。该Agent能从历史分析任务中提取可复用技能，并智能管理分析上下文，在复杂数据科学工作流中表现出色。
- **团队背景**：香港科技大学（广州）全作者团队。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.03841)

---

**论文名称**：**Code2LoRA: Hypernetwork-Generated Adapters for Code Language Models under Software Evolution**（Code2LoRA：面向软件演化的代码语言模型超网络生成适配器）

- **核心亮点**：滑铁卢大学提出Code2LoRA，利用超网络为代码语言模型动态生成LoRA适配器，解决软件持续演化带来的代码模型适配难题。当代码库发生变更时，超网络能快速生成定制化适配器，使模型无需完全重新训练即可适应新版本的代码。
- **团队背景**：滑铁卢大学全作者团队。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.06492)

---

**论文名称**：**SePO: Self-Evolving Prompt Agent for System Prompt Optimization**（SePO：面向系统提示词优化的自进化提示词Agent）

- **核心亮点**：新加坡国立大学×香港城市大学提出SePO，一个自动优化系统提示词的自进化Agent。该Agent能够根据任务反馈自动迭代优化提示词策略，实现从"人工调Prompt"到"AI自我优化Prompt"的转变，在多种任务上显著提升LLM表现。
- **团队背景**：新加坡国立大学×香港城市大学合作。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.04465)

---

**论文名称**：**AdaPlanBench: Evaluating Adaptive Planning in Large Language Model Agents under World and User Constraints**（AdaPlanBench：评估LLM Agent在世界和用户约束下的自适应规划能力）

- **核心亮点**：UIUC发布AdaPlanBench基准，首次系统评估LLM Agent在动态变化的世界状态和用户约束下的自适应规划能力。该基准涵盖多种真实世界场景，揭示当前最先进LLM在复杂约束条件下规划能力的不足。
- **团队背景**：伊利诺伊大学香槟分校全作者团队。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.05622)

---

**论文名称**：**ArcANE: Do Role-Playing Language Agents Stay in Character at the Right Time?**（ArcANE：角色扮演语言Agent是否在正确的时候保持角色？）

- **核心亮点**：首尔大学提出ArcANE框架，系统评估角色扮演Agent是否能够在恰当的时机保持角色一致性。研究揭示当前Agent在角色保持方面存在"过度入戏"或"角色脱离"的问题，为提升Agent角色扮演的自然度提供了新视角。
- **团队背景**：首尔大学全作者团队。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.05553)

---

**论文名称**：**Combinatorial Synthesis: Scaling Code RLVR via Atomic Decomposition and Recombination**（组合合成：通过原子分解与重组扩展代码RLVR）

- **核心亮点**：中科院软件所×港中深×港科大提出组合合成方法，将代码生成任务分解为原子级代码片段，再通过智能重组实现规模化代码强化学习。该方法有效解决了代码RLVR数据稀缺问题，显著提升代码模型的推理验证能力。
- **团队背景**：中科院软件所、香港中文大学（深圳）、香港科技大学、利默里克大学多机构合作。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2605.31058)

---

**论文名称**：**MLEvolve: A Self-Evolving Framework for Automated Machine Learning Algorithm Discovery**（MLEvolve：自动化机器学习算法发现的自进化框架）

- **核心亮点**：上海人工智能实验室×华东师范大学提出MLEvolve，一个能自动发现和优化机器学习算法的自进化框架。该系统通过迭代生成、评估和改进ML算法，实现了从人工设计算法到AI自动发现算法的突破。
- **团队背景**：上海人工智能实验室×华东师范大学联合团队。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.06473)

---

**论文名称**：**AURA: Intent-Directed Probing for Implicit-Need Surfacing in Situated LLM Agents**（AURA：面向场景化LLM Agent隐含需求挖掘的意图引导探询）

- **核心亮点**：广东省智能科学技术研究院提出AURA框架，通过意图引导的探询策略帮助Agent挖掘用户的隐含需求。该框架使Agent能够在对话中主动识别用户未明确表达的需求，提升Agent在复杂场景下的服务质量。
- **团队背景**：广东省智能科学技术研究院全作者团队。
- **相关链接**：[📄 点击阅读论文原文](https://huggingface.co/papers/2606.05557)

---

#### 2. 产业动态与产品创新（AI Hot 精选）

---

**事件/产品名称**：**OpenAI ChatGPT Dreaming V3 记忆系统**

- **核心内容**：OpenAI为ChatGPT推出全新"Dreaming"记忆架构（V3版本），将记忆从"被动存储"升级为"主动维护画像"。新系统在后台自动扫描历史对话，构建按工作、爱好、旅行偏好分类的连贯用户档案。事实记忆准确率从41.5%提升至82.8%，信息更新成功率从52.2%提升至84.1%，算力消耗降至旧系统的1/5。支持记忆摘要展示，用户可查看、编辑和引导记忆内容。
- **落地应用场景**：面向所有ChatGPT Plus/Pro用户，适用于长期项目跟踪、个人偏好管理、跨对话连续性场景。对于使用ChatGPT进行编程、写作、研究的用户，新记忆系统将大幅减少重复解释背景信息的需求。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/OpenAI/status/2062567556524003631)

---

**事件/产品名称**：**OpenAI Codex "Build iOS Apps" 插件 + Codex 开放活动主页**

- **核心内容**：OpenAI为Codex新增"Build iOS Apps"官方插件，用户可在Codex内直接查看和测试iOS应用、打开SwiftUI预览、热重载编辑，无需离开Codex环境即可完成iOS应用开发闭环。同时，Codex推出个人资料页和活动主页，展示活动图、连续天数、累计token等数据，默认私密。
- **落地应用场景**：面向iOS开发者，实现"描述想法→生成应用→测试→发布"的全流程AI辅助开发。对独立开发者和创业团队而言，Codex大幅降低了iOS应用开发门槛。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/OpenAIDevs/status/2062599291479478275)

---

**事件/产品名称**：**Anthropic Mythos/Oceanus 模型曝光 + 递归自我改进研究**

- **核心内容**：Anthropic下一代模型Mythos（内部代号Oceanus）被曝光，定价$16/$80每百万token，零样本输出表现惊艳。Anthropic同日发布重磅研究，称Claude正在深度参与开发下一代AI，模型可能接近递归自我改进能力。联合创始人Daniela Amodei呼吁全球建立"AI暂停按钮"。据报Mythos模型正被用于NSA对中国和伊朗的进攻性网络行动。
- **落地应用场景**：Mythos在代码生成、复杂推理和Agent任务上的能力大幅提升；递归自我改进研究为AI安全治理提供关键参考。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/kimmonismus/status/2062805570982203820)

---

**事件/产品名称**：**Kimi Work 发布：300 Agent 协作办公系统**

- **核心内容**：月之暗面发布Kimi Work，整合Kimi Code核心功能和Kimi Agent的专业Skills（建站、PPT等），打通金融、科研、法律等专业数据库。系统支持300个Agent同时协作，用户无需终端或命令行，安装即可使用。
- **落地应用场景**：面向办公场景，覆盖文档撰写、数据分析、PPT制作、专业信息检索等。对不熟悉编程的商务用户，Kimi Work提供了零门槛的AI办公入口。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/xiaohu/status/2062824756634931256)

---

**事件/产品名称**：**腾讯混元 PlanningBench + Stem 稀疏注意力**

- **核心内容**：腾讯混元联合人大高瓴开源PlanningBench——首个LLM规划能力评测框架，覆盖多步规划、约束满足、动态适应等维度。同时发布Stem稀疏注意力算法，首字延迟降低3.6倍，显著提升LLM推理效率。
- **落地应用场景**：PlanningBench为Agent规划能力提供统一评测标准；Stem注意力算法可直接应用于大模型推理服务优化，降低API响应延迟。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/berryxia/status/2062866734596673828)

---

**事件/产品名称**：**华为云"百模千态，云聚共赢"生态计划**

- **核心内容**：华为云在INSPIRE创想者大会上联合智谱、DeepSeek、MiniMax、Kimi等20余家厂商发布"百模千态，云聚共赢"计划，共建系统化商业生态。同时推出Agentic Infra新范式及四大新服务。
- **落地应用场景**：为国内AI模型厂商提供统一的云计算基础设施和商业分发渠道，加速国产大模型的企业级落地。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/960/720.htm)

---

**事件/产品名称**：**小米机器人团队 CVPR 2026 + ICRA 2026 双料冠军**

- **核心内容**：小米机器人团队同时获得CVPR 2026和ICRA 2026比赛冠军，展示其在机器人视觉感知和自主导航领域的技术实力。
- **落地应用场景**：提升小米在具身智能和机器人领域的技术储备，为未来消费级机器人产品的研发奠定基础。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/960/509.htm)

---

**事件/产品名称**：**阿里千问 × NBA中国：NBA Chat 官方大模型**

- **核心内容**：NBA中国与阿里巴巴共同推出首个官方大模型"NBA Chat"，模型底座为阿里千问，结合篮球历史数据、球员深度分析等数字资产进行微调。已上线"NBA中国"App，支持智能篮球数据解读。
- **落地应用场景**：面向篮球爱好者，提供球员数据解读、赛事分析、历史对比等智能问答服务。是体育+AI垂直落地的典型案例。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/960/548.htm)

---

**事件/产品名称**：**Cursor Canvases + Design Mode 视觉提示**

- **核心内容**：Cursor发布Canvases（类似Codex Sites），支持用户在Design Mode中通过点击元素、在页面上绘制区域或语音描述来向AI智能体传达修改意图。智能体将元素身份（xpath、组件、属性、计算样式等）与页面截图一并纳入上下文，快速定位并执行修改。同时推出Cursor Profiles，用户可认领个人用户名。
- **落地应用场景**：面向前端开发者，实现"所见即所指"的AI编码体验。无需手动描述元素位置，直接在页面上圈选即可让AI理解修改意图。
- **相关链接**：[🌐 点击查看新闻来源](https://cursor.com/blog/design-mode)

---

**事件/产品名称**：**Perplexity 混合本地-云端推理编排器**

- **核心内容**：Perplexity AI发布面向个人电脑的混合推理编排器，可自动将AI任务在设备端模型与云端模型之间动态路由。简单任务走本地推理保护隐私，复杂任务自动上传云端获取更强能力，实现推理负载的智能分配与优化。
- **落地应用场景**：面向注重隐私的企业和个人用户，在敏感数据处理场景中优先使用本地模型，在需要更强推理能力时自动切换云端。
- **相关链接**：[🌐 点击查看新闻来源](https://www.marktechpost.com/2026/06/05/perplexity-ai-introduces-hybrid-local-server-inference-orchestrator-for-personal-computer-automatic-on-device-and-cloud-task-routing)

---

**事件/产品名称**：**Meta Business Agent 对话式商务 + 人脸识别智能眼镜**

- **核心内容**：Meta推出Business Agent，在Instagram、Messenger和即将上线的WhatsApp中原生集成对话式商务工作流，使全球零售品牌能直接在消息应用中自动执行交易。同时，Meta智能眼镜App被曝暗藏人脸识别代码，NameTag功能已推送至超5000万设备。
- **落地应用场景**：Business Agent面向全球零售品牌的电商自动化；人脸识别功能（争议性较大）可在社交场景中实时识别他人身份。
- **相关链接**：[🌐 点击查看新闻来源](https://www.artificialintelligence-news.com/news/meta-business-agent-ai-powered-conversational-commerce)

---

**事件/产品名称**：**Cloudflare AI Gateway 消费限制功能**

- **核心内容**：Cloudflare AI Gateway新增实时消费限制功能，防止跨多个AI提供商的token账单失控。通过与Cloudflare Access集成，企业可使用基于身份的预算和策略管理AI使用成本。
- **落地应用场景**：面向使用多家AI API的企业IT团队，实现统一的AI支出管控、预算分配和异常使用预警。
- **相关链接**：[🌐 点击查看新闻来源](https://blog.cloudflare.com/ai-gateway-spend-limits)

---

**事件/产品名称**：**Apple 批准 Poke 首个 iMessage AI 智能体**

- **核心内容**：初创公司Poke获批成为Apple Messages for Business平台上的首个AI智能体。用户可通过简单短信与AI智能体交互，实现回邮件、设提醒等功能。
- **落地应用场景**：标志着苹果生态正式向AI智能体开放，为iMessage平台上的AI助手服务铺平道路。
- **相关链接**：[🌐 点击查看新闻来源](https://techcrunch.com/2026/06/04/apple-approves-poke-as-the-first-ai-agent-on-its-messages-for-business-platform)

---

**事件/产品名称**：**腾讯 AI Token 额度改革 + 代码 AI 化**

- **核心内容**：腾讯内部调整AI Token额度分配机制，从固定额度改为按工作任务动态调配，"看产出不看消耗"。腾讯高级执行副总裁汤道生透露，今年腾讯大部分代码都由AI生成。姚顺雨透露AI下半场主攻三个方向。
- **落地应用场景**：企业内部AI资源管理的最佳实践参考，从"按人头分"到"按产出分"的效率导向改革。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/960/556.htm)

---

**事件/产品名称**：**DeepSeek-V4-Pro 1.6万亿参数模型成功训练 + 深圳团队昇腾910C**

- **核心内容**：深圳团队依托华为昇腾910C成功训练1.6万亿参数DeepSeek-V4-Pro大模型。DeepSeek连续四周蝉联OpenRouter份额第一。
- **落地应用场景**：证明国产AI芯片（昇腾910C）已具备训练万亿级参数模型的能力，为国产大模型全栈自主可控提供关键技术验证。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/960/281.htm)

---

**事件/产品名称**：**xAI Grok 多线更新：Worktrees + Build 0.2.20 + 图转视频**

- **核心内容**：xAI密集发布多项更新——Grok支持worktrees并行智能体（允许在独立工作区并行运行AI Agent）；Grok Build 0.2.20修复多项Bug并新增工具；Grok Imagine Video 1.5正式开放预览（720p图转视频）；Grok登顶苹果App Store"ai app"搜索。马斯克在JP摩根炉边谈话中宣布SpaceX将因星链和轨道AI数据中心而上市。
- **落地应用场景**：Worktrees面向开发者多任务并行场景；图转视频面向内容创作者；SpaceX上市为AI基础设施投资打开新渠道。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/elonmusk/status/2062796095764234421)

---

**事件/产品名称**：**NVIDIA DGX Spark 推理速度提升2.6倍 + Cosmos 3 发布**

- **核心内容**：NVIDIA发布DGX Spark更新，使用NVIDIA NemoClaw将推理速度提升高达2.6倍，简化本地Agent工作流。Cosmos 3作为首个全模态物理AI开放模型正式发布。黄仁勋确认三星、SK海力士、美光通过HBM4认证，Vera Rubin平台已进入量产。
- **落地应用场景**：DGX Spark面向本地Agent部署场景，2.6倍提速直接降低企业推理成本；Cosmos 3面向自动驾驶、机器人等物理AI应用。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/nvidia/status/2062583740392435852)

---

**事件/产品名称**：**Anthropic 开源 AI 漏洞发现框架**

- **核心内容**：Anthropic将其用于AI驱动漏洞发现的开源框架代码托管在GitHub，借助AI技术帮助识别软件中的安全缺陷。同日，Claude Code v2.1.163和v2.1.165连续发布。
- **落地应用场景**：面向安全研究人员和开发者，AI辅助漏洞发现可大幅提升代码审计效率。Claude Code的版本管理功能增强（新增版本范围限制），面向企业IT管控场景。
- **相关链接**：[🌐 点击查看新闻来源](https://github.com/anthropics/defending-code-reference-harness)

---

**事件/产品名称**：**开源鸿蒙 OpenHarmony EmbodiedAI 1.0.1 发布**

- **核心内容**：开源鸿蒙具身智能PMC发布EmbodiedAI 1.0.1版本，聚焦机器人控制与智能体应用，升级导航规划、运动控制、仿真开发、硬件适配等核心能力，兼容ROS生态、机器人模拟器及多种传感器。
- **落地应用场景**：面向国内机器人开发者和教育机构，提供基于鸿蒙生态的具身智能开发平台，降低机器人应用开发门槛。
- **相关链接**：[🌐 点击查看新闻来源](https://www.ithome.com/0/960/722.htm)

---

**事件/产品名称**：**Airbnb CEO 筹建 AI Lab 专注 UI 设计模型**

- **核心内容**：Airbnb CEO Brian Chesky计划成立新AI实验室，专注于UI设计模型研发。TechCrunch和Bloomberg均确认该消息。
- **落地应用场景**：AI生成UI设计，面向产品设计团队，可能颠覆传统UI/UX设计流程。
- **相关链接**：[🌐 点击查看新闻来源](https://techcrunch.com/2026/06/04/airbnbs-brian-chesky-plans-to-launch-a-new-ai-lab)

---

**事件/产品名称**：**快手可灵 AI 两周年：全球用户突破1亿**

- **核心内容**：快手可灵AI发布两周年，26次迭代，全球用户突破1亿，企业客户近5万。Runway生成游戏电影《50 Crowns》（单人一周制作完成）展示AI视频创作的成熟度。
- **落地应用场景**：可灵AI面向个人创作者和企业营销；Runway面向影视制作——AI视频生成工具已从"尝鲜"进入"实用"阶段。
- **相关链接**：[🌐 点击查看新闻来源](https://x.com/Kling_ai/status/2062912327385575895)
