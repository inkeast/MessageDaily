---
title: "把数据冻在 11 月 2 日，让 AI 写出英伟达之后两周：All About AI 用 Sift 回测未来——50 个剧本、一条 −14% 的故事与「方向对了六天」"
date: 2026-11-28T00:16:26+08:00
draft: false
tags: ["灰色信源", "All About AI", "英伟达", "黄仁勋", "预测市场", "量化交易", "创业", "AI趋势"]
categories: ["podcast-summary"]
summary: "All About AI 频道主（本站此前文章记为 Kristian Fagerlie，Sift Intelligence 创始人）演示一个他称为「回测未来」的实验：把 Sift 收录的全部新闻在 2026 年 11 月 2 日 9:30 硬冻结、关闭联网搜索，让模型研究档案后写出未来两周的故事，并为每个交易日标一个收盘价。50 个剧本里最贴近现实的「黄仁勋间谍调查迫使 CEO 离任、英伟达跌 15%」收在 −14%、176 美元，真实跌幅约 20%（收 163）；AI 在 10 天里方向对了 6 天，国会传唤、董事会特别委员会都出现了，但「黄仁勋退居二线」实际是辞职且剧本早了三天，「特朗普撤回支持」没有发生。本文按视频口述整理，全部价格与日期为节目自述、未独立核实；文末附逐段中文翻译与 ASR 校正表。"
---
> **信源说明**：节目为 All About AI 频道《AI Is Just Getting Scary Now...》（YouTube，vid WqJ_RNRVxvI，队列元数据记录发布于 2026-11-26），音频约 16 分 20 秒，英文转写 9 段、首尾覆盖 0s–980s，中文翻译为本文作者整理。主持人未自报姓名，本站此前文章记为 Kristian Fagerlie（创业公司 Sift Intelligence）。视频内容窗口截至 2026-11-13 周五收盘，口述中提到黄仁勋「已经辞职」，录制应在 11/12 之后；全篇未提 11/23 复职。文中所有股价、涨跌幅与日期均为节目口述，本文未独立核实；屏幕上的来源列表与工具调用画面只能按口述转写。

## 先说结论

这是同一频道主两周内第三次做「让 AI 写未来、再拿现实对照」的实验，但这一期的方法多了一个关键约束：把数据**硬冻结**在 2026 年 11 月 2 日早上 9:30、关闭联网搜索，模型只能读那一刻之前 Sift 已收录的新闻档案，然后写出未来两周的故事，并为每个交易日标一个收盘价。50 个剧本里最贴近现实的一条——「黄仁勋间谍调查迫使 CEO 离任、英伟达跌 15%」——收在 −14%、176 美元，真实情况是从 10/30 起跌约 20%、11/13 收 163；解说视频称 AI 在 10 天里方向对了 6 天，国会传唤与董事会特别委员会都真的出现了，但「黄仁勋退居二线」实际是辞职、剧本早了三天，「特朗普撤回支持」则没有发生。频道主自己的结论很克制：「有点意思」「可能摸到了什么」，但还没决定要不要继续做。本文认为这期视频的价值不在「预测有多准」，而在它展示了一套可以被审计的事前剧本长什么样——以及展示时的挑选偏差有多容易让人高估它。

## 一、实验设计：硬冻结、断网、只许读档案（[0s–360s]）

频道主开场就说，屏幕上是他在模拟「11 月 1 日到 13 日左右发生在英伟达身上的各种剧情」，目的是看「能不能用 AI 回测未来」——也就是把已经发生的事当成一个可以回放的未来，让模型先写一遍，再拿黄线（真实股价）对照。他展示了英伟达从约 200 美元跌到周五 13 日约 160 美元的曲线，并说「这里就是剧情开始的地方」。

方法上他强调了三件事。第一，他在自己创业公司搭的系统里可以做「硬冻结」：把采集到的全部数据冻结在某个设定的日期与时刻，这次是 11 月 2 日 9:30，模型对之后发生的任何事都一无所知。第二，联网搜索与一切外部工具关闭，但模型可以读从开始采集到冻结时刻的全部新闻，「形成一个到那一刻为止世界长什么样的图景」。第三，把它变成循环：可以跑一百个样本（这次实际生成 50 个故事），每次给模型的指令大意是——「现在是 11 月 2 日星期一早上，研究所有新闻，去读我们的档案，然后写一个关于接下来两周的故事；对其中每个工作日，从那个故事里挑一个你认为股价会到哪里的点，故事偏负面就往下画，偏正面就往上画」。他特别说明这不是「预测未来」，更像「模拟未来」。

现场他打开 Claude Code 重新跑了一遍，设定 10 个故事，并刻意不引导方向——可以跑一个长线故事、一个看涨故事，「保持中性」，因为「我们不想把 AI 引向去找一个负面的故事」。第一个出来的是一条很平的故事：「英伟达因 AI 硬件财报反弹，随后被利率与 Anthropic 的供应打断」。

