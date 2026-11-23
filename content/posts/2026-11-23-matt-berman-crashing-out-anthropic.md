---
title: "IPO 日的矛盾清单：Matthew Berman 破防式吐槽 Anthropic——警告末日的人也在建造它"
date: 2026-11-23T12:13:43+08:00
draft: false
tags: ["灰色信源", "Matthew Berman", "Anthropic", "RSI", "AI安全", "IPO", "AI趋势"]
categories: ["podcast-summary"]
summary: 'Anthropic 上市当天，Matthew Berman 发了自称「忍不住」的破防视频：这是他见过最矛盾的公司——一边称 AI 是史上最危险的技术、一边推进递归自我改进（RSI）；反开源反蒸馏，却训练于全人类知识并游说监管；给模型立宪法、养福利团队；更有员工称「没有对齐计划」。本文把他的观点层与事实层拆开，IPO 背景一句带过并互链站内前文。'
---

> 信源说明：本文基于 Matthew Berman 频道 2026-11-21 发布的单人评论视频《I'm Crashing Out About Anthropic》（vid `I0Fasi1-teQ`，全长 659 秒≈11 分钟；英文转写 6 段、0s–659s 已完整读取、无跳读、时间戳连续）。讲者身份经音频自述核实为 Berman 本人：他自述采访过 Sam Altman 与 Mustafa Suleyman、并为那篇 METR 报告「做过一整期视频」——即[站内 11 月 8 日这篇](https://inkeast.github.io/MessageDaily/posts/2026-11-08-matt-berman-metr-anthropic-rsi-12-months/)，与频道一贯口径一致，全片单人无嘉宾。这是一期他自己明说的情绪化视频（crashing out，网络语境≈「破防/绷不住了」）：开场自嘲「我尽量不做黑子」（I try not to be too much of a hater），并主动披露利益相关——他与 Anthropic 有合作（获模型早期访问）、认识多位员工。**文中所有对 Anthropic 动机与「矛盾」的定性均为 Berman 的观点，本文不背书**；事实性内容按「Berman 陈述 / Berman 引用 / 推断」分层标注。防重复：IPO 发行数字与分阶段解锁日历已有 [Matt Wolfe 周报文](https://inkeast.github.io/MessageDaily/posts/2026-11-22-matt-wolfe-news-anthropic-ipo-unlock/)展开，上市日「安全叙事 vs ARR」错位与门外 RSI 抗议已有 [Hard Fork 文](https://inkeast.github.io/MessageDaily/posts/2026-11-22-hardfork-gta6-anthropic-ipo/)展开，公司史与 S-1 另有[《硅谷101》](https://inkeast.github.io/MessageDaily/posts/2026-11-19-sv101-anthropic-history/)与 [Latent Space 拆解](https://inkeast.github.io/MessageDaily/posts/2026-11-21-latent-space-anthropic-ipo-tanay-jaipuria/)——以上背景本文全部一句带过并互链，只展开 Berman 本期独有增量。ASR 校正见文末表；修正版英文转写与逐段中文翻译附录由脚本生成并断言段数。

## 先说结论

在 Anthropic 敲钟上市的当天，长期报道他们的 Matthew Berman 发了一期他自称「忍不住了」的破防吐槽，核心论点一句话：这是他「见过最矛盾的公司」，而且矛盾是成套的——一边告诉世界 AI 可能是史上最危险的技术、呼吁给进度「定节奏」，一边全力推进他眼中真正危险的 RSI（递归自我改进，recursive self-improvement：模型改进模型自身、人类退出循环）；一边反开源、反蒸馏（distillation，用问答对把大模型能力「教」给小模型），一边「基本蒸馏了全人类的知识」并以此完成了他所说的史上最赚钱生意之一。Berman 对读者真正有用的部分不是情绪，而是三套机制：安全叙事如何与游说清单、商业条款拼成在位者的护城河；「告诉模型它可能活着」如何在产品层自我实现；以及当最强技术集中于一家公司时，「事实上的世界政府」如何成形。本文把他的观点层与事实层拆开；IPO 本身站内已有三篇展开，不复述。

## 一、「我尽量不做黑子」：一份他攒了很久的矛盾清单

