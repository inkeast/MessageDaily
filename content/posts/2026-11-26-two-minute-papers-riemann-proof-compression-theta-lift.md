---
title: "没人读完的证明值多少钱：Two Minute Papers 追问黎曼假设证明的压缩进度，并用「称一叠纸」讲 theta 提升"
date: 2026-11-26T18:04:03+08:00
draft: false
tags: ["灰色信源", "Two Minute Papers", "Anthropic", "Claude", "数学", "形式化验证"]
categories: ["podcast-summary"]
summary: "Two Minute Papers 这期 8 分钟视频是对 Claude 参与证明黎曼假设的第二层追问。讲者称陶哲轩此前判断需要超越现有 AI 的新东西、也许 2050 年才有解，35 天后解答就到了，Lean 检查已由多个独立系统确认正确；但 1700 万行没人完整读懂，AI 自动研究循环在压缩证明，他先说部分片段缩了 90% 以上、整体几乎没有变化，随后补充称另一项目已在整个证明上实现有意义的压缩。他的判断是「知道为真」本身不是游戏规则的改变者，关键是证明教了我们什么，并用「一叠纸放上体重秤」的比喻解释 theta 提升——新意在于把五十多年前的技术串联足够多次并想清楚叠多高。全部为讲者陈述与其自称的学生直觉，未经独立核实。"
---
> **信源说明**：本文依据 Two Minute Papers 频道 2026-11-25 发布的视频《Claude AI's Biggest Math Breakthrough Has A Problem.》（YouTube，vid `hwWlEuI359w`，487 秒）。英文转写共 5 个时间段（0s–487s），已全部读取；讲者自报为 Károly Zsolnai-Fehér 博士（ASR 误识，已按公开姓名校正）。标题里的「问题」指证明太长、没人读得懂，并不是指证明有错；Lean 检查、自动压缩、theta 提升与研究者名单均为讲者陈述，他两次自称「只是个学生」，本文按节目口径整理、未经独立核实。本站对同一事件已有记录（[Matthew Berman 一期](https://inkeast.github.io/MessageDaily/posts/2026-11-25-matt-berman-ai-solve-mathematics/)、[Wes Roth 一期](https://inkeast.github.io/MessageDaily/posts/2026-11-25-wes-roth-openai-perplexity-intelligence-thresholds/)、[11 月 21 日](https://inkeast.github.io/MessageDaily/posts/2026-11-21-daily-ai-tracking/)与[11 月 22 日](https://inkeast.github.io/MessageDaily/posts/2026-11-22-daily-ai-tracking/)日报），背景一句带过，只展开这期新增的追问与直觉。

## 先说结论

Two Minute Papers 这期 8 分钟的视频，是对「Claude 参与证明黎曼假设」的第二层追问。讲者先复述事件：黎曼假设——一个关于素数分布的猜想——悬置一百六十多年，数学家 David Hilbert（希尔伯特）说自己睡一千年醒来第一个问题就是「它被证明了吗」，Terence Tao（陶哲轩）不久前还说需要超越现有 AI 的东西、也许 2050 年前后才有解；讲者称仅仅 35 天后，解答就由 Claude 与一位来自 Anthropic 和斯坦福的研究者给出，体量是 1700 万行代码。按讲者的说法，Lean 形式化检查已经跑完、多个独立验证系统确认证明正确，「这本身已是历史」；但据他所知没有人完整理解整个论证，AI 的自动研究循环正在压缩证明——有些片段缩小了 90% 以上，整体却「到目前为止几乎没有变化」（他在第二段补充：另一个项目已在整个证明上实现有意义的压缩）。讲者的核心判断是：「知道它为真」本身不是游戏规则的改变者，因为数学家早已在假设它成立的前提下继续工作，真正值钱的是证明本身教了我们什么。他用「把一叠纸放上体重秤」的比喻讲自己的直觉——新证明的一大跨越，是把五十多年前就有的 theta 提升串联足够多次，并且想清楚「这叠纸要叠多高」。

## 一、从「2050」到「35 天之后」：机器确认了，人没读完

讲者的开场是「这是正在书写的历史」。他先把题目说清：素数是数学和密码学的基石，看上去近乎随机出现，但人们有一个公式，能给出直到某个点为止应当有多少个素数；黎曼假设说的是这个公式精确得不可思议。「你可以只是说说而已，但要证明它，呼，那几乎是不可能的。」一百六十多年里无人解决，他引了两位数学家：Hilbert 的「千年一问」，以及 Tao 的判断——需要超越当前 AI 的东西，也许 2050 年前后才有解答。「而我震惊地发现，仅仅 35 天之后，解答就到来了。」讲者称解答由 Claude AI 与一位 Anthropic 和斯坦福的研究者完成，共 1700 万行代码；「据我所知，还没有人类完整理解整个论证」。

