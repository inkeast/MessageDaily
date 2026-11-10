---
title: "Astra 并没有「教会」AI 数学：Two Minute Papers 给「权重空间编程」的四盆冷水"
date: 2026-11-08T18:06:00+08:00
draft: false
categories: ["podcast-summary"]
tags: ["灰色信源", "Two Minute Papers", "OpenAI", "可解释性", "RSI", "AI趋势"]
summary: 'Two Minute Papers 针对 GPT-6 Astra 读写神经网络权重的 MIT 研究做纠偏：8%→48% 不是「教数学」而是修掉「思考死循环」；读、修、提升是三个独立实验而非一套系统；开源模型已复现部分能力；对 Gemma 3 的提升尝试失败。主持人仍认为「从零写出游戏网络」可能是 AI 的 game changer，且完整基准正在建立。'
---

> 信源说明：本文基于 Two Minute Papers 频道一期视频《OpenAI's Astra Just Opened The Black Box Of AI》（视频 ID `GJ6PMUm5oTA`，2026-11-07 发布，全长 348 秒≈5 分 48 秒，英文转写 3 段、0s–348s 已完整读取，文中数字与引语均依英文转写；主持人依频道公开信息为 Dr. Károly Zsolnai-Fehér，转写作「Dr. Karo Jonahifahir」，已列入文末校正表）。全部实验结果与判断均为节目转述论文内容，本文未做独立核实，归属按「节目称/主持人认为」分层；节目开场即称研究来自 MIT，与本站此前按 The AI Daily Brief 成文的口径互证。**防重复说明**：该研究本站已两次成文——Matthew Berman 的逐实验深拆（[《权重即源代码》](https://inkeast.github.io/MessageDaily/posts/2026-11-05-matt-berman-gpt6-astra-weight-space-programming/)）与 The AI Daily Brief 的报道（含 MIT 归属与 Google 研究者复现，[《补贴时代的账单与 RSI 的阈值》](https://inkeast.github.io/MessageDaily/posts/2026-11-05-ai-daily-brief-grokbot-subsidy-rsi-weight-editing/)）；实验细节与 RSI 推论不重复展开。本期的独立价值是**纠偏视角**：主持人明说「媒体上的讨论有点被误解」，逐条澄清坊间流传的夸大说法。视频时长不足 6 分钟，按短资讯成文。文末附完整转写原文（修正版）与逐段中文翻译。

## 先说结论

这期不到六分钟的视频做的事情很单纯：给上周开始刷屏的「GPT-6 Astra 能直接读、改、写另一个神经网络的权重」（即 weight space programming，权重空间编程）泼冷水——不是否认研究，而是把四类流传中的夸大说法逐一收回。用主持人自己的话收束：它能读、能修、有时能提升，**仅此而已，而这已经足够惊人**。对本站读者，这篇短文的增量在于：它是三条信源里唯一一条以「别人哪里说错了」为主线的报道，恰好补上前两文（正面深拆）之外的传播侧证据。

## 研究本身一句话带过

节目称，Astra 把一个神经网络的原始权重（「一大锅数字」）当输入直接读，能认出这个网络被训练来玩 Breakout、Pong 或 Pac-Man 的简化版；能通过编辑权重修好一辆卡在坡上的小车；能把开源模型 Qwen（转写作 Quan）的数学基准成绩从约 8% 拉到约 48%；还能完全绕过强化学习、从零写出一个会玩游戏的网络。这些实验的机制细节本站已在 Berman 深拆一文展开（改的其实是 Qwen 的「推理死循环」缺陷、单次尝试只动 3.5% 权重等），此处不重复，见文首链接。

## 四盆冷水：流传说法 vs 实验事实

**冷水一：「Astra 教会了这个 AI 数学」——不对，它只是帮模型「解锁」。** 主持人反复强调：Qwen 不是不擅长数学，而是陷入了「疯狂的思想循环」，不停重复、原地打转——他自嘲「有点像我晚上在床上翻来覆去」。Astra 做的是反复测试这个模型、观察哪里出错、定位神经网络中负责死循环的部分、编辑权重把这类行为压下去。8%→48% 的跃升来自修 bug，不是传授能力。

**冷水二：「它读心、修心、强心一条龙」——不对，是三个独立实验。** 节目称这是最常见也最要命的误读：读权重、修权重、提升权重这三件事确实都做到了，但分别在不同实验里完成，且大多在小型 AI 系统上、每个实验有各自的定制 setup——「这和『一个系统同时做三件事』是完全不同的两回事」。

**冷水三：「只有 OpenAI 的 Astra 能做到」——不对，开源复现已经在做。** 主持人称已有独立实验用更小、免费、开源、家用电脑就能跑的模型（他举的例子正是 Qwen）完成了同类事情——读出一个神经网络会玩什么游戏。能力的门槛没有传闻中那么高，也更不专属。

**冷水四：「它总是有效、AI 正在自动互相改进」——不对，失败案例就摆在那里。** 节目称 Astra 曾尝试提升另一个语言模型 Gemma 3，未能取得任何有意义的改进——「它能读、能修、有时能提升，仅限狭窄场景」。