## 二、50 个剧本的分布，和那条 −14% 的故事（[360s–480s]）

点开任何一条故事，报告的结构是一样的：逐日给出当天收盘价与前一天的上下文延续；每条判断都标注证据来源（他念出的有 Fool.com、24WallStreet.com、FinanceYahoo.com，均为档案库里抓到的报道）；然后是「假设」「缺什么」「什么会打破这个故事」——例如「对 MegaSpeed 的正式 DOJ 指控，或一条限制远程算力访问的商务规则，会让英伟达下跌几个百分点」；页面底部列出得出这条故事用到的全部工具调用。

第二条他挑出来看的是 −9.1%：「MegaSpeed（一家上市的英伟达云伙伴）受到司法部调查，一份情报报告成为测试案例。」他说这条故事的线索最早在 10 月 29 日左右就出现在档案里、当时没什么人注意，模型把它捡起来，围绕「MegaSpeed 高管 Alice Huang」的故事造了一个情景——「跟实际发生的不是一回事，但它摸到了什么」，并给两周定了 9% 的跌幅（实际跌得更多）。

## 三、50 次大跑里的离群值：−14%、收 175（[480s–600s]）

真正让他「停下来」的是前一天跑的 50 次大样本。分布仍围绕中位数，但有离群值，最极端的一条是 −14%、收 175 美元：「黄仁勋间谍调查迫使 CEO 离任，英伟达跌 15%。」故事里写道：一篇纽约时报报道说美国情报机构判定 MegaSpeed 高管是「接近黄仁勋的中国信息收集者」，特朗普什么都没做；中期选举之后，国会致函要求黄仁勋作证或接受简报；到 11 月 9 日，在这个 AI 故事里，黄仁勋退出了 CEO 位置。他的反应是「这太准了」，随后给出了他理解的产品用法：如果你在找风险情景，可以把所有绿色（上涨）的故事去掉，只看最差的十条、十五条——「找市场还没看到的风险」；但他也强调需要其他故事来保持平衡。

这里要把「他展示的」和「他跑出的」分开看：他自己说 50 条故事「没有一条一路跌到（真实低点）」，−14% 是 50 条里最悲观的一条，也是事后挑出来最像的一条。这个挑选动作本身没有问题——它是产品演示——但它意味着观众看到的命中率不是这套系统的基准命中率。

## 四、逐日对照：AI 的十天，哪些真、哪些假（[600s–720s]、[720s–840s]）

他播了一段用 AI 故事做成的解说视频（旁白为 AI 生成，他自述「短版」），逐日把剧本与现实并列。以下按旁白口述整理（涨跌幅均为节目数字）：

| 日期 | AI 剧本 | 现实（节目口述） |
| --- | --- | --- |
| 11/2 周一 | 英伟达为黄仁勋辩护，官员称他不知情；后续报道让间谍故事继续发酵，股价反弹 | 跌 1.3%，仍在 200 美元上方 |
| 11/3 周二（选举日） | 后续报道点名对 Alice Huang 与黄仁勋接触的反情报审查 | 涨 2.3%，市场等待中期选举结果与就业数据 |
| 11/4 周三 | 国会致函要求黄仁勋作证或接受机密简报，限期很短 | 跌 3.2%（政治风险在累积，公司业务没变） |
| 11/5 周四 | FBI 反情报审查的报道出现，英伟达称黄仁勋配合，「治理折价」开始形成 | 跌 9.3%，窗口内最大单日跌幅 |
| 11/6 周五 | 董事会设特别委员会并聘外部律师，黄仁勋退出对华许可决策；同晨火热的就业报告推高收益率 | 反弹约 3% |
| 11/9 周一 | 黄仁勋在审查期间退居二线，开盘前任命临时主管，「华盛顿通道的溢价」消失（AI 版跌 9%） | 跌 4.3% |
| 11/10 周二 | 分析师重估失去黄仁勋的风险，「中国的上行」从股票故事里剔除 | 跌 5.5%，窗口内最低收盘 |
| 11/11 周三（退伍军人节） | 临时主管稳住公司，价值买家在约 25 倍前瞻市盈率附近回归 | 涨近 3% |
| 11/12 周四 | 董事会对黄仁勋接触的审查仍在进行，投资者等待 17 日的财报 | 漂移下跌 4.3% |
| 11/13 周五 | 两周结束时黄仁勋已不在 CEO 位、股价接近 176 | 收 163，当日跌 2% |

旁白的收束数字：整个窗口 AI 故事跌约 14% 到 176，真实股价从 10/30 起跌约 20%；「AI 在 10 天里方向对了 6 天，但它的关键事件仍未被证实。」需要提醒的是，这张表是旁白（AI 生成内容）的陈述，与主持人此前「从约 200 跌到约 160」的口述大体一致，但本文无法核对每一天的真实收盘价。

