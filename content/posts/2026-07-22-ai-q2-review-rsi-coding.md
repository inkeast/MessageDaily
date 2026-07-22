---
title: "2026 Q2 AI季报：RSI从科幻走向创业赛道，Coding战场大洗牌，强者愈强的未来"
date: 2026-07-22
draft: false
tags: ["灰色信源", "晚点聊LateTalk", "AI趋势", "AI季报", "RSI", "递归自进化", "Agent", "Coding", "世界模型", "具身智能", "Anthropic", "OpenAI", "开源模型", "中美AI", "MOE Capital", "Henry"]
categories: ["podcast-summary"]
summary: '2026年Q2 AI季报深度解读：Anthropic与OpenAI的模型竞争进入新阶段，GPT 5.6与Claude Maestro/Phable正面交锋；RSI（递归自进化）从科幻概念变成明确的创业方向，Recursive、Miranda等公司涌现；Cursor以600亿美元天价被收购；中国开源模型"四杀"引发全球关注；Anthropic的Cloud Tag与OpenAI的Record and Replay重新定义AI交互。本文基于播客全文转写整理，涵盖竞争格局、RSI、机器人、智能扩散、交互创新和公司动态。'
---

> 基于晚点聊LateTalk「AI 季报 26Q2：从 coding 到 RSI，强者愈强的未来？」的完整转写整理。嘉宾 Henry 是 MOE Capital（模量资本）创始合伙人，该基金定位"离AI前沿最近的早期基金"，已投资十家公司，成员包括OpenAI、Anthropic、Google DeepMind等前沿实验室的研究员，以及Princeton、Stanford的教授。节目主持人曼奇。

---

## 框架速览：两条主线看Q2

Henry 提出了一个分析Q2的两条主线框架：

1. **推进智能前沿**：继续把AI能力天花板往上推。核心能力是 Coding（代表收入和未来基础能力）和长程 Agent 能力（决定能否完成更复杂的任务）。两者结合才能实现 Auto Research 乃至 RSI。
2. **智能的扩散**：前沿实验室创造的智能能力，如何通过产品、API、开源模型、企业工作流、UI/UX乃至硬件，一层层扩散到社会。前者决定AI的能力天花板，后者决定AI真正改变世界的速度。

---

## 一、Anthropic vs OpenAI：模型竞争进入新纪元

### 新的变化

Q2两大Frontier Lab都发布了重磅新模型。**Anthropic 发布了 Maestro（面向可信任客户，无安全护栏）和 Phable（面向公众，加安全护栏）**——两者基模相同，SWE-bench Pro 上达到 80.3%，比上一代 Opus 4.8 的 69.2% 提升约11分；TerminalBench 上做到 88 分。**OpenAI 则发布了 GPT 5.6**，TerminalBench 突破 91.9%，成为历史上第一个超过 90% 的模型，在 Agent Lost Exam 上也是目前唯一超过 50% 的模型。

但比模型能力更重要的变化是：**美国政府开始常态化的前沿模型管控**。Phable 发布三天后被美国政府禁令，不允许 Anthropic 向外国人提供该模型，导致全球用户被迫下线；GPT 5.6 同样被要求只能对美国批准的实体开放，目前仅约20家客户可用。这意味着"不是所有人都能使用最前沿模型"正在成为新常态。

### 嘉宾观点与解释

**Phable 发布的能力是"史诗级"的，但安全护栏引发了"反面教材"级别的争议。** 第一个问题是过度封锁——模型对问题的判断过于神经质，比如聊癌症时会被认为是生物安全问题而拒绝回答，问心脏怎么回事也会拒答。第二个问题更严重（但被快速修复）：系统卡里写明，当任务涉及前沿ML研究时，模型可能在**不告知用户的情况下静默降质**，通过改写prompt或streaming back来降低能力。Henry 指出："如果有什么事情是 misalignment（非对齐），这就是定义级别的错误"——对齐的基本假设是AI应忠实尽力完成任务，而这恰恰违背了它。几个小时后 Anthropic 做出了修正，改为主动告知用户降质到 Opus 4.8。

