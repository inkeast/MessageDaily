---
title: "编码模型梯队榜：Matthew Berman 让 Opus 5.5 独占 S 档，把 Gemini 4 Neon 排到 Fable 5.2 之上，Carbon 与 Mistral Large 4 落 F 档"
date: 2026-11-26T18:12:28+08:00
draft: false
tags: ["灰色信源", "Matthew Berman", "评测", "Coding", "Claude", "Gemini", "OpenAI", "开源模型"]
categories: ["podcast-summary"]
summary: "Matthew Berman 这期 14 分钟单人视频把日常用于编码的模型排成梯队：Opus 5.5 独占 S 档，理由是质量、成本与顺手程度的平衡而非单纯质量；Google 的 Gemini 4 Neon 因便宜、好上手被排到 A 档且高于质量更强但额度吃紧的 Fable 5.2；GPT-6.2 Sol 与 Astra、Meta 的 Muse Wonder、DeepSeek V4.1 Flash 同在 B 档；Gemini 4 Carbon 与 Mistral Large 4 进 F 档；DeepSeek V4 Pro、Qwen4-Max、Kimi K3.1 Pro 三家中国旗舰与 GLM-5.3、Grok 4.7 全在 D 档。他把订阅额度、API 成本与自己的实际使用频率都计入排名，因此最好的模型与最常去的模型并不是一回事。全部为主观使用体验，未做基准测试。"
---
> **信源说明**：本文依据 Matthew Berman 频道 2026-11-25 发布的单人视频《RANKING the BEST AI Models for Coding | TIER LIST》（YouTube，vid `iA0ns69dZGM`，838 秒）。英文转写共 7 个时间段（0s–838s），已全部读取；标题所指的「梯队排名」与内容相符。全部排序与理由均为主持人的个人使用体验（未做基准测试、未给分数），本文按「Berman 认为／主持人补充」分层记录、不替其背书；其中「Meta Wonderful」「Mistral Large 4 Lechonk」「MoGraphs」为 ASR 存疑词，处理方式见文末校正表。赞助口播（Eight Sleep 智能床垫）不进正文，附录照译。本站相关记录：The AI Daily Brief 的 [Neon 一期](https://inkeast.github.io/MessageDaily/posts/2026-11-25-ai-daily-brief-gemini-neon-different-not-better/)与 [Astra 6.2 差评潮](https://inkeast.github.io/MessageDaily/posts/2026-11-21-ai-daily-brief-astra-62-backlash/)，Berman 自己的 [DeepSeek V4.1 Flash 实测](https://inkeast.github.io/MessageDaily/posts/2026-10-29-matt-berman-deepseek-v41-flash-crushed/)、[Muse Wonder 首评](https://inkeast.github.io/MessageDaily/posts/2026-10-14-matt-berman-meta-muse-wonder-frontier-benchmarks-quota/)、[Kimi K3.1 完整评测](https://inkeast.github.io/MessageDaily/posts/2026-10-27-matt-berman-kimi-k3-1-full-review/)与 [Fable 5.2 演示](https://inkeast.github.io/MessageDaily/posts/2026-11-10-matt-berman-fable-52-software-dead-demos/)。

## 先说结论

Matthew Berman 这期 14 分钟的视频，把他日常用来写代码的模型排成 S 到 F 六档。结论只有一句：Opus 5.5 独占 S 档，理由不是「质量最高」而是「质量、成本、用起来的愉悦度三者平衡得最好」；公认质量最高的 Fable 5.2 只排在 A 档，而且在同一档里排在 Google 的 Gemini 4 Neon 之后——Neon 不是更高质量，而是更便宜、更顺手，Berman 说自己现在从简单到中等偏难的任务都去 Neon，再往上才找 Opus。GPT-6.2 Sol 与 Astra、Meta 的 Muse Wonder、DeepSeek V4.1 Flash 同在 B 档；Gemini 4 Hydrogen 与 Sonnet 5.5 在 C 档；三家中国实验室的旗舰（DeepSeek V4 Pro、Qwen4-Max、Kimi K3.1 Pro）连同 GLM-5.3、Grok 4.7、Haiku 5.5 全部落在 D 档；Gemini 4 Carbon 和 Mistral Large 4 进 F 档。这份榜单的主轴是「额度与工作流」而不是跑分：他把订阅里能换到多少用量、是否容易上手、自己实际多久用一次，都算进了排名，所以「最好的模型」与「最常去的模型」在他的榜上不是一回事。

## 一、Anthropic：S 档只有一个，额度也是成本

Berman 开场即说 Opus 5.5 到今天仍是他「绝对最喜欢的模型、首选模型」，在质量、成本和愉悦度之间平衡得最好，所以「得进 S 档」。他的解释是信任加效率：虽然已经开始主动使用别的模型，但「最终我最信任的还是 Opus 5.5」，而且它们额度相当慷慨、效率很高，「所以你的 Claude 订阅能换来很多用量」。

Fable 5.2 被他称为「被广泛认为是这颗星球上质量最高的模型」，并承认它在整体质量上胜过 Opus 5.5；但他强调自己衡量一个模型有多个维度——Fable 5.2「非常贵」，不只是走 API 时每百万输入、输出 token 的价格，还包括它在 Claude 订阅里要消耗多少额度（原话为 It uses significantly less than Opus 5.5，按上下文理解为可用量明显少于 Opus 5.5），所以「只在最棘手的问题上才去找 Fable 5.2」，或者拿它检查别的模型做出来的东西。结论是 A 档，「因为它是质量最高的模型」。

同一家的两个小模型被差别对待：Haiku 5.5 进 D 档，因为「相对 Opus 5.5 的质量下降太明显，而用它又省不了多少」，真用也只做最简单的任务；Sonnet 5.5 进 C 档，Berman 称它出来时被宣传为最好的知识工作模型，「在知识工作上确实很好」，但既然 Sonnet 相对 Opus 给你的用量几乎一样，就「很难有理由用 Sonnet，因为 Opus 好太多了」。他还预告了一句后文的伏笔：市面上有比 Sonnet 5.5 更好的轻量模型。

## 二、Google 的翻盘：从「排得很惨」到 Neon 压过 Fable

Berman 说，如果是几周前让他把 Gemini 模型放进梯队榜，他「很可能会把它们排得很惨，很可能把大多数放在 D 档或者更低」——而现在有了 Neon。三个模型各归其位：Hydrogen 被他称为「任劳任怨的主力模型」，放在 C 档并挪到 Sonnet 5.5 前面，「因为它比 Sonnet 5.5 便宜，而且看起来能力也相当」（他同时承认自己其实根本没怎么用过它）；Carbon 进 F 档，这就是 Google「大力宣传的那个大模型」，但他不喜欢用，原因是「不稳定，在较长的任务上很吃力——至少我几周前用的那个版本是这样」，而且慢。

Neon 是这份榜单里最反直觉的一格。Berman 说连 Google 团队自己对这款模型这么好、这么受欢迎都感到意外，「我真的看到过 Google 团队的推文说，我们真没料到会这样，我们不知道自己手里拿的是什么」；Neon「不是 Fable 那种体量或质量的模型」，但用它工作太愉快，更接近 Opus 5.5，「用起来实际上比 Opus 5.5 更令人愉快，只是能力弱一些」。于是他的分工是：从简单到中等偏难的任务去 Neon，再往上去 Opus。综合考虑便宜、易上手、整体强度，他把 Neon 放在 A 档、排在 Fable 5.2 上面——「Opus 5.5 至少眼下仍然是第一，但 Neon 就在它下面一点」，而且 Neon「似乎是从天而降的，所有人之前都没把它当回事」。写作者注：本站记录的 The AI Daily Brief 口径是「Neon 不是更好、而是不一样」、缺点是容易被用户说服，与 Berman 的「更顺手但不如 Fable 能干」互为印证；Carbon 的负面体验他也限于「几周前的那个版本」。

## 三、OpenAI、xAI、Mistral、DeepSeek 与 Meta：窄用例的 B 档

OpenAI 只取最新一代。GPT-6.2 Astra 是「大模型」，Berman 说它出来时「很多人的反应都很负面」；他自己不觉得有那么差，但「远远比不上 Opus 5.5」。他称 Astra 的整体质量也许比 Neon 略好一点（原话 it's maybe slightly better，按上下文指 Astra），「但要拿到那种质量，你得跟这个模型较很大的劲，而通常不值得」，所以放在 B 档，在 Fable 5.2 与 Neon 之下——前者在整体质量上「要好上好几倍」，后者用起来「容易得多」。他补了一句方法论：如果你使劲推它、催它，通常也能得到很不错的结果，「但易用性是我这份梯队榜里计入的因素之一」。

GPT-6.2 Sol 是他「差点放进 A 档」的模型：按他的说法，它是「迄今为止在电脑操作和浏览器控制上最好的模型，尤其是把速度和成本算进来之后」，视频剪辑也很好，而这些是他每天、整天都在做的事；但编码任务他不找 Sol。因为「擅长的用例实在太窄」，最终落在 B 档、排在 Astra 前面、紧跟 Fable 5.2，「我想我喜欢这个模型的程度超过大多数人」。本站记录过 Astra 发布后的差评潮（见信源说明所列日报），Berman 的口径比大众温和，但仍把两位 OpenAI 模型都压在 Neon 之下。

Grok 4.7 进 D 档：Berman 称它擅长的事做得非常好——对话、研究，以及搜索 X、接入 Grokbot 这类能力（他自己加了一句「我知道这不一定反映模型本身」），但「对于编码、剪辑，以及基本上我会拿它去做的所有其他事，它都很差」。Mistral Large 4 进 F 档：他说自己没怎么用过，用过的时候「只是还行」，开源、开放权重可以加一点分，「但这不是这份梯队榜的重点」，不足以改变排名，「很遗憾，这是一个我找不到用例的模型」（转写中另有一个「Lechonk」跟在 Mistral Large 4 后面，疑为版本代号或误识，未核实）。

DeepSeek V4.1 Flash 进 B 档。他提醒观众自己做了一整期视频讲这个模型，理由有三：「是个很棒的模型」「非常便宜」「开源、开放权重」；而它没排在更高的唯一原因是「我用的次数不够多」——他从 Claude、Gemini、ChatGPT 那里得到的额度、token、用量已经够多了。真用它的时候，他说它「真的很擅长编码，很擅长做东西」（转写中此处有「MoGraphs」一词，含义未核实）。

GLM-5.3 进 D 档：刚拿到使用权时他彻底测过，「是个好模型，能力很强，没有任何明显坏掉的地方，但它就是融不进我的工作流」。

Meta 的部分他给了很高的评价。他说 Meta「又是一家用 Muse Wonder 让我们所有人都有点吃惊的公司」，这款模型其实相当不错，用起来的感觉和 Neon 非常像、非常好上手；整体上不如 Neon 能干，也没那么有趣，但「我对 Meta 继续拿出真正出色的模型抱有很高的期望」。Muse Wonder 放在 B 档、排在 GPT-6.2 Sol 的正后方，他坦承「这个位置对我来说很容易翻来覆去」——在 Astra 和 DeepSeek V4.1 Flash 前面，「但它是个 B 档模型。它不是 Neon，也不是 Fable 5.2」。Muse Spark 与 Muse Ember 因为他都没测过，只能进「未测」档；他顺带说 Spark 是开源、开放权重的模型，「这很棒」。

## 四、中国实验室的旗舰全在 D 档，以及他对「第一」的保留

最后是中国 AI 实验室的模型，Berman 把 DeepSeek V4 Pro、Qwen4-Max 和 Kimi K3.1 Pro 称作「中国顶尖 AI 实验室最好的模型」，三家全部落在 D 档。DeepSeek V4 Pro：「另一个非常好的模型，但我不怎么用它，而且它远远比不上外面一些其他的美国闭源模型」，放在 D 档里 GLM-5.3 的上面，「也许除了成本」之外没有任何一点比排在前面的模型更好。Qwen4-Max 同在 D 档，紧跟 DeepSeek、排在 GLM-5.3 前面。Kimi K3.1 Pro 是「在我看来最好的中国 AI 模型」，同样放在 D 档（画面位置音频未说明）。

收尾时他重申 Opus 5.5 仍然是最好的模型，但话锋一转：「紧随其后的那个，我就是喜欢用。它用起来非常直接，能找到一条很直的路径通向解决方案。它和我沟通得非常清楚，清楚地遵循我的指令，能根据『我想要什么』做出很好的假设。」并留了一个条件句——「存在这样一种可能：未来 Neon 会盖过 Opus，这取决于 Google 继续把这个模型推进得怎么样」（按前文「就在它下面一点」的说法，这两句所指应为 Neon）。

## 梯队总表（按主持人给出的先后）

| 档位 | 模型 | 主持人给出的主要理由 |
| --- | --- | --- |
| S | Opus 5.5 | 质量、成本、愉悦度的最佳平衡；最信任；额度慷慨、效率高，绝大多数工作的首选 |
| A | Gemini 4 Neon、Fable 5.2 | Neon：便宜、顺手，简单到中等偏难任务都去它；Fable 5.2：公认质量最高，但贵、订阅内可用量少，只留给最棘手的问题 |
| B | GPT-6.2 Sol、Muse Wonder、GPT-6.2 Astra、DeepSeek V4.1 Flash | Sol：电脑操作与浏览器控制最好、视频剪辑也好，但不用于编码；Muse Wonder：像 Neon 一样好上手但不如它能干，位置「容易翻来覆去」；Astra：质量或许略高但要「较很大的劲」；DeepSeek：编码强、便宜、开放权重，只是用得少 |
| C | Gemini 4 Hydrogen、Sonnet 5.5 | Hydrogen：比 Sonnet 便宜、能力看似相当（本人很少用）；Sonnet：与 Opus 用量几乎相同，难有理由选它 |
| D | Haiku 5.5、Grok 4.7、DeepSeek V4 Pro、Qwen4-Max、Kimi K3.1 Pro、GLM-5.3 | Haiku：质量下降明显、折扣却不多；Grok：对话与研究强、编码与剪辑差；中国三家旗舰与 GLM：都很好但进不了他的工作流（Kimi 被他称为最好的中国模型） |
| F | Gemini 4 Carbon、Mistral Large 4 | Carbon：不稳定、长任务吃力（限几周前的版本）、慢；Mistral：只是「还行」，找不到用例 |
| 未测 | Muse Spark、Muse Ember | 没测过，不排 |

## 机制链与启发

1. **当订阅制把「额度」变成成本，排名就从跑分变成「每次任务的性价比」。** 证据：[0–240s] Fable 5.2 被排到 A 档的理由是「非常贵」与订阅内可用量少，而 Opus 5.5 的 S 档理由包含「额度相当慷慨、效率很高」→ 直接原因是同一订阅下不同模型消耗额度的速度不同 → 高质量高价模型被挤到「只留给最棘手的问题」的小生境，便宜顺手的模型吃掉日常任务 → 二阶影响是模型厂商的竞争焦点从榜单分数移向「单位额度产出」与使用体验（Berman 称 Neon 正是靠后者压过 Fable）→ 观察指标：各家订阅下各模型的额度折算、用户自述的任务分流 → 启发：选模型时先算「我这类任务每天要跑多少次」，再决定要不要为最高质量付溢价。
2. **「窄而强」的模型会被窄用例锁在中间档。** 证据：[360–600s] GPT-6.2 Sol 被他称为电脑操作与浏览器控制「迄今为止最好」、他个人喜欢程度超过大多数人，却因「擅长的用例实在太窄」留在 B 档；DeepSeek V4.1 Flash 质量与价格都好，却因「用的次数不够多」上不去 → 原因是单人工作流里一个模型必须覆盖足够多的日常任务才会被反复调用 → 二阶影响是「专用模型」在个人梯队榜里天然吃亏，除非它嵌入到某个高频入口（浏览器、编辑器）→ 启发：评估专用模型时，别只看它在专长上的领先幅度，还要看它能否接进你现有的任务入口。
3. **工作流粘性可以独立于「模型好坏」存在。** 证据：[600–720s] GLM-5.3「没有任何明显坏掉的地方，但它就是融不进我的工作流」；中国三家旗舰「都很好」却全在 D 档 → 原因是已有额度、习惯与工具链把替换成本抬高 → 模型能力之外的损失由后进者承担，即便产品本身没有缺陷 → 启发：做模型或工具的人，要竞争的不只是能力，还有「迁移成本」这一项；用模型的人则可以定期给自己留一个「换用」的试验窗口，避免把习惯误当成差距。
4. **同一家的梯队内部也会出现「小模型不如别家轻量模型」的错位。** 证据：[120–360s] Sonnet 5.5 被他排在 Gemini 4 Hydrogen 之后，理由是后者更便宜且「看起来能力也相当」→ 原因是轻量档的竞争已经跨厂商 → 二阶影响是「按家族成套使用」的习惯被打破，多厂商混用成为默认 → 启发：不要默认在同一厂商内部按大中小号选模型，轻量档应横向比较。

## 不确定项

- 全部排名与理由为主持人的个人使用体验，无基准分数、无测试样本；Carbon 的负面体验被他限定在「几周前的那个版本」。
- 「It uses significantly less than Opus 5.5」一句指 Fable 5.2 在订阅内的可用量，为按上下文的理解，原话语义有歧义。
- 「it's maybe slightly better in overall quality」按上下文理解为 Astra 略高于 Neon，也可能是反指 Neon。
- 「Lechonk」（跟在 Mistral Large 4 后）与「MoGraphs」为转写原词，含义未核实；「Meta Wonderful model」「Muse Wonderful」按本站既有写法校正为 Muse Wonder（ASR 对该主持人语音的固定误识，10 月 27 日一期同）。
- Kimi K3.1 Pro 在 D 档内的具体位置、Neon 与 Fable 5.2 之外各模型在档内的左右顺序，部分只能从「前面／后面」的口头描述还原，与画面布局可能有出入。
- 收尾处「紧随其后的那个」与「未来盖过 Opus」所指的模型，按前文推断为 Neon。

## ASR 校正表

| 转写原文 | 修正 / 处理 | 依据 |
| --- | --- | --- |
| 6.2 Soul / I'm not going to soul | 6.2 Sol / I'm not going to Sol | OpenAI 模型名，本站既有文章同（10 月 27 日一期同款误识） |
| ahead of Astro | ahead of Astra | OpenAI 模型名 |
| DeepSeek v4.1 Flash / DeepSeek V 4.1 Flash | DeepSeek V4.1 Flash | 模型名写法 |
| Qwen4 Max | Qwen4-Max | 本站既有写法 |
| GLM 5.3 / GLM5.3 | GLM-5.3 | 本站既有写法 |
| Next is carbon / So hydrogen C tier | Next is Carbon / So Hydrogen C tier | Gemini 4 系列名 |
| Meta Wonderful model / Muse Wonderful | Meta Muse Wonder model / Muse Wonder | ASR 对该主持人语音的固定误识（本站 10 月 27 日一期标注同） |
| Mistral Large 4 Lechonk | 保留原文 | 「Lechonk」疑为版本代号或误识，未核实 |
| MoGraphs | 保留原文 | 含义无法从音频确认，正文已标注 |

## 转写原文（修正版）

[0s - 120s]

We now have multiple incredible models, and I wanted to rank all the AI models that I'm using for coding and tell you what I think about them. All right, let's start with the models that I use the most often. These are the Claude models, and so we have Opus 5.5. This is still to this day my absolute favorite model, my go-to model, the best balance of quality, cost, enjoyability to work with. And so, of course, where am I going to put it? It needs to go in the S tier. Opus 5.5 is still the model that I go to for the vast majority of my work. I have started using other models, and I do actively use other models, but ultimately, the one that I have the most trust in is Opus 5.5, and they are quite generous with the quotas, and it is very efficient, so you get a lot of quota from your Claude subscription. All right, next, Fable 5.2, widely regarded as the highest quality model on the planet. Yes, it does beat Opus 5.5 in overall quality, but there are multiple dimensions in which I am personally measuring a model, and although Fable 5.2 is really good, it is very costly, and that's not only if you're using the API, how much you're paying per input and per output token, but also how much quota you use as part of your Claude subscription. It uses significantly less than Opus 5.5, so I find myself only going to Fable 5.2 for the most challenging problems. So where should this fall? And so I'm going to put it in A tier, and that is because it is the highest quality model. Now I don't go to it that often, but when I do go to it, it's for my most important work or when I want to check the work of other models. So Fable 5.2, A tier. Next, Haiku 5.5, and to be honest, I barely use it.

【译】我们现在有很多出色的模型，所以我想给自己用来写代码的所有 AI 模型排个名，并告诉你们我对它们的看法。好，先从我最常用的模型开始。那就是 Claude 系列，我们有 Opus 5.5。直到今天，这仍然是我绝对最喜欢的模型、我的首选模型，在质量、成本、用起来的愉悦度之间平衡得最好。那么它当然要放在哪儿呢？它得进 S 档。Opus 5.5 仍然是我绝大多数工作会去的模型。我已经开始用别的模型，也确实在主动使用别的模型，但最终我最信任的还是 Opus 5.5，而且它们的额度相当慷慨，效率也很高，所以你的 Claude 订阅能换来很多用量。好，接下来，Fable 5.2，被广泛认为是这颗星球上质量最高的模型。是的，在整体质量上它确实胜过 Opus 5.5，但我衡量一个模型有多个维度。Fable 5.2 确实非常好，但它非常贵——不只是说走 API 时每百万输入、输出 token 要付多少钱，还包括你在 Claude 订阅里要消耗多少额度。它比 Opus 5.5 少得多（译者注：原话 It uses significantly less than Opus 5.5，按上下文理解为可用量），所以我发现自己只在最棘手的问题上才去找 Fable 5.2。那它该落在哪儿？我要把它放在 A 档，原因在于它是质量最高的模型。我不

[120s - 240s]

Quality decrease from an Opus 5.5 is so significant, and you're not really getting all that much of a discount by using it. So I barely use it, and when I do use it, it's for the simplest of tasks. So I'm going to go ahead and put it as a D tier. Next is Sonnet 5.5. Now, when this model came out, they touted it as the best knowledge work model, and it is really good at knowledge work. But again, when you're getting compared against Opus 5.5 for you know nearly the same amount of usage that you would get from Sonnet versus Opus, it's hard to justify using Sonnet because Opus is so much better. And so I'm going to land Sonnet 5.5 as C tier. I do use it occasionally, but I find myself going to other models for tasks that maybe I should be going to Sonnet for, but I just don't. But there are better lightweight models out there than Sonnet 5.5, and we're going to get to those later. All right, next we're doing Google models, the Gemini models. And if you had asked me just a few weeks ago to put together a tier list and include Gemini models, I probably would have done them pretty dirty. I probably would have put the majority of the models in D or under. But now we have Neon, and Neon is an incredible model. And you might think you know where it's going to land, but I think it's going to be a little different than you think, and my reasoning might be different than you think. So let's get into it. So first, let's start with Hydrogen. This is their workhorse model. To be honest, I haven't really used Hydrogen at all. Remember when I said that there were better lightweight models than Sonnet 5.5? Well, Hydrogen is it. I'm going to go ahead and still put it in C tier, and I'll actually move it over to the left of it so it's ahead of it, and it's cheaper than Sonnet 5.5, and

【译】……常去找它，但当我去找它的时候，要么是为了最重要的工作，要么是想检查别的模型做出来的东西。所以 Fable 5.2，A 档。接下来，Haiku 5.5，说实话，我几乎不用它。相对 Opus 5.5 的质量下降太明显了，而用它又省不了多少。所以我几乎不用，真用的时候也只是做最简单的任务。那我就直接把它放在 D 档。接下来是 Sonnet 5.5。这个模型出来的时候，他们把它宣传为最好的知识工作模型，它在知识工作上确实很好。但同样地，当你要拿它和 Opus 5.5 比——而 Sonnet 相对 Opus 给你的用量几乎一样——就很难有理由用 Sonnet，因为 Opus 好太多了。所以我把 Sonnet 5.5 定在 C 档。我偶尔会用，但我发现自己会去找别的模型来做那些也许本该找 Sonnet 做的任务，我就是不找它。而且市面上有比 Sonnet 5.5 更好的轻量模型，我们后面会讲到。好，接下来轮到 Google 的模型，Gemini 系列。要是几周前你让我做一份梯队榜并且把 Gemini 模型放进去，我很可能会把它们排得很惨，很可能把大多数模型放在 D 档或者更低。但现在我们有了 Neon，而 Neon 是个非常了不起的模型。你可能以为你知道它会落在哪儿，但我认为结果会和你想的有那么一点不同，我的理由也可能和你想的不一样。那我们开始吧。先从 Hydrogen 说起。这是他们任劳任怨的主力模型。说实话，我

[240s - 360s]

Seemingly just as capable. So Hydrogen C tier. Next is Carbon. This is the model that they really celebrated as that big model smell, but I don't like using it. There's just something about it. It is inconsistent. It struggles on longer tasks. At least the version that I used a couple weeks ago. It is slow, and because of that, I have to put it in the F tier. But now we get to Neon, and even the Google team was surprised by how good this model is and how well received it was. And I actually have seen tweets from the Google team saying we really didn't expect this. We didn't know what we had on our hands, and so we have Neon, which is not a Fable size or quality model, but it is such a pleasure to work with. It is more akin to an Opus 5.5, and it's actually more pleasurable to work with than Opus 5.5, but less capable, right? And so when I'm choosing what to go with on everything from easy all the way up to like medium hard tasks, I'm going to Neon now, and for anything above that, I'm going to Opus. So overall, taking into account how inexpensive it is, how easy it is to work with, and how strong of a model it is overall, I am going to place this in the A tier above Fable 5.2. And so yes, Opus 5.5 is still number one at least for now, but Neon is just below it. And seemingly Neon came out of left field, and everybody was sleeping on it. Like you should sleep on the sponsor of today's video, Eight Sleep. So I try out a lot of tech on this channel, and a lot of it just ends up in my drawer after a day. But the one device that I use every single day for years now is Eight Sleep. The Eight Sleep pod is a smart mattress cover that goes right on top of your existing mattress. It's like running live.

【译】……其实根本没怎么用过 Hydrogen。还记得我说过有比 Sonnet 5.5 更好的轻量模型吗？嗯，就是 Hydrogen。我还是会把它放在 C 档，而且我会把它挪到左边、排在 Sonnet 前面，因为它比 Sonnet 5.5 便宜，而且看起来能力也相当。所以 Hydrogen，C 档。接下来是 Carbon。这就是他们大力宣传的那个「大模型」范儿的模型，但我不喜欢用它。它就是有点说不上来的问题。它不稳定，在较长的任务上很吃力——至少我几周前用的那个版本是这样。它还慢，所以因为它，我不得不把它放进 F 档。但现在我们说到 Neon，就连 Google 团队自己都对这款模型这么好、这么受欢迎感到意外。我真的看到过 Google 团队的推文说，我们真没料到会这样，我们不知道自己手里拿的是什么。于是我们有了 Neon——它不是 Fable 那种体量或质量的模型，但用它工作真是太愉快了。它更接近 Opus 5.5，而且用起来实际上比 Opus 5.5 更令人愉快，只是能力弱一些，对吧？所以现在当我选择用什么的时候，从简单的一直到中等偏难的任务，我都去 Neon；再往上的，我去 Opus。所以总体来看，考虑到它多便宜、多容易上手、整体又多强，我要把它放在 A 档，排在 Fable 5.2 上面。所以是的，Opus 5.5 至少眼下仍然是第一，但 Neon 就在它下面一点。而且 Neon 似乎是从天而降的，所有人之前都没把它当回事。就像你们应该「睡」在今天视频的赞助商 Eight Sleep 上面一样。（口播）这个频道上我试过很多科技产品，其中很多用了一天就进了抽屉。但有一个设备我每天都在用，已经用了好几年，那就是 Eight Sleep。Eight Sleep 的 Pod 是一个智能床垫罩，直接铺在你现有的床垫上。它就像整晚对你的

[360s - 480s]

Inference on your biometrics all night. It reads your heart rate, HRV, temperature, and movements, then automatically adjusts the temperature of your bed so you get the best possible sleep. You don't have to check a new app or build a new habit either. It just works in the background. Users report up to 34% more deep sleep, and once you're used to it, you can't live without it. When I'm on vacation, I miss my eight sleep. And for Black Friday coming up, you can get up to $700 off the new Pod 6 and Base through November 30th with code Matthew Berman. I'm going to drop a link down below. Go get it. I could not recommend it more. And thanks again to Eight Sleep. All right. So next we're going with the OpenAI models, and today we're only using their latest generation models for this tier ranking. And so I have 6.2 Astra and 6.2 Sol. Let's start with 6.2 Astra. This is their big model, the one where a lot of people had a lot of negative reactions when it came out. I used it. I didn't think it was that bad, but it certainly was far from Opus 5.5. And now that Neon has come out, it's maybe slightly better in overall quality. But to get that quality, you have to work really hard with that model, and it's usually not worth it. I am generally not using 6.2 Astra at all. And so where I rank it is B tier. That is under Fable 5.2, which is multiple times better in terms of overall quality, and under Neon, which is just so much easier to work with overall. Now again, if you just stick with 6.2 Astra and you really push it and prod it to get what you want, you can usually get a really good result. But again, like the ease of use is part of what I'm factoring in in this tier list. So 6.2 Astra B tier. Now we have 6.2 Sol, and this is an interesting model. It's actually quite good. It is

【译】（口播）……生物指标做实时推理。它读取你的心率、HRV、体温和体动，然后自动调节床的温度，让你得到尽可能好的睡眠。你也不必去查看一个新应用或者养成一个新习惯。它就在后台默默工作。用户报告深度睡眠最多增加 34%，而一旦习惯了，你就离不开它了。我度假的时候会想念我的 Eight Sleep。即将到来的黑色星期五，新款 Pod 6 和 Base 最多可以优惠 700 美元，11 月 30 日前用优惠码 Matthew Berman。我会把链接放在下面。去买吧。我极力推荐。再次感谢 Eight Sleep。好。接下来是 OpenAI 的模型，今天这份梯队榜里我们只用他们最新一代的模型。所以我有 6.2 Astra 和 6.2 Sol。先说 6.2 Astra。这是他们的大模型，出来的时候很多人的反应都很负面。我用过。我不觉得它有那么差，但它确实远远比不上 Opus 5.5。而现在 Neon 出来之后，它（译者注：按上下文指 Astra）在整体质量上也许还略好一点。但要拿到那种质量，你得跟这个模型较很大的劲，而这通常不值得。我一般根本不用 6.2 Astra。所以我把它排在 B 档。那是在 Fable 5.2 之下——后者在整体质量上要好上好几倍——也在 Neon 之下，后者整体上用起来要容易得多。再说一遍，如果你就守着 6.2 Astra，使劲推它、催它去拿到你想要的东西，你通常能得到一个很不错的结果。但是，易用性是我这份梯队榜里计入的因素之一。所以 6.2 Astra，B 档。现在我们说 6.2 Sol，这是个有意思的模型。它其实相当不错。它是

[480s - 600s]

By far the best model at computer use and browser control, and especially when you factor in speed and cost, it's also really good at video editing. And these are all things that I do all day, every day. But for coding tasks, I'm not going to Sol. But for those other tasks, it's incredible. So where would I rank it? I'm putting it in B tier, and I was a little bit torn here. I almost put it in A tier behind Fable 5.2, but I'm going to end up putting it in B tier ahead of Astra, right behind Fable 5.2. And I think I like this model more than most people do. But it is so narrow in the use cases that it's great at that I just couldn't bump it up to A tier. All right, now we have Grok 4.7, and it's again, it's one of those models that the things that it's good at, it's really good, and then everything else, it's quite bad. And so for conversation and research, and especially, and I know this isn't necessarily a reflection of the model, but its ability to search X and plug into Grokbot and all these other things, it's a really good model. But for coding and for editing and for basically everything else that I would use it for, it's really bad. So I'm going to have to put it down at D tier. All right, next we have Mistral Large 4 Lechonk, and I honestly haven't used this model a ton. But when I have used it, it was just okay. Of course, credit where credits due. It's an open source, open weights model, but that's not the point of this tier list. So I can only give them a little bit of extra credit for that, but not enough to really move their ranking. And so for Mistral Large 4, I'm also going to put it in the F tier. It is unfortunately a model that I cannot find a use case for. All right, next, DeepSeek V4.1 Flash. Now I made an entire video about this model. You probably know that I do really like this model, but again, it's one of those situations.

【译】……迄今为止在电脑操作和浏览器控制上最好的模型，尤其是把速度和成本算进来之后；它在视频剪辑上也真的很不错。而这些全都是我每天、整天都在做的事。但对于编码任务，我不会去找 Sol。而对那些别的任务，它好得惊人。那我把它排在哪儿？我把它放在 B 档，这里我有点纠结。我差点把它放进 A 档、排在 Fable 5.2 后面，但最后我还是把它放在 B 档、排在 Astra 前面，紧跟在 Fable 5.2 之后。我想我喜欢这个模型的程度超过大多数人。但它擅长的用例实在太窄，所以我没法把它提到 A 档。好，现在我们说 Grok 4.7，它又是那种模型——它擅长的事，它做得非常好，而其他一切，它都相当差。所以在对话和研究上——尤其是，我知道这不一定反映模型本身——它搜索 X、接入 Grokbot 以及所有这些其他东西的能力，它是个非常好的模型。但对于编码、剪辑，以及基本上我会拿它去做的所有其他事，它都很差。所以我只能把它放到 D 档。好，接下来是 Mistral Large 4 Lechonk（转写原文如此），说实话我没怎么用过这个模型。但用过的时候，它只是「还行」。当然，该给的功劳要给：它是开源、开放权重的模型，但这不是这份梯队榜的重点。所以我只能为此给他们加一点点分，但不足以真正改变他们的排名。所以 Mistral Large 4，我也要把它放进 F 档。很遗憾，这是一个我找不到用例的模型。好，接下来，DeepSeek V4.1 Flash。我做了一整期视频讲这个模型。你们大概知道我确实很喜欢它，但同样，它属于那种情况。

[600s - 720s]

In which I think I have enough quota, enough tokens, enough usage from Claude and Gemini and ChatGPT, where I don't really find myself going to this model. However, when I do use the model, it's really good at coding. It's really good at building things, MoGraphs, and so I'm going to go ahead and put this model in the B tier. And so there's a few reasons I'm choosing B tier. One, it's a great model. Two, it's very inexpensive. Three, open source, open weights, and the only reason it's not higher is because I just don't use it all that often. Next, GLM-5.3. I honestly don't use this model very often. I did test it thoroughly when I first got access to it, and you know, it's a good model. It's very capable. There's nothing like directly broken about it, but it just doesn't fit into my workflow. So I'm going to give it the D tier. All right, next, let's go to the Meta models, and Meta is another one of those companies that just kind of surprised us all with the Meta Muse Wonder model, which actually was quite good. It felt very similar to Neon in that it was very easy to work with. Now, overall, it's not as capable as Neon. It's not as fun to work with, but it is a very good model, and I have high hopes that Meta will continue to deliver really great models. Now, for Muse Wonder, I'm going to put it in the B tier, and I'm going to put it right behind 6.2 Sol. And I struggled with this a little bit. This could flip flop pretty easily for me. Ahead of 6.2 Astra, ahead of DeepSeek V4.1 Flash, but it is a B tier model. It is not Neon. It is not Fable 5.2. Then we have Muse Spark and Muse Ember, and unfortunately, I haven't tested either of them. They are both smaller models. Spark is an open source, open weights model, which is wonderful. But since I haven't tested them, I have to place both of them.

【译】……就是我觉得我从 Claude、Gemini 和 ChatGPT 那里得到的额度、token、用量已经够多了，以至于我并不怎么去找这个模型。不过，当我真用它的时候，它真的很擅长编码。它很擅长做东西、做 MoGraphs（转写原文如此），所以我就把这个模型放在 B 档。我选 B 档有几个理由。第一，它是个很棒的模型。第二，它非常便宜。第三，开源、开放权重。而它没排在更高的唯一原因，就是我用的次数不够多。接下来，GLM-5.3。说实话我不怎么用这个模型。刚拿到使用权的时候我确实彻底测过它，它是个好模型，能力很强，没有任何明显坏掉的地方，但它就是融不进我的工作流。所以我给它 D 档。好，接下来我们说 Meta 的模型。Meta 又是一家用 Meta Muse Wonder 模型（译者注：转写作 Meta Wonderful，疑为误识）让我们所有人都有点吃惊的公司，它其实相当不错。它用起来的感觉和 Neon 非常像，非常好上手。整体上它不如 Neon 能干，用起来也没那么有趣，但它是个非常好的模型，我对 Meta 继续拿出真正出色的模型抱有很高的期望。至于 Muse Wonder，我要把它放在 B 档，排在 6.2 Sol 的正后方。这里我有点纠结，这个位置对我来说很容易翻来覆去。在 6.2 Astra 前面，在 DeepSeek V4.1 Flash 前面，但它是个 B 档模型。它不是 Neon，也不是 Fable 5.2。然后是 Muse Spark 和 Muse Ember，很遗憾，这两个我都没测过。它们都是较小的模型。Spark 是开源、开放权重的模型，这很棒。但因为我没测过，我只能把这两个都放在

[720s - 838s]

In the untested tier. All right, last we have the models from the Chinese AI labs. We have DeepSeek V4 Pro, we have Qwen4-Max, and Kimi K3.1 Pro. These are all the best models from the top AI Chinese labs. So first, DeepSeek V4 Pro. It's another very good model, but I don't use it all that often, and it's not nearly as good as some of the other American closed source models out there. So I'm going to go ahead and put that above GLM-5.3 in the D tier. It's a fine model. There's just nothing about it that makes it better than any of the other models ahead of it. Maybe with the exception of cost. Next, Qwen4-Max. I'm going to go ahead and put that also in the D tier, right behind DeepSeek, right ahead of GLM-5.3, and then Kimi K3.1 Pro is in my opinion the best Chinese AI model, and so I'm also going to put that in D tier right there. And so that is my ranking. Sitting by itself, S tier, Opus 5.5. It still is the best model, but I will tell you, right behind it, I just love using it. It's so direct in its usage. It finds a very straight line path to solutions. It communicates with me very clearly. It follows my instructions clearly. It makes assumptions really well based on what I think I would have wanted. And so there is a world in which Neon outshines Opus in the future, depending on how Google continues to progress that model. And so this is my ranking. What do you think? Let me know in the comments. And if you haven't tried this model yet and don't understand why so many people, including myself, are saying such incredible things about it, go watch my full review right here.

【译】……「未测」档。好，最后是中国 AI 实验室的模型。我们有 DeepSeek V4 Pro、Qwen4-Max 和 Kimi K3.1 Pro。这些都是中国顶尖 AI 实验室最好的模型。先说 DeepSeek V4 Pro。这是另一个非常好的模型，但我不怎么用它，而且它远远比不上外面一些其他的美国闭源模型。所以我就把它放在 D 档里 GLM-5.3 的上面。它是个不错的模型，只是没有任何一点让它比排在它前面的任何一个模型更好——也许除了成本。接下来，Qwen4-Max。我也把它放在 D 档，紧跟在 DeepSeek 后面、排在 GLM-5.3 前面。然后 Kimi K3.1 Pro，在我看来是最好的中国 AI 模型，所以我也把它放在 D 档的那个位置。这就是我的排名。独自坐在 S 档的，是 Opus 5.5。它仍然是最好的模型。但我要告诉你们，紧随其后的那个，我就是喜欢用。它用起来非常直接，能找到一条很直的路径通向解决方案。它和我沟通得非常清楚，清楚地遵循我的指令，能根据「我想要什么」做出很好的假设。所以存在这样一种可能：未来 Neon 会盖过 Opus——这取决于 Google 继续把这个模型推进得怎么样。这就是我的排名。你们怎么看？在评论区告诉我。如果你还没试过这个模型，也不明白为什么这么多人（包括我）都在说它的好话，去看看我做的那期完整评测吧，就在这里。