## 五、他请 Claude 做的事实核查，与这东西到底怎么用（[840s–960s]）

视频最后一段是频道主自己请 Claude 对这条 −14% 的故事做逐项核对，结果如下（按他的口述）：后续报道与国会致函——「这发生了，参议院委员会要求黄仁勋作证」；董事会设独立董事特别委员会——「发生了，但比我们故事里早两天」；黄仁勋退居 CEO——「实际发生的是黄仁勋辞职，而我们早了三天」；特朗普停止支持黄仁勋——「没有发生，我认为他实际上支持了他」，加上火热的 CPI 之类。他的总结是「相当准确」，并且「只是觉得有意思」。

他还顺带交代了身份与动机：他现在主要在做 Sift Intelligence 这家初创公司，有早期访问版但还没放很多人进来，「明年年初」会有更多；这期视频也是为了「记录我的创业公司」。结尾预告会用模型做 2027 年预测。

## 行业意味着什么、下一步看什么

**机制一：硬冻结把「事后解释」变成「可审计的事前剧本」。** 变化是数据可用性被时间戳截断（[120s–240s]）；直接原因是模型再也不能从未来新闻里「偷看」；得到的是一类新的证据形态——一个在 T 时刻生成、带着逐日价格点与证伪条件的剧本，可以与 T 之后的现实逐项对照。二阶影响是预测类 AI 产品的评价标准可能从「说得像不像」转向「冻结时刻是否可验证、剧本有没有可观测的先行指标」。对读者的启发很朴素：看到任何「AI 预测对了」的演示，先问它看没看未来数据，再看它展示的是全部样本还是最佳样本——本期两件事一件做对了（冻结），一件没做（只展示最贴近的一条）。

**机制二：政治风险的叙事链比价格点位更容易被模型抓住。** 信号是 10/29 一条当时关注度不高的报道（MegaSpeed 高管的间谍指控）；模型把它外推成一条逐级升级的链——国会 → 董事会 → CEO 离任（[480s–600s]）；谁受影响的是把「华盛顿通道溢价」定价进股票的持有者，解说视频在 11/9 那一格正是这样写的。但时点与幅度的误差很大：关键事件早了三天、幅度少了约六个百分点，10 天里方向只对了 6 天。启发：这类工具的强项是「把一条被低估的线索放大成可以讨论的情景」，弱项是「告诉你哪一天、跌多少」；用它做风险清单比用它做点位预测靠谱。

**机制三：中性采样 + 事后挑选，决定了它该被当成议题生成器而非预警系统。** 他刻意让 10 条现场样本「不引导方向」（[240s–360s]），这是对的；但给观众看的是 50 条里最悲观也最贴近的一条（[480s–600s]）。二阶影响是产品演示里的命中率天然偏高，用户若照此建立预期会失望。可观察指标：如果 Sift 后续开放，看它是否公布「全分布对基准（随机、恒定方向）的胜率」而不只是最佳故事——本站 11/10 对 Sift 复盘 11·5 的整理已经得出过同一结论：方向能指认，时点与幅度测不出。

这期视频也是频道主在 11/23 黑天鹅一期里预告的方向的兑现——当时他说「黄仁勋会不会被解雇、会不会复职」不是黑天鹅，但「可以用同一套设置去构想情景」。至于 11/13 之后的真实走向（黄仁勋 11/12 辞职、11/23 复职、股价单日大涨），本期没有覆盖，见本站对 The AI Daily Brief 复盘的整理。

## 信源与口径