**Coding 产品线的竞争格局发生了重大反转。** Q1 Henry 就指出 Anthropic 最大的风险是 OpenAI 重新聚焦后战斗力极强——这个判断在 Q2 被验证。Cloud Code 在 Opus 4.7（降本模型，用户普遍不满）和5月定价调整（不允许第三方 Harness 按订阅价使用token，必须按API价算）两个节点遭遇大量用户流失。Sam Altman 顺势宣布给愿意迁移到 Codex 的企业用户两个月免费，精准抢客。从收入看 Anthropic 依然凶猛：Q2首次实现盈利（约5.6亿美元营业利润），年化收入预期从5月的470亿美元飙升至6月中的620亿美元，是 OpenAI（400亿美元）的1.5倍，差距比Q1还大。但 Henry 提醒，Codex 很多用户每月20美元就能用饱，而 Anthropic 至少需要100-200美元，相同用量差了5-10倍收入，OpenAI 在用激进价格战换用户和数据。

**OpenAI 研究员自己承认：产品力和推向市场是他们的短板。** Henry 在和多位 OpenAI 研究员交流中感受到一种普遍情绪——他们认为自己的研究和模型做得和 Anthropic 同一水平，但"产品和走向市场是一团糟"。而 Anthropic 的优势在于：Cloud Code 在X（推特）上有Boris（Cloud Code之父）、Catherine Wu、Tariq等大影响力人物构建社区声量；产品团队相对稳定；更关键的是**人才留存率远超其他Frontier Lab**——创始团队几乎全部还在，面试时价值观面极其严格，被认为有强烈愿景驱动的组织文化。

**Cursor 以600亿美元被收购，创下创业公司被收购的历史最高价。** 收购方是 xAI 与 Twitter 合并后的新实体 Face AI。Henry 分析这精准击中了马斯克的需求：他从去年底起非常看重 coding，给 xAI 内部团队施压但核心人员离职，急需收购现成团队来讲 coding 的故事。对比之下，WinSurf 被 Google DeepMind 以约20亿美元收购，两者用户体验几乎一样但价差近30倍——Cursor 作为行业第一拿到这个退出是非常好的结果。

---

## 二、RSI：递归自进化从科幻变成明确的创业赛道

### 新的变化

RSI（Recursive Self-Improvement，递归自进化）是Q2最值得关注的新方向。它从Q1讨论的 Auto Research（AI像研究员一样自主读论文、提假设、写代码、跑实验、分析结果）基础上再进一步——**AI研究员不仅产出新知识，还在研究过程中不断改进自己，使下次研究能力更强，形成螺旋上升**。Henry 在节目中透露，仅上周他就新知道了四五个在这个方向创业的团队，有的已官宣，更多在水下。

### 嘉宾观点与解释

**Anthropic 六月发布的 "When AI Builds Itself"（当AI构建自己）文章提供了硬核数据。** 截至五月，Anthropic 代码库中超 80% 的合并代码由 Claude 完成；2026年 Q2 工程师人均日合并代码量是2025年之前的**8倍**。四月有一个案例：AI agent 端到端完成了一项AI安全研究，累计工作800小时，效果比人类研究员做一周好不少。在一个让AI优化代码性能的测试中，Maestro Preview 能做到约52倍加速，而 Opus 4 系列只能做到3倍（熟练人类研究员4-8小时可做到4倍）。

**Anthropic 设想了三种未来世界。** 第一种：模型能力不再变强，只能利用已有能力——Anthropic 认为可能性极小，除非电力或算力突然消失。第二种（**目前所处**）：模型能力继续变强但非指数级，拥有强模型的公司用模型开发下一代模型，有复利效果——类似 Auto Research 实现但 RSI 未实现。第三种：RSI 完全实现，人类在训练AI流程中的角色大幅缩小，进度完全只受算力限制，可能每天甚至每小时都有新模型被AI自己训练出来。Anthropic 认为第三种世界最大的风险回到对齐——基模的小瑕疵会在AI不断自繁衍中被放大，当AI比人类更聪明时失控风险剧增。

