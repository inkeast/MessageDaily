---
title: "五周、没升版本号、架构不变，DeepSeek V4.1-Flash-1017 却大幅变强：关键词是「自我反馈学习」"
date: 2026-10-29T12:13:00+08:00
draft: false
tags: ["灰色信源", "Two Minute Papers", "DeepSeek", "模型发布", "开源模型", "AI趋势"]
categories: ["podcast-summary"]
summary: 'Two Minute Papers（Károly Zsolnai-Fehér）评 DeepSeek V4.1-Flash-1017：距上一版约五周、版本号未升、架构未变，却在写可用软件与终端操作计算机上大幅进步；API 输出价约百万 token 一美元出头、约为 Claude Opus 5.5 的十五分之一，但主持人实测其同任务多耗两到三倍 token，提醒应按「每任务成本」而非「每百万 token 价格」比价；开放权重可永久拥有。他交叉比对发布公告措辞，猜测能力跃升的关键是「learning from self-feedback」——模型自己critique、自己改。'
---

## 发生了什么

Two Minute Papers 主持人 Károly Zsolnai-Fehér 用不到六分钟讲清了 DeepSeek 的新版本 **V4.1-Flash-1017**（依据英文转写整理，3 段 0s–341s 完整读取；该模型此前已被阿里 Qoder 列为免费模型，与本站 10 月 22 日快讯一致）。他起初以为是个小更新：距上一版 V4.1 Flash 只过了约五周，连版本号都没有递增；结果是「大错特错」——新版本构建可正常工作的软件的能力明显更强，用终端操作计算机的能力也大幅提升，API 输出价格约每百万 token 一美元出头，主持人称约为 Claude Opus 5.5 的十五分之一，权重开放下载、可永久拥有（但跑起来的硬件开销被他戏称为「整整一个 bazillion 美元」，他自己在家跑不动）[0s–120s]。

## 两个值得学的判断

**比价要看每任务成本，不是每百万 token。**主持人提醒「智能便宜到不用计量」的叙事要打折扣：不同模型完成同一任务消耗的 token 数不同，而他自己的独立测试显示 DeepSeek 完成同一件事可能比 Claude Opus 多用两到三倍 token——折算后每任务成本仍然不错，只是没有标价看起来那么惊艳。这是选购模型时最容易被单价迷惑的一个坑[0s–120s]。

**架构没变、能力大跳，关键词是「自我反馈学习」。**他交叉比对了 DeepSeek 官方公告的措辞与既有研究（他明确声明「不保证猜对」），认为关键术语是 learning from self-feedback：与「需要更多训练数据」的行业主叙事相反，模型能够评判自己的作品并从中学习。他给的例子是让 AI 写一个赛车游戏、自己试玩：发现碰撞检测失效就修、发现没有加速就加、觉得画面糙就改，编译后再玩、再改，如此循环[120s–240s]。主持人认为这可能是「AI 从自身经验中学习、像人一样」的研究路线第一次落到一个我们能拥有、能使用的真实产品里——同时他也点出难点：AI 本质上在给自己当裁判和奖励器，「它什么时候做得好并不总是容易判断」[240s–341s]。

## 为什么值得关心

按主持人的框架，每当前沿模型出现，就会开启一场「以更小体积和成本逼近它」的竞赛：DeepSeek 对前沿模型做这件事，而其他系统再把 DeepSeek 蒸馏得更小更便宜——「也许不到一年，你就能在一台高配笔记本上跑它」。开放权重紧跟前沿、五周一次明显迭代，他的结论是竞争让智能对所有玩家更便宜。给工程师的可操作提醒有三条：按每任务成本核算模型开销；把「自我反馈/自我评判」当作选型和复现实验时的关注变量（主播仅凭公告措辞推断，尚待独立验证）；开放权重版本发布的蒸馏跟随节奏，值得纳入自托管规划。本期由 GPU 云 Lambda 赞助（lambda.ai/papers）。

## 归属与转写说明

