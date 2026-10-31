---
title: "OpenAI 说它证明了「不可能」：3-SAT 的亚指数算法，与被撼动的指数时间假设——Two Minute Papers"
date: 2026-10-31T12:09:00+08:00
draft: false
tags: ["灰色信源", "Two Minute Papers", "OpenAI", "AI趋势", "数学"]
categories: ["podcast-summary"]
summary: 'Two Minute Papers 主理人 Károly Zsolnai-Fehér 用猫、狗、机器人的三行逻辑题在三分钟内讲清 3-SAT 与指数时间假设（ETH）：三十年来被视为计算世界「物理定律」的这堵墙，如今被 OpenAI 的一篇论文声称推倒——为 3-SAT 给出亚指数上界，且论文不只给证明、还给出算法本身并能精确数出解的个数。Károly 的态度罕见地重：「我原以为这不会发生在我的有生之年。」他同时给出两条清醒限定：专家评审尚未百分之百完成；这不是 P versus NP 被解决。送药路线、频率分配、传感器布点、航天器装载为何与猫狗机器人题同构，看 Karp 归约一节。'
---

> 信源说明：本文基于 Two Minute Papers 频道视频《OpenAI Claims It Just Did The Impossible》（视频 ID CWdTBLsbaQs，发布于 2026-10-30，全长约 7.4 分钟，英文转写 4 段覆盖 0–441 秒，已完整读取）整理，属本季短资讯体裁。ETH 被证伪事件的完整背景（OpenAI 的「护送」署名机制、ETH 提出者 Impagliazzo 与 Paturi 的角色、对密码学的影响、反方 Viola 的「我相信 P=NP」）本博已在此前详述，见[《「指数时间假设」被 AI 证伪之后》](/posts/2026-10-28-wes-roth-openai-eth-disproof/)（Wes Roth 同主题视频，早于本期三天）；Károly 本人此前对 OpenAI 722 篇数学论文的报道（含三篇撤回的后记）见[《OpenAI 的 722 篇数学论文里有什么》](/posts/2026-10-14-two-minute-papers-openai-722-math-papers-proofs/)。本文只展开本期独有内容：Károly 的外行友好讲解与两条限定。全部为节目陈述，未经独立核实；ASR 疑似错词以「转写疑作」标注，文末附校正表与完整转写原文。

## 先说结论

Károly 的开场白就是结论：「我原以为这不会发生在我的有生之年。」OpenAI 发表论文声称**证明了指数时间假设（ETH）不成立**——为 3-SAT 给出亚指数上界（节目引用的一句话总结："a sub-exponential bound has been claimed for 3SAT, refuting the exponential time hypothesis"）。他形容若经验证成立，「理论计算机科学最重要的假设之一刚刚倒下」。但本期最值得记的是他给的两条限定：专家评审**尚未百分之百完成**（他称证明「看起来越来越有说服力」）；以及这**不等于 P versus NP 被解决**。与 10-28 那期 Wes Roth 视频相比，Károly 的增量在于证明的「交付物」：论文不只给出存在性证明，**还给出了算法本身，并且能精确数出解的个数**——他称之为「历史性时刻」。

## 猫、狗、机器人：三分钟讲清 3-SAT 与 ETH

Károly 的讲解（0–240 秒段）值得原样保留：给三行逻辑式——「猫在睡，或狗在叫，或机器人在跳舞」——问什么时候为真？只让猫睡着，一行时轻松；再加一行「猫醒着，或狗不叫，或机器人在跳舞」，两行开始需要真的推理（猫没法同时睡与醒，得靠机器人跳舞救场）。问题规模一大，暴力枚举的代价是灾难性的：他给的量级是每秒十亿次组合的处理器也要**约一千万亿年（a quadrillion years）**。聪明算法存在、都比蛮算快，但「没有谁能在一般情形下击穿指数壁垒」——这就是 ETH。他强调：无人能证明它，但相信者众，**大量论文把它当地基在其上盖楼**（与 Wes Roth 那期「几十栋楼盖在一块说不存在的地上」同款比喻，互为印证）。

## 为什么送药、频率分配、传感器、航天器都在同一堵墙后

节目最实用的一段是 Karp 归约的通俗版（240–360 秒段）。Károly 先讲了个冷笑话：这不是「把鱼缩小」（carp，与 Karp 谐音；转写作 carb）。四个看似无关的问题——给数百所医院送救命药、油量有限求最省路线；给数千个发射机分配无线频率避免干扰；用最少的传感器监控整个系统；往小航天器上装载价值最高的科学设备——**全都可以变换（mold）成猫狗机器人那道题**。所以若那个问题存在更快的算法，「不总是、但经常」惠及这一整族问题。这也是为什么这道「猫狗机器人题」值得历史级的标题待遇。

## 还没到开香槟的时候

Károly 的限定语值得逐条记录：其一，**专家评审未完成**——「看起来越来越有说服力」是他的措辞，不是定论；其二，P versus NP 仍然悬而未决，两件事不可混为一谈；其三（可推断的观察），这延续了 OpenAI 近期数学/物理成果的发布模式，本博此前已记录其争议（保密验证、署名机制、722 篇论文中三篇撤回）。对读者的观察点沿用上期：一线复杂度理论家的公开评估、亚指数算法是否出现可用的实现、以及同行评审的最终裁决。片尾预告与 Spencer Huang 的访谈（转写名待核），赞助商为 Lambda。

## 转写校正表