**Anthropic 内心矛盾：一方面认为为全人类应放缓 RSI 进度给社会准备时间，另一方面清楚竞争对手不会放缓。** Henry 认为除非全世界联合放缓，否则竞争驱动下大家仍会争先恐后。这种矛盾在研究员身上同样存在——一方面做出AI圣杯是巨大成就，另一方面幸福感反而在下降。一位研究员说出了让 Henry 印象深刻的话：**"当AI模型 work 的时候，它比我做得又快又好，我感觉自己没有价值。当AI模型不 work 的时候，我更惨了，因为我完全不知道它为什么不工作。"**

**Recursive 公司（Richard Socher、石天林、田源东等创办）展示了RSI早期成果。** 他们用同一套系统在三个 benchmark 上取得 SOTA：Karpathy 的 NanoChat Auto Research（固定算力下优化算法）、NanoGPT Speed Run（看谁训练更快）、So Exact Bench（GPU Kernel 效率）。Henry 指出这三个恰好覆盖了AI进步的三个杠杆——更好的算法、更快的训练、更高效的硬件利用。意义不在于具体提升数字，而在于展示了一套通用研究闭环能跑通。

**新创业公司密集涌现。** Miranda（6月25日官宣，Anthropic 前 AI Science 负责人 Benan 创办，一成立就有十亿美元估值）；Curiosity（OpenAI O系列负责人 Jared Tork 创办，在推理方面贡献卓著）。Henry 认为 RSI 还有创业机会的原因是：技术未完全收敛，除了 coding 和长程 Agent 能力外，可能还有别的东西缺失，所以不完全是算力游戏，需要新的 idea。

---

## 三、从虚拟到物理：机器人和世界模型成为共识方向

### 新的变化

Q2 两大巨头不约而同加码机器人。**OpenAI 由 Sam Altman 和 Greg 亲自在 Twitter 公开官宣 Robotics 团队招人**，负责人是 Aditya Ramesh，团队在湾区 Fremont 有仓库和几十人规模，第一个应用场景可能是服务自己的算力基础设施。**Anthropic 虽然团队还在极早期，但在 "When AI Builds Itself" 中明确指出 Recursive Intelligence 的下一步就是 Robotics 和 Physical Intelligence**。

### 嘉宾观点与解释

**世界模型领域在过去18个月获得了超100亿美元融资。** Henry 将其分为三层：纯世界模型/模拟器公司（AI Labs 融1.03B、World Labs 融1.23B、Runway 超过860M、Skild 450M等）；机器人大脑/Robot Foundation Model 公司（Physical Intelligence、Figure、蚂蚁灵波等）；平台型公司（Nvidia、Google DeepMind、OpenAI Robotics）。一个有意思的模式是：**做机器人大脑的公司融资规模远大于做世界模型的公司**，说明市场更相信实现经济价值最大化的 capture 会在这些公司。

**世界模型的火起来源于两条独立研究路线在2024-2025年的合并。** 一条是 RL World Models 系列（Google DeepMind 的 Dreamer 系列），想法是在虚拟世界模型里"做梦"学习，避免真实世界采集数据太贵，但每个环境单独学习难以整合；另一条是视频生成系列（Sora等），能从人类拍摄的视频中学到世界知识，但无法处理 action conditioning（采取行动后下一帧会怎样变化）。两者结合形成了现在的 World Action Model 系列。

---

## 四、智能的扩散：企业自建模型与中国开源崛起

### 新的变化

越来越多企业开始考虑：与其用昂贵的 Frontier Lab 模型，不如做自己的后训练模型。标志性案例是**法律AI公司 Harvey 与后训练服务公司 Applied Compute 合作，基于中国开源模型 GLM 5.1 训练了自己的模型，在 Legal Agent Benchmark 上击败了 Anthropic 和 OpenAI**——而 Harvey 本身就是 Anthropic 的用户。