本文全部事实与数字出自本集转写（含「十五分之一」「两到三倍 token」等主持人个人测试口径，未独立核实）；「learning from self-feedback」为主持人对官方公告措辞的推断，非 DeepSeek 官方技术确认。转写校正：Dr. Karo, Jonah, and Fahir→Károly Zsolnai-Fehér（主持人名）；4.1 Flash/4.1 Flash 1017→V4.1 Flash/V4.1-Flash-1017（模型官方命名）。


## 转写原文（修正版）

[0s - 120s]
Good news, everyone! A new DeepSeek AI system is available. Hooray! So, is this a small or a big jump in AI capabilities? Well, doesn't sound big because the previous version was V4.1 Flash, and this one is called V4.1-Flash-1017. I know names. Only about five weeks have passed since the last version, and they didn't even want to increment the version number. So I guess it's not a big jump, right? Boy, was I wrong when I thought that. Holy matter of papers! Look at that. It builds much better working software and is way better at using the terminal to operate the computer. And it's really good at other things too. You can run it from the API for just a bit more than a dollar per million output tokens. Wow! But can that really be? This is 15 times cheaper than Claude Opus 5.5. Finally, intelligence too cheap to meter. Well, that sounds a bit better than it really is. You see, we always measure dollars per million tokens, but different AI systems use a different amount of tokens for the same task. And DeepSeek is not among the best in this. In my independent tests, I saw that it can easily take two to three times more tokens to accomplish the same thing as Claude Opus. So we should always think in cost per task, not per million tokens. Now, the cost per task for DeepSeek is still quite good, just a bit less good as what is advertised. Now, here is the best part. You can download and own the weights for free and own it forever. You will need, hmm, let me check my notes. Ah, yes, exactly one bazillion dollars.

[120s - 240s]
To get the hardware to run it, so I can't run that myself at home. However, that's not the point. The point is that after a new AI system appears, it starts a race to create something close to as good, but at a fraction of its size and cost. DeepSeek does that to the frontier models, while other systems distill DeepSeek down into smaller and cheaper systems. So, yes, first one bazillion dollars will be needed, and maybe less than a year later, you find yourself being able to run it on a beefy laptop. But wait, the architecture did not change at all in this new version. So how on earth did they get such a huge jump in capabilities? Dear fellow scholars, this is Two Minute Papers with Dr. Karoly Zsolnai-Fehir. So how? Now I looked at the language they used in their announcement and cross-checked it against other real research to see what they may have meant. No guarantees, though. But I think the key term here is learning from self-feedback. What's that? That's the opposite of what everybody is talking about, which is needing to obtain more training data. This is the opposite. This means that the AI is able to critique its own work and learn from it. That is absolutely amazing. Now hold on to your papers, fellow scholars, and imagine that it could code up a racing game. Then it starts playing it and notices, wait a minute, the collision detection does not seem to work. Things go through each other. Well, that's not great. Let's fix that. Compile the new game and keep playing and improving it. Now there is no acceleration. Fix that. Then visual detail is quite poor. Fix that too, and so it goes. So if you remember.

[240s - 341s]
One thing about this episode, let it be this: we keep talking about giving more and more training data to AI systems to learn from. But previous research works have shown that we can create situations where the AI can learn from its own experiences, learning like a human. And maybe this is it coming online in a real product we can own and use. Ooh, that is incredible and very hard to pull off because the AI essentially judges and rewards itself, and it's not always easy to tell when it is doing well or not. The frontier AI models are improving at an incredible pace, and the open weights alternatives are right on their heels. And all this improvement in just five weeks, I am stunned. Amazing! It is lovely to see that there is intense competition in this space. That is amazing news for all of us fellow scholars. More intelligence for all of us, cheaper. What a time to be alive! I use Lambda to reproduce AI research papers often in minutes. It's also great to train your own models or fine-tune an existing one. Run inference or text to image or video, easy peasy. Running a DeepSeek chatbot or agent, super fast, super reliable. Lambda gives you powerful NVIDIA GPUs to run your own experiments. I test ideas from the papers I cover, and moments later, results. Love it. Seriously, try it out now at lambda.ai/papers.