| 转写原文 | 校正 | 依据 |
| --- | --- | --- |
| Dr. Karo Jonahifahir | Károly Zsolnai-Fehér | 沿用本博 10-14 文校正（匈牙利姓名被 ASR 误切分） |
| carb reductions | Karp reductions（Karp 归约；节目冷笑话「不是把鱼缩小」即 carp 谐音） | 上下文与术语 |
| Spencer Huang | 保留原文 | 待访嘉宾名，未核 |
| three satisfiability problem | 3-SAT（三元可满足性问题） | 术语通用缩写 |

## 转写原文（修正版）

[0s - 120s]

I did not think this would happen in my lifetime. Let me try to explain. For many decades now, computer scientists have been thinking up devilishly difficult problems and found that trying to solve some of them takes incredibly long. Some of them would take a quadrillion years to solve by brute force. But we humans have invented so many amazing algorithms. We can write programs that simulate light rays to imagine never-before-seen scenes. I love this so much. And we can write physics simulations to create really lifelike or imaginary worlds. Absolutely ingenious human-developed techniques. And you are saying that some problems just cannot be solved efficiently. Well, that is exactly what the exponential time hypothesis says. It says that some problems are cursed and might always remain so. Here is a cursed problem: the cat is sleeping, or the dog is barking, or the robot is dancing. When is this true? Well, just make the cat sleep. Problem solved. Easy peasy. What is Carlo even talking about? Get out of here. No, no, no, no. Wait. Let's add another line: the cat is awake, or the dog is silent, or the robot is dancing. Now, the first one wants the cat to sleep. The second one wants the cat to be awake. So we have to look for help with a different variable. Let's look at the dog. Oh my, same problem. The dog is barking in the first, silent in the second. But wait, the robot is luckily dancing on both, so that can satisfy both lines. Hooray! But the point here is that one line was very simple, but two lines needed a bit of computation. Still simple, of course. And now you will see the crazy part. We call this

[120s - 240s]

The 3-SAT (three satisfiability) problem, and if you increase the problem size with more animals and lions, what happens? Well, just get a processor that goes through a billion combinations per second, and you are done instantly, right? Not quite. This computer program would take so long it might never finish. Well, it could take one quadrillion years to finish if we check every combination. Well then, this is so many combinations, so better write an algorithm that does it a smarter way and do it a lot quicker. And here comes the crazier part. We couldn't. Nobody had ever been able to do that in the general case. Yes, we can be quicker than just trying all the combinations. We have smart algorithms, but none of them could defeat the exponential barrier. This is called the exponential time hypothesis. So it's over, right? Do we have no hope left to solve this, dear fellow scholars? This is two minute papers with Dr. Károly Zsolnai-Fehér. Well, all this drivel means that this problem is cursed. Cursed how? Well, cursed in a sense that as the problem size grows, even just a tiny bit, it gets much harder. In the worst case, exponentially harder. You. Now we can't prove it, but so many people believe it to be true that many papers have been written assuming that this is true and building on that. And then something insane just happened. Hold on to your papers, fellow scholars, because the age of AI has led us into a breakthrough. OpenAI published a research paper where they say they proved that this hypothesis is not true. They say that it is possible to defeat the exponential barrier. I'm gonna be honest. I did not think this would happen within my lifetime, and it just did. If the proof gets properly

[240s - 360s]

Verified. It looks increasingly convincing, but the expert review is not a hundred percent done yet. But if it holds up, then one of the most important assumptions of theoretical computer science has just fallen. That is insanity. Now, I will note that this is not to be confused by the P question mark equals NP problem that has not been solved. Now, you could say, okay, Karoly, but who cares about this silly satisfiability problem? Just bring your dogs and cats elsewhere. Well, I say you have to consider Karp reductions before you ask. No, this does not mean shrinking a fish. It means something completely different. Imagine you have to deliver life-saving medicine to hundreds of hospitals, and you are a bit low on gas. You need the most efficient route. Or imagine you have to assign radio frequencies to thousands of transmitters so they can talk without too much interference. Or place the fewest number of sensors needed to monitor an entire system. Or select the most valuable scientific equipment to load it onto a small spacecraft. All of these are very different problems, except that they are not. All of these problems can be molded to look like our three satisfiability problem with the cats and dogs. You take your roads and transform them into a dog and cat question. That is Karp reduction. Wait, and does that mean that? Oh yes, yes it does. If you can write an algorithm that can do that one problem faster, it could work for all of these problems, and then even more. Not always, but it's often the case, and that is why this work is so incredibly surprising and valuable. Plus, it gets better. The paper reveals not just the proof.

[360s - 441s]

That such an algorithm exists. The paper actually gives us the algorithm, and it gets even better. It not only finds the solution, but also tells us exactly how many solutions are there. This is a historic moment in our lives. What a time to be alive! And just to demonstrate how incredibly efficiently scientists can communicate with each other, all of this summed up in short: a sub-exponential bound has been claimed for 3SAT, refuting the exponential time hypothesis. Very concise and super awesome. If you enjoyed this, consider subscribing and hitting the bell. We have a lot more papers coming, and if everything goes well, an interview with Spencer Huang as well. Very fun. I use Lambda to reproduce AI research papers often in minutes. It's also great to train your own models or fine-tune an existing one. Run inference or text to image or video, easy peasy. Running a DeepSeek chatbot or agent, super fast, super reliable. Lambda gives you powerful NVIDIA GPUs to run your own experiments. I test ideas from the papers I cover, and moments later, results. Love it. Seriously, try it out now at lambda.ai/papers.