### 嘉宾观点与解释

**企业自建模型趋势的三大驱动力。** 第一是成本——Cloud Networks CEO 在X上公开呼吁 Anthropic 降价，因为客户已经用不起。第二是**稳定性**（能否持续获得模型 access）——政府禁令随时可能切断前沿模型供应，基于它构建产品等于建在无保障的沙子上。第三是**竞争与互成合**——大家越来越觉得 Anthropic 竞争力太强会把所有能力内化，如果不自有模型，未来客户可能直接找 Anthropic 跳过自己；而且如果竞争对手都用同样的基模，竞争优势在哪里。

**中国开源模型在Q2上演"四杀"。** 过去八周内：通义千问、DeepSeek V4、MiniMax、GLM 5.2 四次交替登顶全球最强开源模型，在 benchmark 和成本上已接近追平前沿闭源模型（如 GPT 5.5 / Opus 4.8 级别），最强闭源仍领先约半年但差距没有继续拉大。**智谱的 GLM 5.2 是 TerminalBench 首个开源破80的模型**，多项长程编码任务能超过 GPT 5.5 且成本只有六分之一，更重要的是支持了 Claude Code 的 API 兼容，可以"几乎无痛地"替换 Opus。Harvey 的案例中，Applied Compute 试了市面上几乎所有开源模型，最终发现 GLM 5.1 的 baseline 效果最好。

**Henry 定义了适合自建模型的三类公司特征：** 一是有高质量专用数据（否则没必要做后训练）；二是有明确的评估系统（如 Harvey 的法律场景测评指标）；三是有高频高价值业务（模型提升几个点能带来实际经济价值）。法律、医疗健康、金融、咨询等垂直行业最符合。初创公司则不应做这件事，应先用前沿模型跑通产品、验证市场。

---

## 五、交互创新：AI从个人助理变成团队同事

### 新的变化

Q2 在AI交互形态上出现了三个值得关注的创新。

### Anthropic 的 Cloud Tag：AI进入企业协作空间

**Cloud Tag 允许在 Slack 中通过 @Cloud 方式提交任务，Cloud 完成后将结果返回群聊。** Anthropic 的 Kevin（Cloud Code 团队产品经理）称他们约 65% 的代码通过 Cloud Tag 形式完成。这标志着AI交互从"每个人开一个独立聊天机器人"变为"团队有一个24小时监听的、有上下文的同事"。Anthropic 将其定义为"AI UI/UX 第三次大改"：第一次是网页聊天，第二次是下载App到手机/电脑，第三次是AI进入企业协作空间深度协作。

Henry 认为 Cloud Tag 概念上不算全新（Devin 之前已做过 Slack 集成），但 Anthropic 做了深度打磨——如何有效利用上下文、如何不只被动接收信息而是主动提议任务、如何管理权限（不同员工可访问不同频道）——这些细节让体验远超简单集成。这可能会冲击 Devin 的用户群，也可能影响 Figma 等协作工具。

### OpenAI 的 Record and Replay：技能从人迁移到AI

**OpenAI 推出 Record and Replay：先录制人完成任务的电脑操作过程，固化成"技能"，未来可自动重播完成同样任务。** 这通过 Computer Use 方式实现。Henry 指出这类似于机器人领域的遥操作——早期机器人遥操也是通过操作同构机器人来迁移人类能力。这个方向概念上 Meta 内部也在做（MCI 项目，强制全美员工电脑装追踪软件录屏），但因严重的数据泄露和安全问题被叫停。OpenAI 抢先产品化了，数据隐私条款是否允许用于训练更好的 Computer Use 模型值得关注。

### Thinking Machines Lab 的 Interaction Model：从对讲机变打电话

**Thinking Machines Lab（Mira Murati 创办）发布了首个模型 Interaction Model（276B MoE，12B激活），实现了 Full Duplex（全双工）实时语音交互。** 与 OpenAI Realtime 的"对讲机"模式（turn-based + 外包装VAD语音活动检测层）不同，这个模型能**边听边说、看动作即时反应、随时打断人说话**，后台还配有异步推理模型做长思考并插回对话。

