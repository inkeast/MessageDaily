---
title: "把 GTA 6 当考卷：Sift 八道预测题发售前留档——模型 56% 对市场 77%，现实给了 99"
date: 2026-11-20T12:42:55+08:00
draft: false
tags: ["灰色信源", "All About AI", "预测市场"]
categories: ["podcast-summary"]
summary: 'All About AI 频道主 Kristian 在 GTA 6 发售前两天，用自家 Sift Intelligence 对八道题留档预测：Metacritic 96+ 给 56%（市场赔率 77%）、首周 3000 万份仅 20%，六道剧情题从「Jason 或 Lucia 之死」的 70% 排到「可玩牢房」的 10%，每题附正反证据链。写作者注：发售后媒体分开出 99，市场与模型的双重保守都被现实超越；首周销量题尚未结算。这期真正值得带走的是预测系统评测方法：可结算题目、事前留档、市场对照、低概率不等于不可能。'
---

> 信源说明：本文基于 All About AI 频道一期视频整理（视频 ID `EPopZq-vJjA`，标题 "8 AI Predictions for GTA 6 — Will Jason or Lucia Die?"，全长约 11.1 分钟，英文转写 6 段、0s–667s 已完整读取，文中数字均依转写）。标题与内容相符——全片就是八道预测题的发售前留档与逐题解读。视频发布于 2026-11-17，两天后（11-19）GTA 6 正式发售，因此所有概率均为发售前口径；文中「发售后」一节的对照信息为写作者依公开报道补充，非视频内容。频道主经公开资料核实为挪威 AI 工程师 Kristian Fagerlie，其预测初创公司本系列统一记作 Sift Intelligence（口播名长期被转写成 sick/SICT 等，背景见旧文不再展开）。同系列前文：[《AI 预测股市第一周战绩》](https://inkeast.github.io/MessageDaily/posts/2026-10-13-all-about-ai-sift-intelligence-stock-prediction/)、[《把意外变成教材：Past/Scenario 双功能》](https://inkeast.github.io/MessageDaily/posts/2026-10-22-sift-past-scenario-backtesting/)、[《拿自家预测产品押注中期选举：赛前留档》](https://inkeast.github.io/MessageDaily/posts/2026-10-30-all-about-ai-sift-midterms-preview/)、[《Sift 周预测第 44 周》](https://inkeast.github.io/MessageDaily/posts/2026-11-01-all-about-ai-sift-weekly-predictions-w44/)、[《回到大跌前夜问 AI：Sift 复盘 11·5 美股》](https://inkeast.github.io/MessageDaily/posts/2026-11-10-all-about-ai-sift-nov5-postmortem/)。

## 先说结论

频道主 Kristian 把 GTA 6 当成了自家预测系统的考卷：发售前两天，他挑了八道题——两道硬数据题（Metacritic 媒体分、首周销量）加六道剧情题（主角生死、与他人恋爱、第三主角、NPC 拍摄、全裸漫游、可玩牢房）——每道题先摆正反证据，再由 Sift 模型盲跑出概率（模型看不到市场赔率），最后与市场赔率对照。结果是一个有意思的剖面：模型在两道硬题上都比市场保守（媒体分 56% 对市场 77%，首周 3000 万份只给 20%），剧情题里概率最高的是「Jason 或 Lucia 有一人会死」的 70%。频道主自己反复强调对这批预测「没什么信心」，因为游戏是模型从没测过的领域——这期视频真正值得带走的不是某个数字，而是它展示的预测系统评测方法：可结算的题目、事前留档、证据分层、市场对照。写作者注：截至本文发稿，两道硬数据题已有答案——Metacritic 开分 99，市场与模型的双重保守都被现实超越；首周销量题尚未结算。

## 一张混编考卷：两道硬数据题，六道剧情题

这期视频的起点很简单（0–120s）：GTA 6 几天后发售，频道主想「做点有意思的」，把硬数据题和游戏内问题混在一起测模型——口播里他先说挑了六道、马上自我纠正成八道。八道题依次是：Metacritic 分数线、Jason 或 Lucia 是否会死、首周销量能否过 3000 万份、两人能否与他人恋爱、主线有无第三可玩主角、NPC 能否拍下你发到游戏内社交应用、能否免模组全裸漫游、被逮捕后能否在牢房里继续玩（120–240s）。前两道与销量是可以用外部数据结算的「硬题」，其余六道要靠通关、玩家社群与挖掘验证。有一处口径前后不一需要交代：首段口播说 Metacritic「至少 97 分」，第三段正式读题时是「96 分或更高」，本文以读题口径 96 为准。数字他已提前跑完、做成了片头歌曲配乐的展示视频，正片就是边放边评；收尾时他强调两道「市场题」一定会盯，其余要靠 Reddit 和亲自游玩慢慢验证，并给自己留了台阶——「要把一切弄清楚还需要时间」。

## 硬数据题：模型比市场保守一截

**Metacritic 96 或更高**（240–360s）：市场赔率给 77%（博主未点名平台），Sift 模型给 56%。频道主个人「本会给得更高一点」，并当场自我诊断：可能是模型把题意理解偏了，他也不确定。模型的证据链由博主逐条转述：支持面——Rockstar 大作七次里四次拿到 96+，且每次失手都恰好停在 95（他推测是《荒野大镖客 2》、GTA 5、也许还有圣安地列斯那一档）；反对面——这十年最响的《艾尔登法环》、塞尔达、《博德之门》全都停在 96，另外主机只有 30 帧，在今天的媒体评分人手里可能扣分，而发售前根本不知道谁会来评分。他那句「96 本身就该够线了」的自言自语，其实已经说明连他自己都觉得 56% 偏低。

**首周 3000 万份**（360–480s）：模型只给 20%。支持面：需求创纪录，有一名分析师给出首发季度 4500 万份的预估；反对面：分析师们对整个首发季度的共识预期是 3400 万份，一周就卖 3000 万「有点狠」，而且首发没有 PC 版、只有主机，天然压住销量天花板。这一节里有个方法论细节值得记下：模型被明确设定为「看不到市场」，独立出概率再对照——市场是免费的校准基线，分歧点（56% 对 77%）就是信息所在。

## 剧情六题：用 Rockstar 的老规矩和泄漏线索定价

六道剧情题的概率从 70% 一路排到 10%，每题都有正反证据：

- **Jason 或 Lucia 会死（70%）**：支持——Rockstar 最近四款开放世界大作有三款杀了主角（《荒野大镖客》、GTA 5、《荒野大镖客 2》），官方又把本作定性为 Bonnie and Clyde 式故事，「大家都知道那对是怎么收场的」；GTA 系列本就惯用戏剧性结局。博主称这一题模型与市场相当接近。反对——发售前没有任何确认（转写里另有一句语义不明的「某种 CG 结局不算数」，见校正表）。
- **与他人恋爱（16%）**：支持——《圣安地列斯》和 GTA 4 都写过感情支线，《荒野大镖客 2》里有 Mary Linton，前作还会把前任、外遇或分手后的新欢藏进支线（此句转写存疑）；反对——博主称此前 24 条物料里 0 条提到这对之外的恋情，Rockstar 的 Rob Nelson 说过「他们必须是一对」，模型也认为整部游戏就围绕这对情侣展开。
- **第三可玩主角（21%）**：支持——两代《荒野大镖客》都在结尾把控制权交给新主角，且判定门槛很低，「一小段可玩剧情就算」；反对——目前展示过的所有内容（包括最终任务）都只在 Jason 和 Lucia 之间切换。
- **NPC 拍你并传上网（30%）**：支持——游戏已确认有多个可用的社交信息流、TikTok 风格应用、路人举手机拍你（此句转写存疑）；反对——没有任何主要预览提到 NPC 会把你的画面发上网，「这么好营销的特性如果存在，早就展示了」；但频道主补了一条判定细则：一个脚本化桥段（比如某次劫案任务）就足以判命中。
- **免模组全裸漫游（14%）**：线索来自泄露的裸体片段——频道主称记不清泄露者名字，只记得「那个 NFT 还是 meme coin 的人」；反对——片段里 NPC 没有任何反应，韩国分级机构据报道在测试中无法把两位主角完全脱衣；支持——分级清单列出了裸露内容，发售后「几百万玩家会去找 glitch」。频道主个人不信这条会出现，但仍让模型出了价。
- **被捕后可在牢房里玩（10%）**：五代主系列 GTA 都没有可玩牢房，被捕历来是「警局门口重生、扣一笔钱」了事；唯一的变数是通缉等级这次扩到了六星，「没人展示过六星会发生什么」。他的点评是「那会很酷，对吧」。

这套「历史统计 + 物料反推 + 机制变数」的定价思路，比任何单个结论都有看头：低概率从来不等于不可能——14% 的全裸和 10% 的牢房都写明了翻转路径（glitch 猎人、六星通缉），而判定条款（什么算命中）必须提前写死，否则事后无法结算。

## 发售之后：两道硬题先出了答案（写作者注）

以下为写作者依公开报道补充的对照，均非视频内容：

- **Metacritic：99。** 媒体评分 embargo 于 2026-11-18 早间解除，Metacritic 开出 99——Kotaku 记 145 篇评论，与 1998 年《塞尔达传说：时之笛》并列历史最高；Polygon 注意到它超过了 GTA 5 与《荒野大镖客 2》的 97；SteamDB 汇总 PS5 版 147 评 99、Xbox 版 14 评 99，OpenCritic 均分 98–99。「96 或更高」就此结算为命中——市场 77%、模型 56% 都显得保守，频道主那句「我个人会给更高」的直觉反而是全场最准的判断。
- **首周 3000 万份：尚未结算。** 据 BBC（11-20）与 Deadline 引述 Sensor Tower 估计，预购加首日销量超过 2450 万份、约 23.6 亿美元；Take-Two 官方未公布数字，同一数据源不同时点还有约 1905 万份的口径。首日 2400 万+意味着一周 3000 万并非天方夜谭，但在官方口径或满一周之前，本文不作结论。
- **剧情六题：本文不代为核实。** 涉及剧透，写作者搜索只见粉丝猜测与预发售理论帖、无权威信源交叉印证，按「核不到不写」处理——频道主自己说会去 Reddit 与游戏内核对，并可能出复盘视频，那是更合适的答案出处。

## 这期实验真正可迁移的：预测系统的评测方法

剥掉游戏外壳，这期视频是一份小型预测系统评测的样板，至少有三条链条值得搬走。其一，**评测要选可结算的题目并事前留档**：把「AI 行不行」拆成八道发售日前后自动结算的具体问题，配上发布时间戳，事后诸葛的质疑就无从谈起。其二，**市场对照是最好的免费基线**：模型盲跑、再与市场赔率比对，分歧点就是信息所在——而 99 分的现实同时超越了市场与模型，说明校准永远是对某个区间而言，尾部事件面前 77% 与 56% 的差别，远小于二者与 99 的现实之间的落差。其三，**能力边界要诚实标注**：频道主反复说自己对这批预测「没什么信心」，因为他的预测基准（博主称 Fable 5.2 刚超过 Opus 5.5 领先，模型名依转写）只覆盖其他领域，游戏是从未测过的 niche——这种跨域不置信的直觉本身就是重要信号。对任何想把模型预测引入自有领域的人，这期给出的操作清单是：定义可结算的题、写死判定条款、事前留档、盲跑对照市场、把「模型误解题意」这类失败模式当场记录下来。

## ASR 疑似转写错误校正表

| 原文 | 校正 | 依据 |
|---|---|---|
| my sixth intelligence system | my Sift intelligence system | 600s 段自述「我的创业公司」同名；本站系列文核实该产品口播名长期不稳定（sick/SICT），统一记作 Sift Intelligence |
| SICK Intelligence | Sift Intelligence | 同上 |
| Four, there's like a record demand / So four, we have / another four previous / that is kind of four（共 4 处） | 相应改为 For…（for/against 论据引导词） | 上下文均为列举支持方证据的同音误听 |
| Fable 5.2 just took the lead about Opus 5.5 | 保留原文，转写疑似 about 为 over；模型名未独立核实 | 无把握不改 |
| Awake for against awake cinematic ending wouldn't count | 保留原文，转写疑似「某种假结局 CG 不算数」类语义，不明 | 无把握不改 |
| previous may hide side stories and ex an affair | 保留原文，转写疑似 previous games hide…（前任/外遇/新欢） | 无把握不改 |
| So for these pieces like six, working social feeds | 保留原文，转写疑似 we've seen like six working social feeds | 无把握不改 |
| Previous say the same | 保留原文，转写疑似 Previous games, the same | 无把握不改 |

## 自检记录

事实账本（`work/pub1120/EPopZq-vJjA/ledger.md`）高优先级条目 25 条，正文落点 24 条，未采用 1 条：片头歌曲展示视频的制作花絮（120–240s，低价值；「已提前跑完数字并做成视频」这一事实本身已写入）。另有写作者注 3 条，均单独成节并标注信源。

## 转写原文（修正版）

### [0s - 120s]

Okay, so of course this is a big week for many, at least gamers, and Grand Theft Auto 6 is coming out in just a couple of days. So I wanted to do something. So I thought it could be interesting to try to do some AI predictions on a few questions about GTA 6, and I thought we could kind of mix it up by doing some like hard data questions and some in-game questions. So these are the six I picked out. Not six. Is it eight? Yeah, it's eight. And I'm gonna use my Sift intelligence system to try to predict these eight questions. So the first one is gonna be like Metacritic. Is it gonna be at least 97 or above? So will Jason or Lucia die in the story? Does GTA 6 sell more than 30 million copies in the first seven days? Can Jason and Lucia have other relationships or get into a relationship with someone else? And is there like a third protagonist you can play in the main story? NPCs can film you and post the videos in game on the in-game social app. I think that is something. Jason and Lucia completely naked in free roam without mods. This was some part of the leak, but I don't think it's gonna show up in the game. But let's see what our model says about that. And also, can you be arrested? And you.

【译】好，这周对很多人来说——至少对玩家们来说——是大日子，《侠盗猎车手 6》几天后就要发售了。所以我想做点东西。我想试试用 AI 对 GTA 6 的几个问题做一些预测，而且我想换个花样，把硬数据题和游戏内的剧情题混在一起。这些就是我挑出来的题目。是六道——不对，是六道吗？是八道，对，八道。我会用我的 Sift intelligence 系统来试着预测这八个问题。第一题大概是 Metacritic：会不会至少拿到 97 分及以上？Jason 或 Lucia 会在剧情里死吗？GTA 6 头七天能不能卖出 3000 万份以上？Jason 和 Lucia 能不能有别的感情关系、或者和别人谈起恋爱？主线故事里有没有第三个可以操作的主角？NPC 能不能拍下你、把视频发到游戏内的社交应用上——我觉得这个有点意思。还有，Jason 和 Lucia 能不能不打模组就在自由漫游里完全裸体。泄露内容里有这一块，但我不觉得它会出现在正式游戏里。不过咱们看看模型怎么说。另外，你能被逮捕吗？然后你……

### [120s - 240s]

Can kind of play inside a jail cell. So usually in GTA you just get arrested and you start over again. So it's going to be interesting to see if we kind of need to spend time in jail. I don't know that. But if we kind of mark this now, so we have the Metacritic, that's like a hard data question, and this is also like a hard data question. The rest is more like in-game questions. I thought that was a bit more interesting. So I have already run the numbers and I created like a video with the song I had in the intro, and there we see all numbers. So let's just play the video with all the numbers we predicted, and let's talk a bit about the outcome from our model.

【译】……能不能在牢房里面继续玩。通常在 GTA 里你被逮捕之后就一切从头再来，所以如果我们真的得在监狱里待上一段时间，那会很有意思。这个我不知道。但既然现在把判断记下来：Metacritic 是硬数据题，销量这也是硬数据题，剩下的大多算游戏内问题，我觉得这样更有意思。数字我已经提前跑完了，我用片头那首歌做了个视频，里面能看到全部数字。我们就放这个装着所有预测数字的视频，然后聊聊我们模型给出的结果。

### [240s - 360s]

Okay, so let's take a look here now. So you can see the first question we're gonna look at is: Will GTA Six score 96 or higher on Metacritic? So the market we have the odds here thinks 77% chance of doing that. Our model says 56. I would have personally been a bit higher, but it might have misunderstood the question here a bit. I'm not quite sure, but you can see for this, Rockstar's big game score 96 plus in four out of seven tries. Every miss was exactly 95, so that must be like Red Dead Redemption 2, probably GTA 5, maybe other GTA's, to San Andreas maybe. And against we have this decade's biggest hit, Elden Ring, Zelda, Baldur's Gate stopped at 96, but at 96 that should be good enough. So I don't know what happened here. And against 30 FPS only on consoles, that could cost points with today's critic. But we don't know for sure who is actually gonna rate this now before the game launches. So yeah. Second question is: Will Jason or Lucia die? So here we have 70%. The market is we are pretty close to the market here. So our model doesn't see the market, but they are kind of agreeing that there's like a seven out of ten chance that one of the characters will die. So you can see for Rockstar killed off a lead in three of its last four open world games: Red Dead, GTA 5, and Red Dead 2. Okay. Rockstar calls it a Bonnie and Clyde story. Everyone knows how that one ends. Yeah, they kind of. Yeah, you know that. Awake for against awake cinematic ending wouldn't count, and no, that has been confirmed yet. So yeah, something might happen. It's not unusual in the GTA games that they have some dramatic endings with the main protagonists. For question three, we have will GTA Six sell 30 million copies in the first week? Here our

【译】好，我们现在来看这里。你可以看到第一个问题是：GTA 6 在 Metacritic 上会不会拿到 96 分或更高？市场这边给的赔率认为有 77% 的机会，我们的模型给 56%。我个人本来会给得更高一点，可能是它把题意理解偏了一点，我不太确定。你可以看到，Rockstar 的大作七次里有四次拿到 96+，而每次失手都恰好是 95 分——那应该是《荒野大镖客 2》，可能是 GTA 5，也许还有别的 GTA，比如圣安地列斯。反过来（反对面）是这十年最大的爆款——《艾尔登法环》、塞尔达、《博德之门》——都停在 96，不过 96 分本身应该就够线了。所以我不知道这里出了什么状况。另外，主机上只有 30 帧，这在今天的媒体评分人手里可能会扣分。但在游戏发售前，我们确实不知道最终会是哪些人来评分。第二个问题：Jason 或 Lucia 会死吗？这里我们给 70%，和市场相当接近——我们的模型是看不到市场的，但两边差不多都同意：大概七成机会其中一个角色会死。支持面：Rockstar 最近四款开放世界大作里有三款杀了主角——《荒野大镖客》、GTA 5、《荒野大镖客 2》。Rockstar 把它称作 Bonnie and Clyde 式的故事，大家都知道那一对是怎么收场的。对，差不多。反对方（转写此处语义不明）：某种「醒来/假结局」式 CG 结局不算数，而且目前为止什么都没有得到证实。是的，可能会有事发生。GTA 系列里主角迎来戏剧性结局并不稀奇。第三题：GTA 6 首周能卖出 3000 万份吗？这里我们……

### [360s - 480s]

Our model projects like a 20% chance. For, there's like a record demand, and one analyst expects 45 million copies in the launch quarter. But the first week—that's pretty harsh, right? Analysts expect 34 million in the whole launch quarter. 30 million in a week is a stretch. So yeah, it's gonna do well, but how well? And it's not PC now, so it's only like console, right? That kind of puts a damper on the sales too. But we will see. Can Jason or Lucia date someone else? Here, our model only gives that like 16% chance. So for, we have Rockstar has done this before in San Andreas and GTA 4. Mary Linton in Red Dead 2. So against zero out of 24 previous mentions, anyone besides the couple—they have to be a couple, says Rockstar's Rob Nelson. We also have another for: previous may hide side stories and ex an affair or someone new after a breakup. Yeah, might happen, but our model is not very positive since this game is really focused around this couple, probably. Will a third playable protagonist appear? Here, our model gives this like a two out of ten chance, 21%. Okay, so you can see for this, both Red Dead games handed control to a new hero at the end of the story. Also, the bar is low. One short playable story segment counts. So in like a mission, maybe we switch to another character. That could happen, right? But let's just wait and see. And against this, so everything we've seen so far from the game, including the final mission, switches only between Jason and Lucia. Yeah, 20% chance. Question six: Will NPCs film you and post it on social media? So we see a lot of this social media thing, I think, from the games. So for these pieces like six, working social feeds, TikTok-style app, and bystanders filming on their phones. So again.

【译】……模型估计大概 20% 的概率。支持面：需求是创纪录的，有一名分析师预计首发季度 4500 万份。但「首周」——那可就苛刻了，对吧？分析师们预计整个首发季度 3400 万份，一周就卖 3000 万份有点狠。所以它肯定会卖得很好，但到底有多好？而且现在没有 PC 版，只有主机，对吧？这也会给销量泼点冷水。我们走着瞧。Jason 或 Lucia 能和别人约会吗？这里我们的模型只给 16% 的概率。支持面：Rockstar 以前做过——《圣安地列斯》和 GTA 4，《荒野大镖客 2》里的 Mary Linton。反对面：此前 24 条物料里 0 条提到这对之外的感情——他们必须是一对，Rockstar 的 Rob Nelson 是这么说的。我们还有一条支持面（转写疑似）：前作会把支线故事藏起来——前任、外遇、或者分手之后的新欢。有可能会发生，但我们的模型不太乐观，因为这款游戏大概真的完全围绕这对情侣展开。会出现第三位可玩主角吗？这里我们的模型给两成机会，21%。支持面：两代《荒野大镖客》都在故事结尾把控制权交给了一位新主角；而且判定门槛很低——一小段可玩剧情就算数。比如某个任务里我们切换到另一个角色，这是有可能的，对吧？但咱们等着瞧。反对面：目前我们看过的一切，包括最终任务，都只在 Jason 和 Lucia 之间切换。20% 吧。第六题：NPC 会拍下你并发到社交媒体上吗？我们从游戏里已经看到很多这种社交媒体元素了（转写疑似）——大概六个能用的社交信息流、TikTok 风格的应用、路人举着手机拍你。所以……

### [480s - 600s]

Against no major preview mentions NPC posting your footage, and a feature this marketable would have been shown. So the model thinks like if this was like a feature, we would have seen this. But one scripted moment would be enough. So like a heist or like a mission or something could actually make this happen. But only like a 30% chance from our model. Can Jason and Lucia go fully naked in free roam? So this was kind of from the leaked nude clips that what was it called? I can't remember that NFT or like meme coin guy. He had some clips, but the NPCs don't react in those clips. So that is kind of against it. And Korea's rating board reportedly tried and couldn't fully undress Jason and Lucia in like some kind of test. The rating lists nudity, and millions of players will hunt for glitches after the launch. So that is kind of for, but our model gives it only 14%. So our last question is gonna be: Can you get arrested and play inside a jail cell? So against this, so our model gives this like one in ten. So no mainline GTA has a playable cell in five games. Get busted and you reappear outside the station. This is kind of the standard, right? Previous say the same. You respawn at the police station's door minus some money. That's kind of how it's always been. So the wildcard is the wanted level now goes to six stars, and nobody has shown what happens there. But yeah, one in ten. But it would be pretty cool, right? So yeah, I think we think we kind of had some interesting. We have two market ones, so I'm gonna definitely look out for this and to kind of check all the in-game ones. I probably have to check out Reddit, right, and stuff like that, and play it a bit myself, maybe, and see what I can do here. But it's gonna take some time before we know everything.

【译】反对面：没有一家主要预览提到 NPC 会把你的画面发上网，而一个这么好营销的特性如果存在，早就被展示了。模型的逻辑差不多是：如果真有这个功能，我们应该已经见过它。但一个脚本化桥段就足够了——比如某次劫案或某个任务里真可以来这么一出。模型只给大概 30% 的概率。Jason 和 Lucia 能在自由漫游里完全裸体吗？这个算是来自泄露的裸体片段——那家伙叫什么来着？我记不清了，那个搞 NFT 还是 meme coin 的人。他手里有一些片段，但那些片段里 NPC 没有反应，这算反对面。还有，据报道韩国分级机构试过、在某次测试里没法把 Jason 和 Lucia 完全脱光。分级清单列出了裸露内容，而且发售后几百万玩家会去找 glitch——这些算支持面。但我们的模型只给 14%。最后一题：你能被逮捕并且在牢房里玩吗？反对面：我们的模型给十分之一。五代主系列 GTA 都没有可玩牢房，被铺之后你在警局外面重新出现——这差不多是标准流程，对吧？前作都是同一个说法：你在警局门口重生、损失一点钱，历来如此。变数在于：通缉等级现在升到了六星，而没有人展示过六星会发生什么。十分之一。但那会挺酷的，对吧？所以我觉得我们这几道挺有意思。有两道市场题我肯定会盯着，游戏内的题我得去 Reddit 之类的地方核对，也许自己再玩一玩，看看能做成什么样。不过要把一切弄清楚还需要时间。

### [600s - 667s]

So yeah, pretty cool to check this out. Really looking forward to the game, and I kind of had to do a video on this because there's so much attention around it. And I kind of wanted to test the model to see how well it does on this very specific custom questions about one game. I don't have a lot of confidence in these predictions, but yeah, I'm definitely going to check them out. So like I said, I'm using my startup, Sift Intelligence, for this. This is kind of my AI model to make predictions on basically everything. So I also had started up my benchmark here, so now you can see what models are best so far at predicting. This is more real events, right? So Fable 5.2 just took the lead about Opus 5.5. So that is kind of the best model so far at making predictions, at least in like other niches. But I haven't done any gaming niches. But yeah, definitely looking forward to the game. Just a couple of days now, and I think a lot of people has been waiting forever for this. So yeah, I'm going to check out how this goes. Maybe I do some kind of update how we actually did. But yeah, have a great day. Enjoy the game in a couple of days, and I'll see you again very soon.

【译】所以，能看看这些真的挺酷。我非常期待这款游戏，关注度这么高，我也确实得为它做一期视频。我还想测试一下模型在「针对单个游戏的非常具体的自定义问题」上表现如何。我对这批预测没有多少信心，但我肯定会去核对它们。就像我说过的，我用的是我的创业公司 Sift Intelligence——这是我的 AI 模型，基本上可以对任何事情做预测。我还把基准测试搭起来了，现在你能看到目前为止哪些模型最擅长做预测——是真实事件。Fable 5.2 刚刚超过 Opus 5.5 领先（模型名依转写），那是目前预测做得最好的模型，至少在其他领域是这样；游戏领域我还没测过。但真的很期待这款游戏，就差几天了，我想很多人已经等了太久太久。所以我会看看结果怎么样，也许会做一期「我们实际表现如何」的更新。祝你们今天愉快，几天后享受游戏，我们很快再见。