## 泼完冷水之后，主持人仍然兴奋

纠偏不等于唱衰，这条视频的底色仍是惊叹。主持人认为其中最惊人的是「从零写出会玩游戏的网络」——这类网络通常要靠海量试错的强化学习训练出来，如今被直接写出来，「可能是 AI 的绝对 game changer」；他并称研究者**正在为此建立完整的基准测试**（"a full benchmark for this kind of weight space programming"），「这件事可能真会成气候」。结尾他照例搬出「论文第一定律」：不要看我们现在在哪里，要看两篇论文之后我们会到哪里。

## 读者启发

这期短视频给的不是新事实，而是一个传播学样本：一项能力真实的论文，在二手传播中如何被逐级放大——「分离实验里的能力」被传成「单一系统的全能」，「一次成功」被传成「总是有效」，「用了前沿模型」被传成「只有这个模型行」。三个可迁移的检查动作：看到惊人百分比先问「改的到底是什么」（本例是修死循环而非教数学）；看到「读修写」并列表述先问是不是同一套系统同时做到；看到「只能 X 能做」先找开源复现。顺带一提，这种「纠偏型」信源在热点论文传播中值得专门留意——它往往比第 N 篇正面报道的信息密度更高。

## ASR 校正表

| 转写原文 | 处理 | 说明 |
| --- | --- | --- |
| Dr. Karo Jonahifahir | Dr. Károly Zsolnai-Fehér | 频道主自报家门段转写失真，依频道公开信息校正 |
| Quan | Qwen | 上下文（开源权重、可家用运行）与 Berman 篇口径（Qwen3.5 0.8B）互证 |
| GPT-6Astra | GPT-6 Astra | 模型名加空格规范化 |
| who? | 保留原文 | 视频内演示音效/口误，无法判定所指 |
| 48 percent / eight percent | 约 48% / 约 8% | 口述数字，节目未附基准名，与 Berman 篇口径一致 |

## 转写原文（修正版）

[0s - 120s]
Incredible piece of research from MIT. I wanted to make a video about it because it is a potential game changer for AI, and I see people talking about it in the media, and it seems a bit misunderstood. Okay, what happened? Well, three things. One, scientists used GPT-6 Astra to read the mind, the weights of a neural network. Now, that is normally extremely difficult or impossible. This is a big soup of numbers that roughly gives you the neurons and their connections in a digital brain, an AI system. They work great, but they are often completely incomprehensible. Now, hold on to your papers, fellow scholars, because surprisingly, after looking at another AI, Astra was able to identify what games this AI system was trained on. It's breakout, yeah, or pong or Pac-Man, simplified versions. It recognizes all that from just a bunch of numbers. That is insane. Two, it can fix a failing AI system by editing these weights, kind of like brain surgery for AIs, changing the numbers so it is able to drive smarter. Before the edit, the car initially got stuck on the ramp. Who? Now let's see after the edit. Hmm. Look at that. It now successfully goes through. So there was reading, there was fixing, and now three improving. You give it Qwen, one of the crowd favorite open weights AI systems that you can run at home, and before the edit, it could solve about eight percent of the tasks on a math benchmark. Now after the edit, whoa, that's super nice. About forty-eight percent. That is an insane jump. So how did it do that? Well.

【译】来自 MIT 的一篇难以置信的研究。我想为它做一期视频，因为它可能是 AI 的潜在 game changer，而我看到媒体上大家的讨论似乎有点被误解了。好，发生了什么？三件事。第一，科学家用 GPT-6 Astra 去读一个神经网络的「心智」——也就是权重。这件事通常极难甚至不可能：那是庞大的一锅数字，粗略地给出一个数字大脑（AI 系统）里的神经元和它们的连接。它们运转得很好，但往往完全无法理解。各位学者，抓紧你们的论文，因为令人惊讶的是，在看过另一个 AI 之后，Astra 能够识别出这个 AI 系统是被训练来玩什么游戏的——是 Breakout，对，或者 Pong、吃豆人的简化版。它只从一堆数字里就认出了这些。这太疯狂了。第二，它可以通过编辑这些权重去修复一个失灵的 AI 系统，有点像给 AI 做脑部手术，改变这些数字，让它开得更聪明。编辑之前，小车一开始卡在了坡道上。（演示音效）现在看编辑之后。嗯，看到了吗，它成功开过去了。所以：有读取，有修复，现在第三件——提升。你给它 Qwen，一个人气很高的、可以在家自己跑的开源权重 AI 系统，编辑之前，它在一项数学基准上只能解出大约百分之八的任务；编辑之后，哇，太漂亮了——大约百分之四十八。这是疯狂的跃升。那它是怎么做到的？

