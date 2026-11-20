---
title: "OpenAI 的杨-米尔斯「突破」：机器检查通过之后，人类还在拆 25 万条声明的混沌"
date: 2026-11-20T12:08:42+08:00
draft: false
tags: ["灰色信源", "Two Minute Papers", "OpenAI", "AI趋势", "数学"]
categories: ["podcast-summary"]
summary: 'Two Minute Papers 拆解 OpenAI 的杨-米尔斯存在性「待定解」：AI 把 Balaban 网格法扩展为多尺度追踪，证明用 Lean 写成并通过机器检查；但 David Gross 领衔的评审仍在进行——25 万条声明六成冗余、部分名为 cake 与 cookie，主理人称之「彻底的混沌」，以学生式兴奋与「意面倒桌」批评的双重立场收场。质量间隙未解，千禧年大奖尚无完整归属。'
---

## 先说结论

Two Minute Papers 主理人 Károly Zsolnai-Fehér 用约 6 分钟，把 OpenAI 一场发布里最重的成果——杨-米尔斯存在性问题的「待定解」——讲成大众能跟上的一课：AI 在数学家沿用的网格方法上做多尺度扩展，证明用 Lean 语言写成并通过逐步机器检查；但诺贝尔物理学奖得主 David Gross 领衔的人类评审仍在进行，评审者面对的是约 25 万条声明、六成冗余的「彻底的混沌」（utter chaos，即视频标题所指，标题与内容相符）。主理人立场双重：一边自称「我只是个学生」、为新知识兴奋，一边批评 OpenAI「把一大团意大利面倒在科学界桌上」。范围先说清：这只是存在性部分的待定解，质量间隙（mass gap，指该理论中最轻粒子质量须严格大于零的部分——写作者注）未解决，百万美元千禧年大奖没有被完整拿走。以下均为节目陈述与主理人观点，数字与进展未经独立核实。

## 难在哪：一张不断加细却会「失控」的网格