接下来是他给出的现状：解答被形式化在 Lean 里（讲者的解释是「一种可以被形式化检查的数学语言」），检查已经完成，多个独立的验证系统一致认定证明正确——「这本身已经是历史性的」。但「现在呢」的答案分两支：一支是 AI 系统正在跑的自动研究循环，目标是把证明压缩得更优雅、希望也更易理解；讲者称有些片段缩小了 90% 以上，「但那些只是片段。看整个证明的话，到目前为止它几乎没有动」。

口径提醒（写作者注）：「35 天」是讲者从 Tao 的发言算起的说法；本站 Wes Roth 一期记录的是主持人口径「一场 41–42 天的冲刺、成果因 IPO 推迟数日发布」，Matthew Berman 一期记录的是「11 月 20 日以 1700 万行 Lean 代码库形式发布、次日 Lean FRO 与 Mathlib Initiative 确认」，三个口径的起点不同，本文并列、不替节目调和。做出现实成果的研究者，本站此前记录为 Anthropic 数学家 Levent Alpöge；讲者这次说的「Anthropic 和斯坦福」里的斯坦福身份，本文未核实。

## 二、「知道为真」为什么不是 game changer：问题从定理转向证明

第二段开头是一个「更新」：讲者说发视频前等一等是对的，不要立刻发布，因为他找到了另一个已经在整个证明上实现有意义压缩的项目——这是对第一段「整体几乎没动」的自我修正。

他真正的问题是：一个我们不理解的黎曼假设证明有多大价值，它是不是真正的游戏规则改变者（game changer）？他的回答是，仅仅「现在知道它为真」不是：「因为你可以继续做你的数学，假设它为真，然后该干嘛干嘛。很多人的研究里本来就是这么做的。」所以「也许比定理本身更重要的，是证明本身教给了我们什么。如果这证明是一团意大利面，我们在这里学到了什么有用的东西吗？」他补了一句观察：围绕这件事的讨论很多，但谈的是一切，唯独不谈证明本身；「在互联网的这个角落，我们不回避科学部分」。

然后是两次自我降级里的第一次：「我只是一个学生，我不够聪明，没法理解整个解答，但我会尽力给你们讲讲我的那点小直觉。」这句话决定了下一节的性质——那是讲者的理解，不是证明作者的解释。

## 三、称一叠纸与 theta 提升：讲者的「学生直觉」

直觉从素数计数讲起：到 10 为止有 4 个素数，到 100 为止有 25 个；对任意 x，现在已有相当不错的技术做估计，这个估计给出一个地板和一个天花板，通常彼此很接近。问题在于，我们要的是对任意 x 都成立的证明，而「当 x 变得非常大时，地板和天花板会漂移得非常远，几乎有 x 本身那么多位数」——界太松、搜索区域太大。他的比喻是找车钥匙：有人说「我知道它们在哪儿」，追问之下答案是「在宇宙里的某个地方」。

怎么把天花板压下来？他用称纸作比：一张纸太轻，浴室体重秤量不出来；把一百张叠在一起称，再除以一百，就得到一张纸的重量。数学家没法把素数一个一个地「称」，就需要把它们放在一起称、把误差缩下去，也就是「需要每个素数都在公式里出现很多次」。讲者说这是个绝妙的主意，「只除了一点——它不可能做到」：如果把这个公式对全体整数一遍遍叠加，误差也在同时增大，随时间爆炸，「想象每加一张新纸，秤就变得更晃一点」。

theta 提升（theta lift）在这时登场——讲者称之为「一种能让秤保持稳定的技术」：每多加一张纸，这叠纸变高，但误差仍受控。他据此给出对新证明的解读：「一大跨越，就是弄清楚这叠纸要叠多高、要用多少次提升。」讲者称他们发现这样做「x 的平方根倍」就能把地板和天花板挤得更近、得到精确结果（转写作 the square root of x times，含义未核实）。他特意强调 theta 提升本身不新，是五十多年前发明的，「但把它多次串联在一起、并产生巨大效果的方式，那部分我认为是新颖的」。