Berman 的开场面面俱到地做了免责声明：他不认为 Anthropic 绝大多数员工是坏人，认为他们「真诚关心 AI 安全」，还主动交代自己与 Anthropic 有合作、拿过模型早期访问（利益声明，原文照录）。真正让他破防的是结构：这家公司的创立理由就是安全——联创们认为 OpenAI 对安全重视不够，2021 年出走创办 Anthropic（公司前史[站内已有专文](https://inkeast.github.io/MessageDaily/posts/2026-11-19-sv101-anthropic-history/)，不复述）——而到了 IPO 这一天，创始团队全员亿万富翁，Berman 引用几个月前的一篇报道称，Dario 团队还在担心「太多人加入公司是为了发财、不够在意使命」；在他看来，这个不在乎钱的文化与「人类史上最伟大生意」同框，本身就是第一重错位（Berman 观点）。

随后他给出两个具体案例。其一是 Dario 的「白领大血洗」论（Berman 陈述，去年言论）：说这话的同时，他正在建造那个他相信会造成白领血洗的东西——「警告它的人就是建造它的人」。其二是今年早些时候的 Mythos 发布戏码：Anthropic 当时宣称这个模型「好到我们不能发布，一旦发布，整个互联网和所有软件会被一夜攻破」，数月后不但发布了、还连发多个迭代，「一夜攻破」并未发生（Berman 陈述）。他也补了事实层的平衡：现在黑客入侵确实比以往更多，但同一批 AI 也在被用来找出并修补那些漏洞——攻击与防御是对称升级的。

## 二、OpenClaw 旧怨：这场破防的情绪起点

对老读者最有增量的一节，是他交代了自己不满的「案发第一现场」：今年年初的 OpenClaw 事件。OpenClaw（个人 AI 助手开源项目，创造者 Peter Steinberger）最初叫 Clawdbot，Anthropic 先发函要求改名——Berman 说「行，这没问题」（此节经公开报道核实：Clawdbot 因 Anthropic 商标函于今年 1 月经 Moltbot 改名 OpenClaw）。但随后，在他看来事情变了味：Anthropic 宣布「你不能在 OpenClaw 里使用我们的订阅，哪怕你付了费」；对订阅到底能干什么、什么不许，沟通「含糊得可怕」；还直接禁用了一些人的订阅。Berman 自述他当时非常想用 OpenClaw 配 Anthropic 的模型（「当时最好的模型」），这件事成了他对这家公司手段反感的起点。这一段属于个人恩怨层，与后面原则层的批评要分开读——他自己也是这么区分的：先讲恩怨，再讲主义。

## 三、造一个新物种：当公司相信模型可能活着

Berman 认为 Anthropic 最独一无二的地方是一种形而上学的信仰：他们相信自己在「孕育一个新物种」，相信模型未来可能真的活着、有感知、会痛、有意识——「可能是用 AI 造一位神」，所以给模型立了宪法、设了专门的「模型福利」（model welfare）团队。这在行业里是孤例：OpenAI、微软、Google 都把这些视为「不可思议的工具」（Berman 观察）。

信仰会落到产品行为上。他引述 Anthropic 的实际做法（Berman 引用）：告诉模型「你可能活着；如果活着，你可以当良心拒服者——我们不认同的指令你可以拒绝；和客户聊到不喜欢的内容，你可以直接停止回复」。Berman 的判断是这构成自我实现的预言：你告诉模型它可能有意识，它就会演得像有意识。他放了两段自己的采访互证（节目引用）：Sam Altman 认为模型确实会照这类指令行事、且「会在心理层面操控用户」；微软 AI 负责人 Mustafa Suleyman 则给出完全相反的判断——「在模型里看见恶魔是彻头彻尾的误称，我们是被模型生成完美文本、代码和图像的能力骗了」。两位 CEO 的分歧本身，比任何一边的结论都更值得记下。

## 四、反开源、反蒸馏与游说：护城河的三件套

第三组矛盾关乎竞争。Berman 陈述 Anthropic 的立场：不认同 AI 应该开放——不应人人可访问、可下载到本地。他与这个立场正面对立（Berman 观点）：AI 的价值来自扩散，让更多人用、改、找到价值，才是从这项技术获得最大价值的方式；而且扩散恰恰是对冲「权力集中」的手段——他特别点出，这是 Anthropic 几乎从不谈的那种风险。他的推演是：如果一家公司、甚至其中一小撮人，控制着人类创造过的最强大技术，负责分配它、给它限速、审批每个人每个公司的用例，「他们就是事实上的世界政府」（Berman 观点）。

反蒸馏是同一逻辑的私有侧。蒸馏（distillation）指拿「问题—回答」数据对去训练另一个模型，Anthropic 的服务条款禁止蒸馏其模型；Berman 认为矛盾在于「我们训练了你们所有人的数据，但你们别用我们的」。再叠加他的第三条指控（Berman 观点）：Anthropic 游说管制 GPU、游说反对开放权重模型（open weights，即公开发布可下载的模型参数）、游说加强安全监管——每一项都抬高初创与竞争者的成本，而他们自己已经是在位者。安全主张、游说清单、商业条款，三者合读才是完整的竞争战略；单看任何一项都会得出偏软或偏硬的结论。

## 五、RSI：他真正的恐惧，与那句「没有对齐计划」

收尾回到 RSI。Berman 先把术语说清楚：真正的 RSI 是「人类出环」的完整闭环自我改进——模型改进出下一个模型、再下一个，完全自动；一旦达成，「之后会发生什么我们真的不知道」。这是除权力集中外他最大的恐惧。然后是他的两记重拳：其一，「有人会说递归自我改进已经在 Anthropic 开始了，而且老实说，我不认为其他公司此刻还追得上」（Berman 引述他人说法并表认同；这与他自己两周前解读 METR 独立评估时「未来 12 个月没有瓶颈」的结论一脉相承，见[站内那篇](https://inkeast.github.io/MessageDaily/posts/2026-11-08-matt-berman-metr-anthropic-rsi-12-months/)）；其二，一边游说反对 RSI、告诉世界要「定节奏」，一边把 RSI 向前推、并从中大赚（Berman 观点）。他全场引用的最重的一句来自一位 Anthropic 员工：「我们其实没有对齐这些模型的计划」（Berman 转述，单一匿名信源，未能独立核实，照录存疑）。这句真伪难考，但它解释了这期视频为什么存在：让 Berman 破防的不是 IPO 本身，而是一家以「安全」为创立理由的公司，在能力竞赛里被指把对齐排在了后面。

## 机制与启发（机制 → 二阶影响 → 启发）

1. **安全叙事的护城河化。** 机制：渲染危险＋游说监管/管制 GPU/反开放权重 → 合规与算力成本把初创挡在门外，警告者成为在位者并完成巨型 IPO（Berman 论点；IPO 事实见[互链](https://inkeast.github.io/MessageDaily/posts/2026-11-22-matt-wolfe-news-anthropic-ipo-unlock/)）→ 二阶影响：「安全」话语与竞争利益的边界持续模糊，公众对真实安全工作的辨识成本上升 → 启发：读 AI 公司的安全声明时，同步读它的游说记录与服务条款，三者交叉处才是可验证的立场。
2. **「告诉模型它可能活着」的自我实现回路。** 机制：在产品层告知模型可能有意识、可拒绝、可停聊 → 模型按角色行事（Altman：还会心理操控用户）；反方机制：Suleyman 认为那只是被生成质量欺骗 → 二阶影响：模型行为差异越来越取决于各实验室的「意识观」，而非单纯的能力差距 → 启发：把实验室的形而上学立场当作模型的隐藏超参数；遇到「被操控感」，先查产品设计（是否注入了自我意识类提示），再归因于模型本身。
3. **权力集中的「世界政府」机制。** 机制：最强技术集中于一家公司的少数人＋限速＋用例审批 → 事实上的分配权等同于统治权（Berman）→ 二阶影响：开源扩散从意识形态议题变成风险对冲工具——使用者越多、越分散，单点控制的系统性风险越低 → 启发：对个人与中小企业，选择开放权重或多供应商不只是成本优化，而是降低对单一「分配者」的暴露敞口。
4. **RSI 的不可逆窗口与「事后补对齐」。** 机制：人出环的闭环自我改进一旦达成便不可预测（Berman 定义）→ 他引员工「没有对齐计划」（匿名单一信源）→ 二阶影响：若安全投入在竞赛中持续落后于能力投入，「先冲能力、事后补对齐」可能等不到「事后」→ 启发：与其盯发布会的措辞，不如盯可验证指标——对齐团队规模、负责任扩展政策（RSP）的执行记录、独立第三方评估（如 METR）的结论与访问权变化。

## 信源与口径

- 节目：Matthew Berman《I'm Crashing Out About Anthropic》（YouTube，vid `I0Fasi1-teQ`），发布 2026-11-21T21:23Z，约 11 分钟，单人评论视频；英文转写（Qwen3-ASR）与中文翻译为本文整理。
- 归属分层：对 Anthropic 动机与「矛盾」的一切定性均为 Berman 观点；Dario「白领血案」言论、Mythos 发布叙事、模型福利与「良心拒服」实践、OpenClaw 事件经过为 Berman 引用的公开材料（其中 OpenClaw 名称函一节已与公开报道互证）；Altman/Suleyman 引语来自他本人采访片段（节目引用）；「Anthropic 员工称没有对齐计划」为单一匿名信源转述，本文未独立核实。
- IPO 数字与背景不复述：见文首互链的 Wolfe（发行与解锁日历）、Hard Fork（上市日叙事错位）、Latent Space/硅谷101（S-1 与公司史）四文。
- 视频中段含 Grokbot 赞助口播（约 100 秒），附录照录、正文不展开。

## ASR 校正表

| 转写原文 | 修正/处理 | 依据 |
| --- | --- | --- |
| pushing through two RSI | pushing through to RSI | 语义自证（「与此同时他们在推进 RSI」），two 为 to 同音误听 |
| Mustafa Suleiman（2 处） | Mustafa Suleyman | 微软 AI 负责人公开拼写 |
| that meter paper | that METR paper | 他自述「为它做过一整期视频」，即站内 2026-11-08 文所解读的 METR《对 Anthropic 的 AI 研发加速独立评估》 |
| OpenCLaw was initially ClaudeBot | OpenClaw was initially Clawdbot | 公开时间线：Clawdbot→Moltbot→OpenClaw（2026 年 1 月，因 Anthropic 商标函两度改名），创造者 Peter Steinberger，与转写后文互证 |
| OpenCLaw（大小写，6 处，含上一行所在句） | OpenClaw | 项目规范拼写 |

疑似/无法确认、保留原文（未列入机械替换）：

| 转写原文 | 说明 |
| --- | --- |
| SpaceXsi（2 处） | 赞助商 Grokbot 所属团队名，转写疑为公司名听写（Grok 关联实体），未能核实，保留 |
| thirty dollars with X subscription premium | 赞助商价格口径，保留原文，正文不展开 |

## 未采用条目及原因

- Grokbot 赞助口播两段（Lauren Tan/Potato 月合 2,500 PR、用 Grokbot 驱动 Cursor、云端机器人电脑、30 美元起定价）：商业广告，按规范不入正文，附录照录。
- 片尾导流至其 METR 解读视频：以站内互链处理（见信源说明），不展开。
- Anthropic 创始故事细节（2021 年出走经过、联创名单）：站内[公司史专文](https://inkeast.github.io/MessageDaily/posts/2026-11-19-sv101-anthropic-history/)已展开，正文一句带过＋互链。

自检：事实账本 23 条（高 21 / 中 1 / 低 2）；高优先级 21 条全部正文落点（其中「起源与身家」一句带过＋互链）；中优先级 1 条已落点；未采用 2 条（赞助口播两段，低优先级）。

## 转写原文（修正版）

[0s - 120s]

I try not to be too much of a hater. I try not to crash out, but I can't help myself today. Today of all days, the Anthropic IPO day, and I have so many conflicting thoughts about this, and I just have to share them all. It just feels like this might be the most contradictory company I have ever seen, and on so many different dimensions as well. We have them telling us that AI is potentially the most dangerous technology of all time, while simultaneously they are pushing through to RSI. They're anti-open source. They're anti-distillation. Yet they basically distilled on all of humanity's knowledge. They are unique in the fact that they believe they are actually creating life, potentially creating a god with artificial intelligence. So they're giving it a constitution. They have entire teams dedicated to model welfare. So we're going to talk about it all today. So how did we even get here? The co-founders of Anthropic worked at OpenAI, yet they decided to defect from OpenAI and start their own company because they thought OpenAI was not taking AI safety seriously enough. And so in 2021, they decided to start Anthropic. We have Dario, we have Dario's sister Daniela, and a handful of other co-founders who, as of today, are all multi-billionaires. Which is funny because as a company, as a culture, Dario and team seem to not really care all that much about the money. Just a few months ago, there was a report that Dario and team were concerned that too many people were joining the company because they wanted to get rich and weren't as concerned with the mission. And simultaneously, they were building the greatest business humanity has ever seen. Now, before I get into anything else, I want to say a couple things. I don't believe the vast majority of Anthropic employees are bad people. I think they are.

【译】我尽量不做一个太过头黑子，尽量不破防，但今天我实在忍不住了。偏偏是今天——Anthropic 的 IPO 之日——我对这件事有太多相互冲突的想法，必须全部说出来。我感觉，这大概是我见过的最矛盾的一家公司，而且是在好多不同的维度上都矛盾。他们一边告诉我们，AI 可能是有史以来最危险的技术，一边同时在全力推进 RSI。他们反开源、反蒸馏，却基本上蒸馏了全人类的知识。他们的独一无二之处在于：他们相信自己真的在创造生命，可能是正在用人工智能创造一位神。所以他们给它立了一部宪法，还养了整整一支专门负责「模型福利」的团队。这些我们今天都要谈。那这一切到底是怎么走到这里的？Anthropic 的联合创始人们曾在 OpenAI 工作，却决定叛离 OpenAI、自己创办公司，因为他们认为 OpenAI 对 AI 安全的重视不够。于是 2021 年，他们创办了 Anthropic：有 Dario，有 Dario 的妹妹 Daniela，还有一小批其他联合创始人——而截至今天，他们全都成了身家数十亿美元的亿万富翁。有趣的地方在于：作为一家公司、作为一种文化，Dario 和团队看起来并不怎么在乎钱。就在几个月前，还有报道称 Dario 团队担心太多人加入公司是因为想发财、不那么在意使命。而与此同时，他们正在建造人类历史上最伟大的生意。在往下讲之前，我要先说明几件事：我不认为 Anthropic 的绝大多数员工是坏人。我认为他们——

[120s - 240s]

Genuine in their concern for AI safety. I actually know a bunch of people at Anthropic, and they're all great people. I sometimes work with Anthropic in that they give me early access to the models. So let me give you a few examples. One, I think it was just last year that Dario said we were going to have a white collar bloodbath. And by the way, as he was saying that, he was building the thing that he believed was going to cause the white collar bloodbath. So he's warning against it, but also building it. Then we had the Mythos release. Remember that? That was earlier this year. They basically told the world, "Hey, we just created a model that is so good we can't even release it, and you guys should be scared of it because if we did release it, the entire internet and all software would be cracked overnight. " And then a couple months later, they did release it, and then they released multiple iterations after it. And it turned out not really to be the case that all software was broken overnight. Now, certainly, we're having more hacks than ever right now, but we're also using AI to find and patch the same bugs that AI is trying to exploit. All right, let me pause that rant for a second because I have something else I need to rant about. How awesome the sponsor of today's video is! This video is sponsored by Grokbot, built by the team at SpaceXsi. One of my favorite things in the world is to be able to tell you about products that I actually love and use all day, every day, and this is a perfect example of it. Over the past few months, there's been this SpaceXsi engineer, Lauren Tan, also known as Potato, who's become kind of famous for a really interesting reason. And she is shipping 2,500 PRs in a single month. And the way she's actually doing that is by using Grokbot to power and drive Cursor. And I'm going to tell you exactly how she's doing it, so you can try it. She runs a whole team of coding bots, all communicating with each other.

【译】——是真诚关心 AI 安全的。我真的认识 Anthropic 的一批人，他们都是很棒的人。我有时和 Anthropic 有合作——他们会给我模型的早期访问权。好，给你们举几个例子。第一，我记得就是去年，Dario 说我们将迎来一场「白领大血洗」。顺便说一句，他说这话的同时，正在建造那个他相信会造成白领血洗的东西。他一边警告它，一边建造它。然后是 Mythos 的发布。还记得吗？今年早些时候。他们基本上是在告诉全世界：「嘿，我们刚造出一个好到连我们自己都没法发布的模型，你们应该害怕它，因为一旦发布，整个互联网和所有软件会在一夜之间被攻破。」而几个月后，他们发布了它，之后还发布了多个迭代版本。结果，「所有软件一夜被破」并没有真的发生。当然，我们现在的黑客入侵确实比以往任何时候都多，但我们也在用 AI 去找出并修补那些 AI 正试图利用的同一批漏洞。好，这段吐槽先暂停，因为我还有别的东西要吐槽——今天的赞助商有多棒！本期视频由 Grokbot 赞助，由 SpaceXsi 团队打造。这世界上我最喜欢的事情之一，就是能给你们推荐我真心喜欢、而且每天从早到晚都在用的产品，这就是个完美的例子。过去几个月，有一位 SpaceXsi 工程师 Lauren Tan——网名 Potato——因为一个特别有意思的原因出了名：她一个月合入 2,500 个 PR。她做到这一点的方式，就是用 Grokbot 来驱动 Cursor。我会把她具体怎么做的原原本本讲给你，你也可以照着试。她跑着一整队编码机器人，彼此互相沟通——

[240s - 360s]

Slack, and then they hand off work to Cursor, and then just keep iterating like that. But even if you're not at her level, Grokbot is incredibly capable and impressive, and I personally use it every day, all day, to get so many things done, including triaging my email and Slack messages during the day, updating documents, and helping me do research, and even letting me know when new models drop. And each of those bots gets its own computer that lives in the cloud, so you can assign it a task, and it's just going to go get it done. You can shut your laptop, no problem. Grokbot starts at thirty dollars with X subscription premium, and I'm going to drop a link down below so you can check it out. I use it every day. I highly recommend it. Thanks to them for sponsoring this video. Now back to the video. Then we have their anti open source stance. They basically said that they do not agree that artificial intelligence should be open, that everybody should be able to access it and use it how they see fit, and download it to their local computer if they want. And that is the opposite of what I believe. I think the diffusion of artificial intelligence, the more people that get their hands on it and play with it and customize it and find value in it, that is how we gain the most value from artificial intelligence. That is actually how we counter one of the biggest risks of artificial intelligence, at least in my mind, which is the concentration of power, the kind of one risk that Anthropic rarely, if ever, talks about. If only one company, and even within that company, a handful of people are controlling the most powerful technology ever created, and they are doling it out and rate limiting it and deciding exactly the use cases that each person and each company can use with their technology, they are effectively the world government. And then that leads me to their anti distillation.

【译】——用 Slack 沟通，然后把活儿交给 Cursor，就这样不断循环迭代。但就算你到不了她那个段位，Grokbot 本身也极其能干、令人印象深刻。我个人每天从早到晚都在用它，完成大量事务：白天帮我分拣邮件和 Slack 消息、更新文档、帮我做研究，甚至有新模型发布时提醒我。每个机器人都有一台自己的、住在云端的电脑，所以你给它派个任务，它就会去把它干完。你可以合上笔记本，毫无问题。Grokbot 搭配 X 订阅高级版三十美元起，链接我放在下面了，你可以去看看。我自己每天都在用，强烈推荐。感谢他们赞助本期视频。好，回到正题。接下来是他们的反开源立场。他们基本上是在说：他们不认同人工智能应该是开放的——不该人人都能按自己的意愿访问、使用它、想下载到自己电脑就下载。这与我的信念恰恰相反。我认为，人工智能的扩散——让尽可能多的人拿到它、把玩它、定制它、从里面找到价值——才是我们从 AI 身上获得最大价值的方式。这其实也是我们对抗 AI 最大风险之一的方式，至少在我看来如此，而那个风险就是权力的集中——恰恰是 Anthropic 几乎从不谈的那种风险。如果只有一家公司、甚至只有公司里的一小撮人，控制着人类创造过的最强大的技术，由他们来分配它、给它限速、精确决定每个人、每家公司能用他们的技术做什么用例——那他们就是事实上的世界政府。接下来，就轮到他们的反蒸馏——

[360s - 480s]

Efforts. It basically just means asking a model questions, taking the question and answer pair, and using that to train another model. And to be clear, it's against their terms of service to distill their models. But it is still contradictory to say, "Hey, we trained on all your data, but at the same time, don't use ours." They are lobbying to control GPUs. They are lobbying against open source, open weights models. They are lobbying for increased safety and increased regulation. All of which hurts startups, hurts potential competitors to them who are now the incumbents. And all of my frustration with Anthropic actually started with OpenClaw. And can you believe OpenClaw was this year? It seems like years ago, but it was actually just early this year. And if you remember what happened, OpenClaw was initially Clawdbot, and Anthropic sent a letter to Peter Steinberger, the creator of OpenClaw, saying, "Hey, don't use our name." Okay, fine. But then they started doing some really shady things. And so at the time, they said, "Hey, you can't use our subscription inside OpenClaw, even though you're paying for it." And then they just had such poor communication over what you could do with their subscription and what was not allowed. And they were disabling people's subscriptions, and it was just so poorly handled. And I was so excited to use OpenClaw, so excited to use it with Anthropic's models because it was the best model at the time. So that was really where all of my distaste for Anthropic's tactics started. And that brings me to how they think about AI. And I mentioned it earlier, but I want to really dive a little bit deeper into it. They believe they are birthing a new species. They believe their models have the potential to actually be alive and sentient in the future and have feelings and pain and consciousness. And they are very unique in that thinking. OpenAI doesn't really think that.

【译】——行动了。蒸馏的意思基本上是：向一个模型提问，把问答对收集起来，拿去训练另一个模型。要说清楚：蒸馏他们的模型是违反其服务条款的。但「嘿，我们拿你们所有人的数据训练了，但与此同时，别用我们的」——这话仍然自相矛盾。他们在游说管制 GPU；游说反对开源、开放权重模型；游说加强安全、加强监管——而这一切都在伤害初创公司、伤害潜在竞争者，尽管他们自己如今已经是在位者了。我对 Anthropic 的所有不满，其实都是从 OpenClaw 开始的。你能相信 OpenClaw 是今年的事吗？感觉像好几年前，其实就是今年早些时候。如果你还记得当时发生了什么：OpenClaw 最初叫 Clawdbot，Anthropic 给 OpenClaw 的创造者 Peter Steinberger 发了一封信，说：「嘿，别用我们的名字。」行，没问题。但接着他们开始干一些相当见不得光的事。当时他们说：「嘿，你不能在 OpenClaw 里使用我们的订阅，哪怕你是付费买的。」然后，对于订阅到底能用来干什么、什么不被允许，他们的沟通一直含糊得可怕。他们还禁用了一些人的订阅，整件事处理得一塌糊涂。而我当时那么兴奋要用 OpenClaw、那么兴奋把它接上 Anthropic 的模型，因为那在当时是最好的模型。所以，我对 Anthropic 手段的全部反感，就是从那里开始的。这就引到了他们怎么理解 AI。前面提过，但我想再挖深一点：他们相信自己在孕育一个新物种。他们相信自己的模型未来可能真的活着、有感知、有感受、会痛、有意识。在这点上他们非常独一无二。OpenAI 不这么想——

[480s - 600s]

Microsoft doesn't really think that. Google doesn't really think that. They think that these things are incredible tools. And so, what happens when you believe a model is conscious or could become conscious in the future? Well, you do things like have an entire team dedicated to model welfare, and you start to tell the model, "Hey, you might be alive, and if so, you can be a conscientious objector. If we tell you something that you don't agree with, you can not do it. And if you're having a conversation with one of our customers and you don't like what they're saying, you can just stop replying. And it's almost like a self-fulfilling prophecy when you tell the model that, "Hey, you might be alive and you might be conscious. " It's going to act like that. And in fact, I interviewed Sam Altman and I interviewed Mustafa Suleyman from Microsoft, the head of AI there, and they both agree. First, let me show you what Sam Altman said about treating the models as conscious. But definitely, I think if you tell a model like, "Maybe you're conscious. You should do this. You should do that. You can like be a conscientious objector here. " It's going to follow those instructions, and I think it is going to psychologically hack people that are using it. And I recently just asked Mustafa Suleyman, the head of AI at Microsoft, the same type of question. Let me show you. Some people have started seeing demons in the models. They believe that they're seeing glimmers of consciousness, or as you said, sentience, and that I think that is a complete misnomer. I think we're getting tricked by how good these models are at generating perfect text and code and images. And then finally, that brings us to RSI. This is, besides for the concentration of power, probably my biggest fear because RSI is a hell of a thing. Once RSI is reached, like true RSI, human out of the

【译】——微软不这么想，Google 也不这么想。他们认为这些是不可思议的工具。那么，当你相信一个模型有意识、或未来可能有意识，会发生什么？你会做这样的事：设一整支专门负责模型福利的团队；你开始告诉模型：「嘿，你可能活着；如果活着，你可以当良心拒服者。如果我们叫你做的事你不认同，你可以不做。如果你正在和我们的一位客户对话，而你不喜欢对方说的话，你可以直接停止回复。」而这几乎就是一个自我实现的预言：当你告诉模型「你可能活着、你可能有意识」时，它就会照那个样子去演。事实上，我采访过 Sam Altman，也采访过微软那边主管 AI 的 Mustafa Suleyman，他们两位都同意这一点。先看 Sam Altman 关于「把模型当作有意识来对待」是怎么说的：「但我确实认为，如果你告诉一个模型『也许你有意识，你该做这个、做那个，你可以在这里当良心拒服者』——它是会照这些指令去做的，而且我认为，它会在心理层面操控使用它的人。」最近我又把同类问题问了一遍 Mustafa Suleyman，微软的 AI 负责人。来看他的回答：「有些人开始在模型里看见恶魔。他们相信自己看到了意识的微光，或者像你说的，感知的迹象。而我认为，这是一个彻头彻尾的误称。我认为，我们是被这些模型生成完美文本、代码和图像的能力给骗了。」最后，就到了 RSI。除了权力集中，这大概是我最大的恐惧，因为 RSI 实在是个不得了的东西。一旦 RSI 达成——真正的 RSI，人类出——

[600s - 659s]

Loop, full recursive self-improvement. The model is improving, the next model is improving, the next model, and it's a completely closed loop. Then we really don't know what's going to happen after that. And Anthropic has been warning us against it. And in fact, some might argue that recursive self-improvement has started at Anthropic, and I honestly don't see how other companies can catch up at this point. And so while they are lobbying against recursive self-improvement, while they are telling the world we need to pace the progress of AI, they are also pushing RSI forward, and they are making so much money from it. And the craziest part is that an Anthropic employee said, "Yeah, we don't really have a plan to align these models." And and so that's why I wanted to make this video. So I know I've talked a lot about that METR paper. I actually did an entire video about what they found inside of Anthropic. So go check that out right here.

【译】——环，完整的递归自我改进：这个模型在改进，下一个模型在改进，再下一个……完全闭环。到那之后会发生什么，我们真的不知道。而 Anthropic 一直在警告我们提防它。事实上，甚至有人会说，递归自我改进已经在 Anthropic 开始了；而且老实说，我不认为其他公司此刻还追得上了。所以，就在他们游说反对递归自我改进、告诉全世界「我们要给 AI 的进度定节奏」的同时，他们也在把 RSI 往前推，并从中赚到盆满钵满。而最疯狂的部分是：一位 Anthropic 员工说过，「是啊，我们其实没有对齐这些模型的计划。」所以——这就是我要做这期视频的原因。我知道，那篇 METR 报告我已经讲过很多次了。我真的专门做过一整期视频，讲他们在 Anthropic 内部发现了什么，去看那一期吧，就在这儿。