强核力的方程精确得惊人，但没人能证明贴近现实的结构放大细看时这些方程依然自洽——这就是杨-米尔斯存在性与质量间隙问题，克雷研究所七个千禧年大奖之一（奖金 100 万美元，他玩笑称「够买两块 GPU」）。这份存在性证明出自 OpenAI 十月末那场多成果发布（同场成果与事件全貌见 [Wes Roth 旧文](https://inkeast.github.io/MessageDaily/posts/2026-10-28-wes-roth-openai-eth-disproof/)与 [AI Daily Brief 编译](https://inkeast.github.io/MessageDaily/posts/2026-10-12-ai-daily-brief-math-field-disruption-openai-revenue/)）。人类科学家的既有做法：把时空放进一张网格，在网格点上计算力如何行为，再把网格不断加密直到「无穷细」的极限——若能证明理论在极限处成立就是一大步；考察网格尺寸变化的数学工具叫 Balaban 方法。麻烦在于：尺度越细，力场里的量子涨落越多，加密网格不会自动靠近答案，反而让问题「最终彻底失控」，且没有证明表明一切会收敛出有意义的结果。

## OpenAI 的增量：同时盯住多张网格

节目称，OpenAI 的系统建立在 Balaban 方法之上，关键改动是同时追踪多张粗细不同的网格：算出最细网格上的微小变化如何传导到较粗的网格，再把跨尺度影响合成总效应，从而「第一次」表明数学不会失控。验证侧，证明用 Lean（一种逻辑可被机器逐步检查的语言）写成、检查通过；但主理人立刻补上科学家式的保留——机器检查只说明推导自洽，「万一 AI 证明的是和我们想要的不一样的东西呢？」命题对齐与意义认定，仍要人来把关。

## 检查通过≠解决：Gross 小组正在拆的「混沌」

评审交给了人类：David Gross 领衔的小组正在核查。主理人特别强调：「尽管一些头条和视频的说法并非如此——还没完成呢。」初步结果令人鼓舞，但评审者面对的是他口中的「彻底的混沌」：代码约 250,000 条声明（可理解为小的定义与数学结果），其中 60% 在最终证明里根本用不上，部分声明的名字还叫 cake 与 cookie——「我真希望自己在开玩笑，但我没有。」他随即限定：未使用的代码不等于证明是错的，只意味着极难理解。这与本站十月报道的「结果派 vs 理解题派」之争（722 篇 AI 论文中三篇已因社区反馈撤回，[见旧文](https://inkeast.github.io/MessageDaily/posts/2026-10-14-two-minute-papers-openai-722-math-papers-proofs/)）互为印证：AI 证明的瓶颈正从「算得出」移向「读得懂、信得过」。

## 立场与观察

主理人的双重立场值得整段保留：一方面「我只是个学生……我为新知识、为学到新东西而兴奋」；另一方面，他觉得自己在看着 OpenAI「把一大团意大利面倒在科学界的桌上，然后请大家自己解开」。为证明判断独立，他补上一句「我和 OpenAI 没有任何关系，他们连我的邮件都不回」，并强调频道「好与坏我们都会谈」。收尾回到全局：这是存在性问题的待定解，质量间隙还在，「百万美元的千禧年问题还没有被完全解决」，但「我们正活在一个不可能正变成可能的了不起的时代」。

## 机制链条与读者启发

- **部分解落地**：OpenAI 交出存在性待定解（多网格机制+Lean 机器检查）→ OpenAI 得先发声量，数学界背上验证负担 → 同行评审被推到 AI 规模证明的验证一线，「验证者」价值上升 → 观察 Gross 小组结论、克雷所是否部分认定、质量间隙是否被跟进 → 启发：机器自证不等于证了想要的命题，命题对齐与意义认定仍是人类把关节点。
- **可读性成新瓶颈**：25 万条声明六成冗余、cake/cookie 命名 → AI 生成不以人类可读性为目标 → 评审者付出时间与认知成本，解释器与证明精简工具成为机会 → 观察冗余比例能否被工具压缩、评审周期长短 → 启发：产出超出人类可读尺度时，验证与解释既是瓶颈也是机会。
- **头条与事实的节奏差**：部分头条把「机器检查通过+评审中」压缩成「已解决」→ 实际质量间隙未解 → 公众对 AI 数学能力的印象跑在证据前面 → 观察后续报道是否区分 proposed / under review / solved → 启发：判断重大声称，先核对精确范围与验证状态，再决定信多少。

对行业：AI 成果的发布节奏已快于人类验证节奏，「发布即头条、验证靠义务劳动」正把同行评审变成整条链路里最慢也最值钱的一环。盯三件事：Gross 小组的评审结论与克雷所表态、Lean 社区对这套证明的复用与精简、下一家实验室是否沿用「机器检查+人类护送」双轨发布。

## ASR 疑似转写错误校正表

| 原文 | 校正 | 依据 |
|---|---|---|
| Dr. Karo Jolnefahir | Dr. Károly Zsolnai-Fehér | 频道主理人公开姓名；站内 2026-10-14 文已确认该匈牙利姓名被 ASR 误切分，本次为同一姓名的另一变体 |
| Belaban's approach | Balaban's approach | 同文首段正确转写作 Balaban's approach；Balaban 为格点杨-米尔斯理论的数学家 |
| young meers existence problem | Yang-Mills existence problem | 全文主题即该问题，首段已正确出现 |
| Yang Mills existence and mass gap problem | Yang-Mills existence and mass gap problem | 标准专名写法带连字符 |

## 转写原文（修正版）

[0s - 120s]
Everything around you, from distant galaxies to the atoms in your body, is governed by forces we don't fully understand. We have equations that describe the strong nuclear force with incredible accuracy, but there is a problem. We cannot prove that they don't fall apart when we zoom closely into the fabric of reality. The problem is so difficult. If you solve it, you get one million dollars, enough for two GPUs. Now, in the age of AI, I wonder if we can finally get a solution to this, which we call the Yang-Mills existence and mass gap problem. This is the existence part, where the goal is to prove that the strong nuclear force still makes sense if we zoom in like crazy. Let's start at the part human scientists came up with. We start with a grid that represents space and time. This grid has grid points connected by lines along which we evaluate how the force behaves and interacts. Now, of course, this is not reality. This is an approximation of reality. The results for this are not conclusive. So now the next idea is to make the grid denser. More grid points give us a finer approximation, hopefully closer to the solution. Then make it even finer and continue this until it becomes infinitely fine. If we can prove that the theory works at this limit, we have made a huge step towards solving the problem. This is where a technique called Balaban's approach comes in, giving us a way to see what happens if we change the grid size. So coarse grid, we have an approximation of how this force behaves. Of course, it's not perfect. Now let's make the grid finer and finer still. Now hold on to your papers, fellow scholars, and oh my, are you seeing what I am seeing?

【译】你周围的一切——从遥远的星系到你身体里的原子——都受着我们尚未完全理解的力支配。我们有描述强核力的方程，精确得惊人，但存在一个问题：我们无法证明，当我们贴近现实的结构放大细看时，这些方程不会崩解。这个问题难到什么程度？如果你解出它，就能拿到一百万美元——够买两块 GPU。现在，在 AI 的时代，我不禁想问：我们是否终于能得到这个我们称之为「杨-米尔斯存在性与质量间隙问题」的解？先说存在性部分：目标是证明强核力在我们疯狂放大细看时仍然说得通。先从人类科学家想出的办法讲起。我们先画一张代表时间和空间的网格，网格上的点由线相连，沿着这些线评估这个力如何行为、如何相互作用。当然，这不是现实，只是对现实的近似，由此得出的结果并不具结论性。于是下一个想法是让网格更密：更多的网格点给出更精细的近似，有望更接近解。然后让它更细、再更细，不断继续，直到它变得无穷细。如果我们能证明理论在这个极限下依然成立，就朝解决问题迈出了一大步。这时就该请出所谓的 Balaban 方法了：它给我们一种办法，去看改变网格尺寸会发生什么。粗网格上，我们有这个力如何行为的近似——当然不完美。现在把网格越调越细、越调越细。各位学者，抓稳你们的论文——哦天哪，你们看到我看到的了吗？

[120s - 240s]
We have a huge problem here. As we zoom into smaller and smaller scales, we start seeing more and more changes in the force field. These are called quantum fluctuations, and that is a huge problem. Why? Because making the grid finer doesn't automatically bring us closer to the solution. No, it makes the problem increasingly difficult, eventually spiraling out of control. That is bad news, and we have no proof that everything will settle down with a meaningful answer. So the question is: as we are entering the age of AI, can AI help solve this? Solve something that no human has ever done. Dear fellow scholars, this is Two Minute Papers with Dr. Károly Zsolnai-Fehér, and here is the clever part. OpenAI's AI system builds on Balaban's approach, but it tracks not just one grid, but multiple grids at the same time. Why? Well, because it can then calculate how tiny changes on the finest grids affect the coarser ones, and by tracking how changes at one scale affect the others, the AI can calculate their combined effect and show that finally, for the first time, the mathematics does not spiral out of control. Okay, but hold on. How do we know if the solution is correct? Well, the AI wrote this in Lean, a language in which the logic can be checked step by step, and the checks passed. Ha ha! Nice. But we are scientists here. For us, that is not enough. What if the AI proved something different from what we wanted? And now, fellow scholars, that's where we need humans to check the proof. A group led by Nobel Prize-winning physicist David Gross is on the case and is reviewing it. So what do they say? Well.

【译】我们遇到了一个大麻烦。当我们放大到越来越小的尺度，力场里开始出现越来越多的变化。这些被称为量子涨落，而这是个巨大的问题。为什么？因为把网格调细并不会自动让我们更接近解——不，它让问题越来越难，最终彻底失控。这是坏消息，而且我们没有任何证明表明一切最终会收敛出一个有意义的答案。于是问题来了：当我们进入 AI 的时代，AI 能帮忙解决这个从来没有人解决过的问题吗？各位学者，这里是 Two Minute Papers，我是 Károly Zsolnai-Fehér 博士。而这就是精妙之处：OpenAI 的 AI 系统建立在 Balaban 方法之上，但它同时追踪的不只是一张网格，而是多张网格。为什么？因为这样它就能计算出最细网格上的微小变化如何影响较粗的网格；通过追踪一个尺度上的变化如何影响其他尺度，AI 可以算出它们的综合效应，并表明——终于，第一次——数学没有失控。好，但是等等，我们怎么知道这个解是正确的？AI 把它写在了 Lean 里——一种逻辑可以被逐步检查的语言——而且检查通过了。哈哈，不错。但我们是科学家，对我们来说这还不够：万一 AI 证明的是和我们想要的不一样的东西呢？而这时候，各位学者，就需要人类来核查这个证明了。一个由诺贝尔奖得主、物理学家 David Gross 领衔的小组正在处理这件事，正在评审。那他们怎么说？嗯……

[240s - 360s]
Despite what you hear in some of the headlines and some videos, it's not done yet. The review is still underway, but I'll give you some info on this. The initial results are encouraging, but I don't envy these folks. Why is that? Well, because they are dealing with utter chaos. The code contains about 250,000 declarations. Think of these as small definitions and mathematical results. That's okay, sort of. But here is the crazy part: 60% of these declarations are not even needed in the final proof, and some of them are called cake and cookie. And I wish I was kidding. I am not. Now, unused code doesn't mean the proof is wrong. No, it means that it is incredibly challenging to understand. So, on one side, I am just a student. I am here to learn, and I am really excited for new knowledge, to learn new things. And look, I am not in any kind of relationship with OpenAI. They don't even answer my emails. This is an authentic channel where we talk about good and bad. So, at the same time, I kind of feel that OpenAI is dumping a bunch of spaghetti onto the science world and asking them to untangle it. I don't know. So, this is a proposed solution of the Yang-Mills existence problem. The mass gap part also remains unsolved. So, the million-dollar millennium problem is not fully solved yet. But we are living in an incredible time where the impossible becomes possible. What a time to be alive! I use Lambda to reproduce AI research papers often in minutes. It's great to train your own models, fine-tune an existing one, run inference, text to image or video, easy peasy. Running a deepseek chatbot or agent, super fast, super reliable. Lambda gives you powerful.

【译】尽管你在一些头条和视频里听到的说法并非如此——还没完成呢。评审仍在进行中，不过我可以透露一些信息：初步结果令人鼓舞，但我可不羡慕这些评审的人。为什么？因为他们面对的是彻底的混乱。这份代码包含大约 25 万条声明（declaration），可以把它们理解为一些小的定义和数学结果。这倒还好，某种程度上。但疯狂的部分来了：这些声明里有 60% 在最终证明里根本用不上，其中一些的名字还叫 cake（蛋糕）和 cookie（曲奇）。我真希望自己在开玩笑，但我没有。未使用的代码并不意味着证明是错的——不，它意味着这个证明极其难以理解。所以，一方面，我只是个学生，我是来学习的，我为新知识、为学到新东西而兴奋。还有，我和 OpenAI 没有任何关系，他们连我的邮件都不回。这是一个真诚的频道，好与坏我们都会谈。但与此同时，我又有点觉得，OpenAI 是把一大团意大利面倒在了科学界的桌上，然后请大家自己来解开。我说不好。所以，这是杨-米尔斯存在性问题的一个待定解（proposed solution），质量间隙部分仍未解决。这百万美元的千禧年大奖问题还没有被完全解决。但我们正活在一个不可能正变成可能的了不起的时代。能活在这个时代，真是太好了！我用 Lambda 复现 AI 研究论文，往往几分钟就好。训练你自己的模型、微调一个现成的模型、跑推理、文生图或文生视频，都轻而易举。跑一个 DeepSeek 聊天机器人或 agent，超级快、超级可靠。Lambda 给你强大的……

[360s - 373s]
NVIDIA GPUs to run your own experiments. I test ideas from papers I cover, and moments later, results. Love it. Seriously, try it out now at lambda.com/papers.

【译】……NVIDIA GPU，来跑你自己的实验。我会测试我报道的论文里的想法，片刻之后就有结果。我爱死了。说真的，现在就去 lambda.com/papers 试试吧。