然后是第二次自我降级：「我只是一个学生，我可能是错的，但那就是我的小直觉。」对「我们学到了东西吗」这个问题，他的回答是肯定的：是的，这里有大量可学的东西。

## 四、「接下来」：研究者名单、P = NP 与开放权重

讲者给出两件「现在怎么办」。第一，他说自己不是分配功劳的权威，但频道要庆祝伟大的科学家，于是放出一份「对黎曼假设解答的主要里程碑做出贡献的研究者名单」——名单只在画面上，音频没有念出名字，本文无从转述。第二，「已经有人在试着让一群 AI 智能体去解决另一个百万美元的千禧年难题——P 等于 NP。那是我最喜欢的一个。」他称这是「公开进行的科学」（science building in public），旁人可以看到他们在尝试什么、做得怎么样；他的保留意见是，「一个真正的解答可能需要更多的方向指引，但谁知道呢」。

收尾是他期待的未来：拿到新工具，去学习和理解更多关于世界的东西、发明新事物、改善人们的生活；而且希望未来有更多这样的成果——「不是用闭源模型，而是用开放权重模型、公开构建，任何人都可以贡献」。写作者注：本站对 P versus NP 的既有记录是 OpenAI 十月末「证伪了与 P vs NP 深度相关的指数时间假设（ETH）」，OpenAI 总裁 Greg Brockman 当时的表态是「P 大概率仍然不等于 NP」（见 [Wes Roth 10 月 28 日一期](https://inkeast.github.io/MessageDaily/posts/2026-10-28-wes-roth-openai-eth-disproof/)）；讲者这里说的是一群智能体正在尝试，是进行中的项目，不是已有的结果。

## 机制链与启发

1. **「正确」已经可以规模化，「理解」还不能。** 证据：[0–120s] 的「Lean 检查完成、多个独立系统确认」对比「没有人完整理解整个论证」→ 直接原因是机器检查能处理 1700 万行，而人的阅读不能 → 以结论为原材料的数学工作者几乎没有增量收益（他们本来就在假设它成立），以理解为业的研究者与教师承接全部压力 → 二阶影响是「证明压缩」从副业变成一项独立的研究工作，由自动研究循环承担 → 观察指标：整体压缩率、是否出现人类可读的讲解稿及其篇幅 → 启发：把「压缩与讲解」当作交付物的一部分，而不是事后补充。
2. **新意常在「用量与配比」，而不是「新工具」。** 证据：[240–360s] 讲者称 theta 提升已有五十多年历史，新的是「叠多高」——串联多少次 → 直接原因是要压住的是误差随叠加而爆炸的机制，所以关键变量是次数 → 掌握旧工具的人获得杠杆，迷信「必须等新方法」的人低估手头手段 → 二阶影响是研究品味的偏移：从寻找新对象转向寻找新的组合方式 → 启发：遇到「老办法做不动」的问题，先问是不是用量、次数或配比没找对。这与 Berman 一期「不是新数学，而是把现有工具串成长链」的判断互为印证（两期出自不同讲者，均非证明作者）。
3. **一个结论的边际价值，取决于此前有多少工作已把它当前提。** 证据：[120–240s] 的「很多人本来就在假设它成立的前提下做研究」→ 原因是黎曼假设被大量定理当作前提，证明带来的直接后果早已被「预支」→ 那么证明的价值主要落在副产品：新技术、新理解、以及对机器验证信任的一次演示 → 启发：评估一项突破时，先数一数有多少工作已经把它当作既定条件，再判断它的增量在哪里。
4. **「公开构建」让外部方向成为可能。** 证据：[360–480s] 的 P = NP 多智能体尝试与「开放权重、任何人都能贡献」的期待（讲者推测，非既成事实）→ 原因是讲者认为真解「需要更多方向指引」，而过程可见才谈得上外部指引 → 二阶影响是「谁能贡献方向」成为开放与闭源模型之争里的具体论据 → 启发：做长链任务时，把过程日志当成可被外部审阅的接口来设计。

## 不确定项

- 「部分片段缩小 90% 以上、整体几乎没有变化」与「另一个项目已在整个证明上实现有意义的压缩」均为讲者陈述，音频未给出项目名称与数据；两者之间的关系（是否是同一批工作）讲者未说明。
- 「x 的平方根倍」（转写作 the square root of x times）是讲者口述的直觉，其在原证明中的确切含义本文未核实。
- 做出成果的研究者身份，讲者称「Anthropic 和斯坦福的研究者」；本站此前记录为 Anthropic 数学家 Levent Alpöge，「斯坦福」身份未核实。研究者名单只在画面上，音频未念出。
- Hilbert 与 Tao 的话均为讲者转述，本文未核对原始出处；「35 天」从 Tao 的发言算起，与本站 Wes Roth 一期「41–42 天冲刺」的口径起点不同。
- 口播段（Lambda 云 GPU）已识别，不进正文，附录照译。

## ASR 校正表

| 转写原文 | 修正 / 处理 | 依据 |
| --- | --- | --- |
| Dr. Karol Jonah Ifahir | Dr. Károly Zsolnai-Fehér | 讲者公开姓名（频道自述；本站既有文章同） |
| This is two minute papers with | This is Two Minute Papers with | 频道官方名称 |
| the square root of x times | 保留原文 | 讲者口述直觉，含义无法从音频确认，正文已标注 |
| researcher at Anthropic and Stanford | 保留原文 | 本站此前记录为 Anthropic 数学家；「斯坦福」身份未核实 |
| theta lift / Lean / P equals NP | 保留 | 与通行术语一致 |

## 转写原文（修正版）

[0s - 120s]

This is history in the making. I cannot believe what just happened. The Riemann Hypothesis is a conjecture about the distribution of prime numbers. These are the building blocks of mathematics and cryptography. They seem to appear almost randomly, but we have a formula that shows how many prime numbers there should be up until a given point. This might be the universe's secret pattern, and the Riemann Hypothesis says that this formula is incredibly accurate. This is all well and good. You can just say things, but proving that, whoo, that's nearly impossible. For over 160 years, no one was able to solve it. Legendary mathematician David Hilbert said, "If I were to awaken after having slept for a thousand years, my first question would be, has the Riemann Hypothesis been proven?" Another legendary mathematician, Terence Tao, said something beyond current AI is needed to solve the Riemann Hypothesis, and maybe around 2050 we would have some solution. And I was stunned to find out that just 35 days later, the solution arrived by Claude AI and researcher at Anthropic and Stanford. It is 17 million lines of code. To the best of my knowledge, no human has fully understood the entire argument. So, what now? Well, one, the solution was formalized in Lean, a mathematical language that can be formally checked. The check has finished, and multiple independent verification systems agreed that the proof is correct. That is already historic. But there are AI systems currently working on auto research loops to compress the proof down to something more elegant, hopefully something more comprehensible. Some pieces shrank over 90 percent, but those are just pieces. Looking at the whole proof, it barely moved so far.

【译】这是正在书写的历史。我不敢相信刚刚发生的事。黎曼假设（Riemann Hypothesis）是一个关于素数分布的猜想。素数是数学和密码学的基石。它们看起来几乎是随机出现的，但我们有一个公式，能给出直到某个给定点为止应当有多少个素数。这也许是宇宙的秘密图案，而黎曼假设说，这个公式精确得不可思议。这一切都很好。你可以只是说说而已，但要证明它，呼，那几乎是不可能的。一百六十多年来，没有人能解决它。传奇数学家 David Hilbert（大卫·希尔伯特）说过：「如果我在沉睡一千年之后醒来，我的第一个问题会是：黎曼假设被证明了吗？」另一位传奇数学家 Terence Tao（陶哲轩）说，解决黎曼假设需要超越当前 AI 的东西，也许到 2050 年前后我们才会有某种解答。而我震惊地发现，仅仅 35 天之后，解答就到来了——由 Claude AI 和一位来自 Anthropic 与斯坦福的研究者完成。它是 1700 万行代码。据我所知，还没有人类完整理解整个论证。那么，现在呢？第一，这个解答被形式化在 Lean 里——一种可以被形式化检查的数学语言。检查已经完成，多个独立的验证系统一致认定证明是正确的。这本身已经是历史性的。但目前已经有 AI 系统在

[120s - 240s]

Update. It's good to wait with a video for a bit instead of publishing immediately because I found another project that already has meaningful compression on the whole project. Awesome. Now my question is: How valuable is a proof for the Riemann hypothesis that we don't understand, and is it being a true game changer? Now, just the fact that we now know is true is not a game changer in and of itself. Why? Well, because you could continue your mathematics assuming it is true and just go about your day. And a lot of people did that in their research. So in this case, I think perhaps even more important than the theorem is what the proof itself teaches us. If the proof is a bunch of spaghetti, did we learn anything useful here? And I see so much talk about everything but the proof itself. But here in this corner of the internet, we don't shy away from the science part. Now I will note that I am just a student. I am not smart enough to understand the whole solution, but I'll try my best to give you my little intuition. So you want to know how many prime numbers exist up to a chosen number x. Up to ten, we have four primes. Up to one hundred, it is twenty-five. Now we have pretty good techniques to estimate this for any x. This estimate gives us a floor and a ceiling, and they are typically pretty close together, pretty tight. But there is a problem. We want a proof that holds for any x, and as x grows really large, the floor and the ceiling drift apart really far, almost as many digits as x itself. That's too loose of a bound. That's too large of a search area around it. It's like when you're searching for your car keys and someone says, "I know where they are." Oh goodness, thank you. Where are they? And he says, "Well, somewhere in the universe." Oh, okay. Well, we need something a bit more.

【译】……跑自动研究循环，试图把证明压缩成更优雅、希望也更易理解的东西。有些片段缩小了 90% 以上，但那些只是片段。看整个证明的话，到目前为止它几乎没有动。更新。发视频前等一等是对的，不要立刻发布，因为我找到了另一个项目，它已经在整个证明上实现了有意义的压缩。太好了。现在我的问题是：一个我们不理解的黎曼假设证明有多大价值？它真的是游戏规则的改变者（game changer）吗？仅仅我们现在知道它为真这件事，本身并不是游戏规则的改变者。为什么？因为你可以继续做你的数学，假设它为真，然后该干嘛干嘛。很多人的研究里本来就是这么做的。所以在这个案例里，我认为也许比定理本身更重要的，是证明本身教给了我们什么。如果这证明是一团意大利面，我们在这里学到了什么有用的东西吗？而我看到那么多讨论，谈的却是一切，唯独不谈证明本身。不过在互联网的这个角落，我们不回避科学部分。我要说明，我只是一个学生。我不够聪明，没法理解整个解答，但我会尽力给你们讲讲我的那点小直觉。你想知道直到某个选定的数 x 为止存在多少个素数。到 10 为止，有 4 个素数。到 100 为止，是 25 个。现在我们已经有相当不错的技术可以对任意 x 做估计。这个估计给我们一个地板和一个天花板，它们通常彼此相当接近、相当紧。但有一个问题。我们要的是一个对任意 x 都成立的证明，而当 x 变得非常大时，地板和天花板会漂移得非常远——几乎有 x 本身那么多

[240s - 360s]

More accurate than that. So how do we solve that, dear fellow scholars? This is Two Minute Papers with Dr. Károly Zsolnai-Fehér. So we wish to bring that ceiling down, but doing that is not easy. Imagine trying to weigh a single sheet of paper on a bathroom scale. One sheet is too light. The scale can't measure that well. So what do we do? Of course, we stack a hundred sheets together and put that on the scale and weigh the whole stack. We get something divided by a hundred, and ta-da, we have the weight of one sheet of paper. That's kind of what mathematicians are trying to do here. Since we can't weigh the primes one by one, we need something to weigh all of them together to shrink the error down. We need every prime to show up many times in the formula, just like stacking those sheets of paper. Now that is a great idea. Apart from the fact that it is impossible. If you stack this formula over and over for whole numbers, you find something crazy. Yup, there it is. The error is also increasing at the same time, and over time it explodes. Imagine the scale getting wobblier and wobblier every time you add a new sheet of paper. It doesn't measure correctly anymore. No good. Enter the theta lift. This is a technique that keeps the scale steady. So as you add one more sheet of paper, the stack grows, but the error stays under control. Now, hold on to your papers, fellow scholars, because one of the big leaps in the new proof is figuring out how tall to build that stack, how many lifts to use. They found that if you do that, the square root of x times you will squeeze the floor and the ceiling closer and get an accurate result. So just to make sure, the theta lift itself is not new. It was invented fifty plus years ago. But the way it is chained to

【译】……位数。这个界太松了。围绕它的搜索区域太大了。这就像你在找车钥匙，有人说：「我知道它们在哪儿。」噢天哪，谢谢。在哪儿？他说：「嗯，在宇宙里的某个地方。」噢，好吧。那我们需要比这更精确一点的东西。那么亲爱的学者伙伴们，我们怎么解决这个问题呢？这里是 Two Minute Papers，我是 Károly Zsolnai-Fehér 博士。我们希望把那个天花板压下来，但要做到这一点并不容易。想象一下在浴室体重秤上称一张纸。一张纸太轻了，秤量不了那么精细。那我们怎么办？当然是把一百张纸叠在一起，放到秤上，称整叠的重量。我们得到一个数再除以一百，然后，当当，我们就得到了一张纸的重量。数学家在这里努力做的，差不多就是这种事。既然我们没法把素数一个一个地称，我们就需要某种办法把它们全部放在一起称，把误差缩下去。我们需要每个素数都在公式里出现很多次，就像把那些纸叠起来一样。这是个绝妙的主意。只除了一点——它不可能做到。如果你把这个公式对全体整数一遍又一遍地叠加，你会发现一件疯狂的事。没错，就是这样。误差同时也在增大，而且随着时间推移它会爆炸。想象每加一张新纸，秤就变得更晃一点。它就再也量不准了。不行。theta 提升（theta lift）登场。这是一种能让秤保持稳定的技术。这样你每多加一张纸，这叠纸会变高，但误差仍然受控。现在，学者伙伴们，抓紧你们的论文，因为新证明里的一大跨越，就是弄清楚这叠纸要叠多高、要用多少

[360s - 480s]

Together many times to great effect. Now that part I think is novel. Now I am just a student. I could be wrong, but that is my little intuition. And to answer the knowledge question, yes, there is plenty to learn here. And the next question is what now? Well, two things. One, I am not the authority in assigning credit, but we celebrate great scientists here on this channel. So here is a list of researchers who contributed to the major milestones of the Riemann hypothesis solution. Huge respect and thank you to all of you. We can't list them all, but we can try. And this is absolute history in the making. I cannot believe what just happened here. And two, get this: people are already trying to have a bunch of AI agents solve another million-dollar Millennium problem, P equals NP. That one is my favorite. This is science building in public, which I think is super cool. You can even watch what they are trying and how they are doing. I think a real solution might need a bit more direction, but who really knows? So that is a future that I see in front of us, us getting new tools to be able to learn and understand more about the world, invent new things, and improve the lives of people. And hopefully lots and lots of more of these in the future, not with closed models, but with open-weight models, building in public, where anyone can contribute. Love that. What a time to be alive! And subscribe and hit the bell for more papers. I use Lambda to reproduce AI research papers often in minutes. It's great to train your own models, fine-tune an existing one, run inference, text to image or video, easy peasy. Running a DeepSeek chatbot or agent, super fast, super reliable. Lambda gives you powerful NVIDIA GPUs to run your own experiments. I test ideas from papers I cover and.

【译】……次提升。他们发现，如果这样做——x 的平方根倍——你就会把地板和天花板挤得更近，得到精确的结果。所以先确认一下：theta 提升本身并不是新东西。它是五十多年前发明的。但把它多次串联在一起、并产生巨大效果的方式，那部分我认为是新颖的。我只是一个学生，我可能是错的，但那就是我的小直觉。回答关于知识的问题：是的，这里有大量可学的东西。下一个问题是：现在怎么办？嗯，两件事。第一，我不是分配功劳的权威，但这个频道会庆祝伟大的科学家。所以这里是一份对黎曼假设解答的主要里程碑做出贡献的研究者名单。向你们所有人致以巨大的敬意和感谢。我们没法列出所有人，但我们可以尽力。而这绝对是正在书写的历史。我不敢相信这里刚刚发生的事。第二，听好了：已经有人在试着让一群 AI 智能体去解决另一个百万美元的千禧年难题——P 等于 NP。那是我最喜欢的一个。这是公开进行的科学，我觉得这非常酷。你甚至可以看到他们在尝试什么、做得怎么样。我认为一个真正的解答可能需要更多的方向指引，但谁知道呢？所以这就是我看到的未来：我们获得新的工具，能够学习和理解更多关于世界的东西、发明新事物、改善人们的生活。而且希望未来有许许多多这样的成果——不是用闭源模型，而是用开放权重模型、公开构建，任何人都可以贡献。爱了。活在这个时代多好啊！订阅并按下铃铛，获取更多论文。（口播）我经常用 Lambda 在几分钟内复现 AI 研究论文。它很适合训练你自己的模型、微调现有模型、跑推理、文生图或文生视频，轻轻松松。运行 DeepSeek 聊天机器人或智能体，超快、超可靠。Lambda 给你强大的 NVIDIA GPU 来跑你自己的实验。我测试我所报道的论文里的想法，然后

[480s - 487s]

Moments later, results. Love it. Seriously, try it out now at lambda.com slash papers.

【译】（口播）……片刻之后，结果就出来了。爱了。认真说，现在就去 lambda.com/papers 试试吧。