- 节目：All About AI《AI Is Just Getting Scary Now...》（YouTube，vid WqJ_RNRVxvI，队列元数据记录 2026-11-26 发布），音频约 16 分 20 秒，英文转写 9 段（0s–980s），中文翻译为本文作者整理；视频标题「AI 现在只是变得吓人了」为频道主的营销式标题，正文主话题是「用 AI 回测未来」的英伟达实验。
- 主持人身份依据本站此前对该频道的整理（[Sift 复盘 11·5 美股](https://inkeast.github.io/MessageDaily/posts/2026-11-10-all-about-ai-sift-nov5-postmortem/)、[Sift 黑天鹅情景](https://inkeast.github.io/MessageDaily/posts/2026-11-25-all-about-ai-sift-black-swan-scenarios/)），本期音频未自报姓名；他自述经营 Sift Intelligence 初创公司，本期视频带有产品演示性质。
- 全部股价、涨跌幅、日期、「6/10 天方向正确」等均为节目口述或其播放的 AI 生成旁白所述，本文未独立核实；对照背景可参考本站对黄仁勋辞职与复职的整理（[如闪电般归来](https://inkeast.github.io/MessageDaily/posts/2026-11-18-quanliwai-197-huang-lightning-return/)、[太大而不能倒来到了芯片业](https://inkeast.github.io/MessageDaily/posts/2026-11-25-ai-daily-brief-jensen-back-white-house-too-big-to-fail/)）。
- 实验所用模型名、样本数（口述提到「一百个」与实际 50 个）以口述为准；「展示偏差」「应与基准比较」等判断为本文写作者推断，非节目观点。

## ASR 校正表

| 转写原文 | 修正 / 处理 | 依据 |
| --- | --- | --- |
| close to Jensen Wong / demands Wong testify | Jensen Huang / demands Huang testify | 同一转写其余处（[600s–720s]）及本站既有文章均作 Huang |
| MegaSpeed Alice Wong story | MegaSpeed Alice Huang story | 同一转写 [600s–720s] 与本站 65 处均作 Alice Huang |
| sought out Hong's testimony | sought out Huang's testimony | 上下文为参议院委员会要求黄仁勋作证 |
| 24WallStreet.com | 保留原文；转写疑似 247WallSt.com（24/7 Wall St.） | 屏幕来源列表无法核实，不替换 |
| the midterm election reaction to AMD | 保留原文 | 语义不明，疑为「the midterm election reaction, and AMD」，无法确认 |

## 转写原文（修正版）

[0s - 120s]

So what you see here is me simulating all kind of the drama that has been happening to Nvidia from this is from November first, I think, all the way up to November 13th. So what I wanted to do was to see if we can use AI to backtest the future and simulate exactly what did actually happen. So here we are exploring 50 different stories, and I also have the model to plot in each day where it thinks the stock price is going to end up, and then we can kind of compare it. You will see a yellow line soon. This is where what really happened. So yeah, I'm just going to tell you a bit about this experiment I did today, actually testing the future with a backtest, right? So yeah, let's just get into it. Let's go day by day. Monday, November 2nd, Nvidia defends Huang, and U.S. officials did not believe he knew of her role. In the AI story, follow-on reporting keeps the spy story alive. And the stock. So yeah, like I said, can we actually use AI to actually backtest the future? So this is pretty interesting, and this something I've been working on for a while now. And I think the Nvidia drama the last few weeks, since we have recorded all the data, and you can see here I have the option to freeze the data before November 2nd. You can see here from the the equity graph that this is where all the drama started, right? The downturn in Nvidia from like 200 all the way down to like 160, at least on Friday 13th. It went even, of course, further down, but I think two weeks is fine. So how do we actually do this? So in my system, the system I have set up my startup, we can do like a hard freeze, so we can freeze all the data we have collected on like a set date and time. So in this test, our model or AI model we are using will not know anything after November.

【译】你们现在看到的，是我在模拟发生在英伟达身上的各种剧情——我想是从 11 月 1 日，一直到 11 月 13 日。我想做的是看看能不能用 AI 回测未来，把实际发生的事一模一样地模拟出来。所以这里我们在探索 50 个不同的故事，我还让模型为每一天标出它认为股价会收在哪里，然后我们可以拿来比较。你们马上会看到一条黄线，那是真实发生的情况。好，我就来讲讲我今天做的这个实验——其实是「回测未来」对吧？我们一天一天来。11 月 2 日星期一，英伟达为黄仁勋辩护，美国官员表示不相信他知晓她的角色。在 AI 的故事里，后续报道让间谍故事继续存活。然后是股价。就像我说的，我们到底能不能用 AI 回测未来？这非常有意思，这是我已经做了一阵子的事情。我认为英伟达过去几周的剧情——既然我们已经录下了全部数据，你们可以看到这里我有一个选项，可以把数据冻结在 11 月 2 日之前。从权益曲线可以看到，这里就是所有剧情开始的地方，对吧？英伟达的下跌，从大约 200 一路跌到大约 160，至少到周五 13 日是这样。它后来当然跌得更多，但我想两周就够了。那我们

[120s - 240s]

Second, nine thirty, and of course, web web search and everything is disabled. But it can read all the news up to this date, from where we started collecting, right? So it can kind of form an image of how the world looks all the way up to this date. And then we turn this into a loop, right? So we're gonna do we could do like hundred examples. So we feed it in a question. I think the question we're gonna use is something like, I guess I can show you the question. So if you kind of look at the setup here, this is basically what we are telling our model. So we just tell AI it's Monday morning, November second, right? Research all the news. So go look at our archive. Then you're gonna write a story about the next two weeks. And for every day of that week, okay, I guess weekdays, you're gonna from that story, you're gonna pick a spot where you wanna plot the stock price. Is the story negative? You're gonna of course probably plot it downwards. If it's positive, you're gonna pick it upwards, right? Just do that over and over again. Come up with different stories, scenarios based on the research we have available from November second, right? So we're gonna extrapolate from like a thread, like a story the model selects, and from there it's gonna try to yeah, not predict the future, like almost simulate the future and see where kind of we end up with the stock price after 14 days. That is basically the experiment. And then we're gonna check, right? We end up with this line here. So one story was very positive, one was negative, and here you can kind of see. So we have number one is the closest story, and you can see this is a real finding. So here you can see how good this line tracked our the actual graph, right? So we get like a coverage and stuff like that. So what is gonna be interesting now is to actually see the results because I think you will kind of be surprised how well these small

【译】实际上是怎么做的？在我的系统里——我为自己的创业公司搭的这套系统——可以做「硬冻结」，可以把我们采集到的全部数据冻结在某个设定的日期和时刻。在这项测试里，我们的模型、也就是我们用的 AI 模型，对 11 月 2 日 9:30 之后的任何事都一无所知。当然，联网搜索和一切外部工具都是禁用的。但它可以读到这一刻之前的全部新闻，也就是从我们开始采集算起，对吧？所以它能形成一个到这一刻为止世界长什么样的图景。然后我们把它变成一个循环，对吧？我们可以跑一百个样本。我们把问题喂进去。我想我们要用的问题大概是——我猜可以把问题给你们看看。看这里的设置，这基本上就是我们告诉模型的：我们告诉它现在是 11 月 2 日星期一早上，对吧？研究所有新闻，去读我们的档案。然后你要写一个关于接下来两周的故事。对那一周的每一天——我想是指工作日——你要从那个故事里挑一个你想标股价的位置。故事偏负面？你多半会往下画。偏正面，你就往上画，对吧？就这样一遍又一遍地做。基于我们手头截至 11 月 2 日的研究，想出不同的故事、不同的情景，对吧？所以我们要从某条线索外推，就像模型自己选的一个故事，然后它要试着——不是预测未来，而是几乎模拟未来，看 14 天后股价会落到哪里。这基本上就是实验。然后我们来核对，对吧？最后得到这里这条线。所以

[240s - 360s]

Models are at kind of dreaming up these scenarios, but of course it has a clue. But still, I was very impressed, and I want to show you the results. So before I show you the result, I want to show you how this works now, so you can see these are the results for this is generated 50 stories, and you can kind of see the spread from all the. So here is kind of a real path, right? And you can see none of these stories came all the way down, but you can see we have a big spread here. So let me show you how this works now. If we go to the one we set up, you can see we start with this real graph of what actual did happen. Like I said, so the the AI doesn't know that. So if we head to Claude Code now, and we can just start this, right? So when we run this now, we actually want to kick in kind of the loop, and now it's just gonna start coming up with stories, coming up with stock prices, and it's gonna plot them on this graph here. So I set this up to generate 10 stories. They could be any story. We're not gonna kind of push it in one direction. We could push it in like one long shot story. We can do one bullish story. So it's kind of neutral. So we don't kind of lead the AI into looking for like a negative story. That's not something we want to do. So yeah, I'm just gonna let this run now, and I'm gonna take you back when we have the first story. Let's take a look at it and kind of how it looks, and then we're just gonna push on to the next 10 here. So now we can see we have our first story here. So let's take a look at what happened. So this was like a very flat stories. So this is like Nvidia rebounds on AI hardware earnings, then rates and Anthropic supply undo it. So we can click on this story, right? And here we can read all the things or kind of the story the AI is kind of I like to call it dreaming up or like creating the scenario for.

【译】有一个故事非常正面，一个是负面的，这里你们可以看到。排名第一的是最接近的故事，你们可以看到这是一个真实的发现。这里你们可以看到这条线对我们的真实曲线跟踪得有多好，对吧？我们得到覆盖率之类的东西。现在有意思的其实是看结果，因为我想你们会有点惊讶，这些小模型居然这么会做梦出这些情景——当然它是有线索的。但我还是很震撼，我想把结果给你们看。在展示结果之前，我想先给你们看这是怎么运作的。这些是生成的 50 个故事的结果，你们可以看到所有这些的分布。这里是真实的路径，对吧？你们可以看到没有一个故事一路跌到底，但可以看到这里有一个很大的分布。那我给你们看看它怎么运作。如果我们回到我们设置的那一个，可以看到我们从真实发生的曲线开始——像我说的，AI 并不知道这条曲线。如果我们打开 Claude Code，就可以直接启动它，对吧？现在运行，我们其实要启动这个循环，现在它就会开始想出故事、想出股价，然后把它们画到这张图上。我把它设置成生成 10 个故事。可以是任何故事。我们不会把它往某个方向推。可以跑一个长线故事，可以跑一个看涨故事。所以它是中性的，我们不会引导 AI 去找一个负面的故事，那不是我们想做的。好，我现在让它跑，等第一个故事出来我再带你们回来看。看看它长什么样，然后我们接着看下面 10 个。现在我们可以看到

[360s - 480s]

So it goes through every day, puts a stock price or like a story close stock price for that day, and of course it keeps the context from day to day. So you can see this is kind of it. It do actually mention MegaSpeed here. That's interesting. Congressional attention to MegaSpeed grows, and the midterm election reaction to AMD. So it has a story for every day here. Here we can kind of read more into every every day. First visible signs, and all of this is grounded right in evidence. So we have evidence that we capture. So this is from Fool.com, 24WallStreet.com, FinanceYahoo.com. So all of this is grounded from our database, right? We have assumptions. What is missing? What the story? What would break the story, right? So for example, if a formal DOJ charge against MegaSpeed or a commerce rule restricting remote compute access would knock Nvidia down several percent. So this is what could break the story. So this is kind of the report we get. Here we can see all the tool calls we made, right? To come to this, and yeah, that is basically it. And every story kind of gets plotted here, right? Okay, so now we have all the the simulated stories here, and we got a nice spread from like under the median and a bit over. So good representation here. So was there anything other interesting one? Yeah, we found one pretty interesting one here minus nine point one percent. Yeah, here we can see MegaSpeed, a listed Nvidia cloud partner, under Justice Department's probe, and an intelligence report becomes the test case. So here we can kind of take some threads from the MegaSpeed, and yeah, kind of a story that spawned like 29th of October or something was very early. Wasn't a lot of

【译】我们的第一个故事。看看发生了什么。这是一条非常平的故事：「英伟达因 AI 硬件财报反弹，随后被利率与 Anthropic 的供应打断。」我们可以点开这个故事，这里可以读到 AI——我喜欢说它是在做梦、或者说在创造情景——写出的所有内容。它过了一天又一天，给出当天的股价或者说故事里当天的收盘价，当然它保留了从一天到下一天的上下文。可以看到它确实在这里提到了 MegaSpeed，这很有意思：「国会对 MegaSpeed 的关注升温，中期选举对 AMD 的反应。」它对每一天都有一个故事。这里我们可以读每一天的更多内容。「最初的可见迹象」，而所有这些都扎根在证据上。我们有采集到的证据：这来自 Fool.com、24WallStreet.com、FinanceYahoo.com。所有这些都来自我们的数据库，对吧？我们有假设。缺什么？这个故事、什么会打破这个故事，对吧？比如，对 MegaSpeed 的正式 DOJ 指控，或者一条限制远程算力访问的商务规则，会让英伟达下跌几个百分点。这就是可能打破这个故事的东西。这基本上就是我们得到的报告。这里可以看到为了得出它我们做的全部工具调用，对吧，基本上就是这样。每个故事都会画到这里。好，现在我们有了全部模拟出来的故事，得到了一个不错的分布，从中位数下方一点到上方一点，所以这里的代表性不错。还有没有别的有意思的？有，我们找到了一个挺有意思的，负 9.1%。这里可以看到：「MegaSpeed，一家上市的英伟达云伙伴，受到司法部调查，一份情报

[480s - 600s]

Attention around it, but here kind of the model picks this story up and it tries to create some kind of scenarios around this MegaSpeed Alice Huang story. Right? It's not exactly what happened, but it's onto something. And it priced this like a nine percent drop in the next two weeks. Of course, it did drop much more, but yeah, you kind of get the point here out. So now I want to show you what happened when I ran this in a bigger run yesterday. I ran this like fifty times, right? And here you can see the spread. Of course, is around the median, but we have some outliers here. The most interesting one is the minus fourteen percent, one seventy-five close. Huang espionage probe forces CEO CEO leave. Nvidia falls fifteen percent. That is just so accurate what happens. So a NYT report says U.S. intelligence judged MegaSpeed executive Chinese collector close to Jensen Huang. Trump did nothing. That is what happened, right? Now you can see on day what day was it? After midterms, congressional letter demands Huang testify or receive a briefing. And on October or like November ninth, here in this AI story, Huang step back at CEO. So that is pretty crazy. I kind of stopped up when I saw that. So in theory, just to keep it like if you're looking for like risky situations, you can just remove all the green stories here and then you just look at kind of the top ten. But we need those other stories to keep it balanced, right? So we could just look at the top fifteen here when if we want to look for risk that the market hasn't seen, right? So that is just super interesting. So I want to play you. I created like a small animated video from what the AI story was. So let me just.

【译】报告成为测试案例。」所以我们可以从 MegaSpeed 这里拿一些线索，这个——大概是 10 月 29 日左右出现的故事——很早，当时没多少关注度，但模型把这个故事捡了起来，试着围绕这个 MegaSpeed、Alice Huang 的故事造一些情景，对吧？它跟实际发生的不是一回事，但它摸到了什么。它给接下来两周定价了 9% 的下跌。当然，它实际上跌得多得多，但你们懂我的意思。现在我想给你们看昨天我跑更大一轮时发生了什么。我跑了大概 50 次，对吧？这里可以看到分布，当然是围绕中位数的，但我们这里有一些离群值。最有意思的是负 14% 的那个、收 175：「黄仁勋间谍调查迫使 CEO 离任，英伟达跌 15%。」这跟发生的实际情况简直太准了。一篇纽约时报报道说，美国情报机构判定 MegaSpeed 高管是中国信息收集者、与黄仁勋接近。特朗普什么都没做。这就是发生了的事，对吧？现在可以看到是哪一天——中期选举之后，国会致函要求黄仁勋作证或接受一次简报。然后在 11 月 9 日，在这个 AI 故事里，黄仁勋退出了 CEO 的位置。这太疯狂了，我看到的时候停了一下。理论上说——简单讲——如果你在找风险情景，你可以把所有绿色的故事去掉，然后只看最差的十条。但我们需要其他那些故事来保持平衡，对吧？所以如果我们想

[600s - 720s]

Play you that, and you can compare yourself how close it was to what did actually happen. Here's the short version. On October 29th, the New York Times reported that U.S. intelligence judged Alice Huang, an executive at Megaspeed, to be a Chinese information gatherer, and that she is close to Jensen Huang. No one has been charged. The AI then wrote what might follow: a congressional push, a board committee, and Huang stepping back as CEO. Let's go day by day. Monday, November 2nd, Nvidia defends Huang, and U.S. officials did not believe he knew of her role. In the AI's story, follow-on reporting keeps the spy story alive, and the stock bounces. In reality, it slips 1.3%, but stays above $200. Tuesday, election day. In the AI's story, a follow-up report names a counterintelligence review of Alice Huang's contacts with Jensen Huang. In reality, the stock rises 2.3%, while the market waits for the midterm results and the jobs data. Wednesday, in the AI's story, a congressional letter demands that Huang testify or take a classified briefing with a short deadline. In reality, the stock falls 3.2%. The political risk is building, but nothing in the company's business has changed. Thursday, in the AI's story, reports of an FBI counterintelligence review appear, and Nvidia says Huang is cooperating. This is where a governance discount starts to form. In reality, the stock falls 9.3%, the biggest drop in the window. Friday, November 6th. In the AI's story, Nvidia's board forms a special committee with outside lawyers, and Huang steps away from China licensing decisions. The same morning.

【译】找市场还没看到的风险，我们可以只看最差的十五条。这非常有意思。我想给你们放一段我用 AI 故事做的小动画视频。让我播放给你们看，你们自己比较一下它跟实际发生的有多接近。这是短版。10 月 29 日，纽约时报报道美国情报机构判定 MegaSpeed 高管 Alice Huang 是中国信息收集者，并且她与黄仁勋关系密切。没有人被起诉。AI 接着写下可能随之而来的事：国会施压、一个董事会委员会，以及黄仁勋退离 CEO 位置。我们一天一天来。11 月 2 日星期一，英伟达为黄仁勋辩护，美国官员表示不相信他知晓她的角色。在 AI 的故事里，后续报道让间谍故事继续存活，股价反弹。现实中，它下跌 1.3%，但保持在 200 美元上方。星期二，选举日。在 AI 的故事里，一篇后续报道点名了对 Alice Huang 与黄仁勋接触的反情报审查。现实中，股价上涨 2.3%，市场等待中期选举结果和就业数据。星期三，在 AI 的故事里，国会致函要求黄仁勋作证，或在很短的限期内接受一次机密简报。现实中，股价下跌 3.2%。政治风险在累积，但公司的业务没有任何变化。星期四，在 AI 的故事里，出现了 FBI 反情报审查的报道，英伟达称黄仁勋在配合。这是一个「治理折价」开始形成的时刻。现实中，股价下跌 9.3%，是窗口内最大的跌幅。11 月 6 日星期五。在 AI 的故事里，英伟达董事会组成了一个

[720s - 840s]

A hot jobs report pushes yields up. In reality, the stock bounces about three percent. Monday, November ninth. In the AI story, Huang steps back as CEO pending the review, and an interim executive is named before the open. The premium for his Washington access goes. In reality, the stock falls four point three percent. The AI's version falls nine percent. Tuesday. In the AI story, analysts reprice the risk of losing Huang, and the China upside is removed from the stock story. In reality, the stock falls five point five percent to the lowest close in the window. Wednesday, Veterans Day. In the AI story, an interim executive steadies the company, and value buyers return near twenty-five times forward earnings. In reality, the stock rises almost three percent. Thursday. In the AI story, the board's review of Huang's contacts is still running, and investors wait for the earnings report on the seventeenth. In reality, the stock drifts four point three percent lower. Friday, November thirteenth. In the AI story, the two weeks end with Huang out as CEO and the stock near one seventy six. In reality, it closed at one sixty three, two percent lower on the day. Over the window, the AI stock falls about fourteen percent to one seventy six. The real stock fell about twenty percent from October thirtieth. The AI got the direction right on six of ten days, but its key event is still unproven. So yeah, I just found this interesting, right? And I had a lot of fun kind of simulating this, and I haven't really decided yet if I'm going to keep investigating this, but I think we are kind of onto something here, and I'm going to keep the.

【译】由外部律师组成的特别委员会，黄仁勋退出对华许可决策。同一个早晨，一份火热的就业报告推高了收益率。现实中，股价反弹了大约 3%。11 月 9 日星期一。在 AI 的故事里，黄仁勋在审查期间退居 CEO 二线，开盘前任命了一位临时主管。他身上那种「华盛顿通道」的溢价消失了。现实中，股价下跌 4.3%。AI 的版本跌 9%。星期二。在 AI 的故事里，分析师重新定价失去黄仁勋的风险，「中国的上行」从这只股票的故事里被剔除。现实中，股价下跌 5.5%，收在窗口内的最低点。星期三，退伍军人节。在 AI 的故事里，一位临时主管稳住了公司，价值买家在约 25 倍前瞻市盈率附近回归。现实中，股价上涨近 3%。星期四。在 AI 的故事里，董事会对黄仁勋接触情况的审查仍在进行，投资者等待 17 日的财报。现实中，股价漂移下行 4.3%。11 月 13 日星期五。在 AI 的故事里，这两周结束时黄仁勋已经不在 CEO 位置、股价接近 176。现实中，它收在 163，当天下跌 2%。整个窗口里，AI 的股价跌了大约 14% 到 176。真实的股价从 10 月 30 日起跌了大约 20%。AI 在 10 天里有 6 天方向对了，但它的关键事件仍未被证实。是啊，我就是觉得这很有意思，对吧？我做这个模拟做得很开心，

[840s - 960s]

Developing this feature, trying to backtest the future. I had done it before, but having this kind of hit, like I know, like we do a lot of simulations, and sometimes you would just see something like. But I thought it did a really good job, kind of developing the story from kind of all the way back to November second, and extrapolating that out over the next two weeks with pretty good accuracy. To be honest, I did ask actually Claude to kind of fact check this story, and you can see here kind of what happened. So what it predicted, follow up reporting, and congressional letter. This did happen. A Senate committee sought out Huang's testimony, so that did happen. Board forms a special committee or of an independent directors. This did happen, but it was two days earlier than in our story. It was two days earlier than it did happen. Huang steps back as CEO. What happened was that Huang resigned, but also we were three days early. Trump stops backing Huang. That didn't happen. I think he actually backed him and hot CPI and stuff like that. So we did. It was pretty accurate. So so it was pretty accurate. I I just found it interesting. So I just wanted to share this and turn this into a video because yeah, I like to document my startup, and I can look back on this right. So if you didn't know, I run the Sift Intelligence startup. This is what I'm mostly doing now. Like I have an early access, but I haven't had the opportunity yet to let many people in and try it. But I will come back with more on that hopefully early next year and stuff like that. So yeah, if you enjoy this kind of content, I'm gonna do some 2027 predictions coming up. What I think is gonna happen next year, we're gonna use the model, maybe do some AI predictions.

【译】我还没决定要不要继续研究这个，但我认为我们有点摸到什么了，我会继续开发这个功能，试着回测未来。我以前做过，但像这样命中一次——我知道，我们做过很多次模拟，有时候你就是会看到类似的东西。但我认为它做得非常好，从 11 月 2 日开始一路发展这个故事，再把它外推到接下来的两周，准确度相当不错。说实话，我确实让 Claude 对这个故事做了事实核查，你们可以看到结果。它预测的：后续报道和国会致函——这发生了，参议院委员会要求黄仁勋作证，所以这发生了。董事会设特别委员会或独立董事委员会——这发生了，但比我们故事里早了两天，比实际发生的时间早两天。黄仁勋退居 CEO 二线——实际发生的是黄仁勋辞职了，而我们早了三天。特朗普停止支持黄仁勋——那没有发生。我认为他实际上是支持他的，还有火热的 CPI 之类。所以我们……它相当准确。是相当准确。我就是觉得有意思，所以想分享这个、把它做成视频，因为我喜欢记录我的创业公司，以后可以回头看。如果你不知道，我经营的是 Sift Intelligence 这家初创公司。这是我现在主要在做的事。像是有早期访问版，但还没机会放很多人进来试用。但我会在明年年初带来更多这类东西。所以，如果你喜欢这类内容，我会做一些 2027 年的预测。我觉得明年会发生什么，我们会用模型，也许做一些 AI 预测，

[960s - 980s]

And maybe some general world 2027 predictions soon. So if you like this kind of things, give this video a like, subscribe, and hopefully I'll see you again very soon. And I will update you if I find more interesting stuff in these types of videos. So yeah, thank you for tuning in. Have a great day, and I'll see you again soon.

【译】也许很快还会做一些 2027 年的整体世界预测。所以如果你喜欢这类东西，给这个视频点个赞、订阅一下，希望很快再见。如果我在这类视频里发现更多有意思的东西，我会更新你们。好，感谢收看，祝你今天愉快，我们很快再见。