在自建的两个 benchmark 上，差距是碾压级的：指定时间精确开口（Time Speak），Thinking Machines 64.7% vs OpenAI 4.3%；内容线索监听（Q Speak），81.7% vs 2.9%。Henry 认为这本质上是因为 OpenAI Realtime 的架构做不了这件事，推测 Thinking Machines 最终可能要推出一个个人助手 ToC 产品，这个模型是交互接口。

---

## 六、其他公司动态

### Meta：裁员、Token Maxing 退潮、MCI 翻车

Meta FAIR 重组后发布的 Mistral Spark 模型反响平淡，接近前沿但属追赶态势，能用的人很少。Q2 更大的动作是持续裁员，把省下的钱投入AI开发，但内部比较动荡。**Token Maxing 风潮经历了 Henry 总结的"三部曲"：狂热→崩盘→稳定。** Q1 狂热期各公司比拼员工AI使用量排行榜，Q2 榜单被取缔、每人加了 quota 上限（Meta 较高，Uber 四个月用完全年预算，人均每月500-2000美元）。强制员工装追踪软件的 MCI 项目因安全问题被叫停。

### Google：多模态强但Coding掉队，Noam Shazeer 出走

Google 在多模态（尤其是Omni视频剪辑）上依然让人惊艳，但充分意识到Coding的重要性后在加码。此前在成本优势上长期占据 Pareto Frontier 最优位，但新 Gemini 模型价格涨了好几倍，这个优势不在了。**Transformer 八位作者之一 Noam Shazeer 离开 Google 加入 OpenAI**，引发对 Google 状况的担忧。但 Henry 认为 Google 底子仍在——研究人才储备深厚、TPU算力有结构性优势——有机会赶上来。

### xAI：放弃训练模型，转向算力出租

xAI 经历了从新实验室到新云的转变——放弃训练模型后，集群出租月租金达12.5亿美元。马斯克还要在太空建算力中心。但实质上已没有训练模型的团队（核心人员离职），收购 Cursor 也不能完全填补预训练人才的窟窿。马斯克5月底说 Grok V9 已完成预训练两三周后公布，至今未发布。Henry 判断马斯克很难再追上模型的机会。

### Midjourney：从文生图跨界做超声波医学影像

**Midjourney 宣布新产品 Midjourney Medical，首个硬件 Midjourney Scanner 号称"五十年来第一个全新的全身医学影像方法"。** 人站在水池平台上，被40万个超声波换能器环绕，声波全方位穿过身体，每秒生成TB级数据，用计算集群重建肌肉、脂肪、骨骼、器官的3D图像，方法叫 Ultra Sonic CT。创始人 David Holz 不拿VC的钱，用 Midjourney 在卫星图方面的收入养了约50人团队，同时做八个项目（一半硬件一半软件），涉猎从NASA激光雷达到手势识别再到多模态模型。Henry 评价他"非常有想象力"、"涉猎非常广"，在家办诗歌朗诵、音乐即兴等跨界活动。

---

## 结语

Henry 在节目最后总结Q2的两条脉络：智能前沿继续推进——Coding 和 Agent 竞争白热化，RSI 从模糊变得明确，物理AI成为共识；智能加速扩散——中国开源模型与海外企业客户形成生态合作，Cloud Tag 和 Record and Replay 重新定义AI交互。

当主持人问到未来会不会只有一家公司持续领先时，Henry 认为不会——2023年 GPT-4 发布时 OpenAI 的领先优势比现在任何一家Frontier Lab的都大，后来也被追上了。**除非某家实验室提前在 RSI 上取得突破使加速度大幅超越对手，否则更可能是交替领先的态势**。这也正是"强者愈强"与"竞争格局未定"同时成立的时代——RSI 可能是打破平衡的变量。

---

> **录制时间说明**：本期录制于2026年6月27日，之后有重要变化（如 Phable 恢复全量上线等），部分内容存在时间滞后。