[120s - 240s]
Astra recognized that Qwen was not necessarily bad at math. It just got into these crazy thinking loops. It keeps repeating itself and going around in circles, kind of like I do, rolling around in bed at night. It tested the other AI model repeatedly and observed what was going wrong. Then it recognized which parts of the neural network are responsible for the loop and edited those to suppress this kind of behavior. But not so fast. Misunderstanding one that I see going around is that Astra taught this AI mathematics. Not quite. Instead, it essentially helped unblock it. And there are still plenty of limitations. And this is where the coverage that I see elsewhere gets a little carried away. Real talk here. I'll try to clear up what it can do and cannot do in reality. But I am also just a student who is trying to learn, so I might be wrong. Another misunderstanding is that this one can read the mind, can fix the mind, and improve the mind of an AI. This does all three, yes. But people say that it can do all three at the same time. That is not the case. It does these three things, but in separate experiments, mostly on small AI systems, each with its own custom setup. That is very different. And some say that only OpenAI's Astra can do that. That is not true. There are independent experiments already doing the same, reading which game a neural network can play, and they do it with smaller, free and open models like Qwen that you can run at home yourself. And there are even more things misunderstood about this work. Dear fellow scholars, this is Two Minute Papers with Dr. Károly Zsolnai-Fehér. Here's another one. They say it always works.

【译】Astra 认识到，Qwen 未必是不擅长数学，它只是陷入了那些疯狂的思想循环：不停自我重复、原地转圈——有点像我夜里在床上翻来覆去。它反复测试这个 AI 模型，观察是哪里出了问题，然后识别出神经网络中哪些部分要对这个循环负责，并编辑它们来压制这种行为。但是先别激动。我看到流传的第一个误解是：Astra 教会了这个 AI 数学。并不完全是——它本质上只是帮它「解锁」了。而且仍然有大量局限，这也是我看到别处报道有点过头的地方。说句实在话：我会尽力讲清它现实中能做什么、不能做什么，但我也只是一个正在学习的学生，我可能会错。另一个误解是：它能读心智、修心智、增强心智，三样都做了——是的，都对。但人们说它能**同时**做这三件事，那就不是了。它做了这三件事，但是在各自独立的实验里，而且大多是在小型 AI 系统上，每个实验有自己定制的 setup。这是完全不同的两回事。还有人说不只有 OpenAI 的 Astra 才能做到——不对，反过来说才对：已经有独立实验在做同样的事情，读出一个神经网络会玩哪个游戏，而且用的是更小、免费、开源的模型，比如你自己在家就能跑的 Qwen。关于这项工作被误解的地方还有更多。各位亲爱的学者，这里是 Two Minute Papers，我是 Károly Zsolnai-Fehér 博士。再来一条：他们说这总是有效——

[240s - 348s]
And AI systems are automatically improving each other using this. Not really. Astra tried improving a different language model, Gemma 3, and it couldn't master any meaningful improvements. So it can read, it can fix, and it can improve sometimes in narrow cases. And I think that is insane enough, amazing piece of work. But there are outlets that I feel overstate the capabilities a bit. And wait, it does something more. It can even write a game-playing neural network from scratch, which is very surprising because we typically train these with lots and lots of trial and error called reinforcement learning. That is incredible and could be an absolute game changer for AI. Do they know? Let's see. Of course they know. They are building a full benchmark for this kind of weight space programming as we speak. So this might become a thing. Brilliant work. And yes, we should apply the first law of papers, which says, "Do not look at where we are. Look at where we will be. Two more papers down the line. What a time to be alive! I use Lambda to reproduce AI research papers often in minutes. It's great to train your own models, fine-tune an existing one, run inference, text to image or video, easy peasy. Running a DeepSeek chatbot or agent, super fast, super reliable. Lambda gives you powerful NVIDIA GPUs to run your own experiments. I test ideas from papers I cover, and moments later, results. Love it. Seriously, try it out now at lambda.com/papers.

【译】——而且 AI 系统正在借此自动地互相改进。并不是。Astra 尝试提升另一个语言模型 Gemma 3，却没能做出任何有意义的改进。所以：它能读、能修、有时能在狭窄场景下提升——我认为光这些就已经够疯狂、够出色了。但确实有一些媒体我觉得把能力说得有点过头。等等，它还做了一件更多的事：它甚至能从零写出一个会玩游戏的神经网络，这非常出人意料，因为我们通常要用海量、海量的试错——也就是所谓的强化学习——来训练它们。这不可思议，可能是 AI 领域绝对的 game changer。他们自己知道吗？当然知道。就在我们说话的当下，他们正在为这类「权重空间编程」建立一个完整的基准测试。所以这件事可能真会成气候。出色的作品。是的，我们应该应用「论文第一定律」，它说的是：不要看我们现在在哪里，要看两篇论文之后我们将到哪里。生逢其时啊！（以下为广告）我常用 Lambda 在几分钟内复现 AI 研究论文：训练自己的模型、微调现有模型、跑推理、文生图或文生视频，都轻而易举；跑一个 DeepSeek 聊天机器人或 agent，超级快、超级可靠。Lambda 给你强大的 NVIDIA GPU 来做自己的实验。我把视频里讲的论文想法拿去一测，片刻之后就有结果。我很喜欢。说真的，现在就去 lambda.com/papers 试试吧。
