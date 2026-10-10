---
title: "按任务计价的智能，与全面撞车的 Agent：Matt Wolfe 本周 AI 圈速览"
date: 2026-10-10
draft: false
tags: ["灰色信源", "Matt Wolfe", "AI趋势", "Agent", "开源模型", "评测"]
categories: ["podcast-summary"]
summary: "Matt Wolfe（视频 ID NUizyuGj-kE，约 35 分钟，周四录制）梳理一周动态：Anthropic 发布 Haiku 5.5——榜单全面落后 Sonnet/Opus，但每任务成本约 21 美分、约为 Sonnet 四分之一价，'智能'正变成可按任务计价的商品；OpenAI 开启 28 天连发，GPT-6 向全员开放并配'intelligent UI'（训练模型组合文本/图形/交互件作答），另推 6 倍价换 8 倍速的 Sol Ultrafast 与三类输出的 Decisions API；Agent 赛道洪水滔天——Grokbot 宣布按任务路由最优后端（含 Opus、Midjourney、Suno）、Figure 创始人推 Hark Pro、Google 出企业版 Gemini Agent，会议记录场景出现 Granola/OpenAI/Google 离线版三重奏；开源权重侧 Mistral Large 4（1T 参数）与 Reflection Beam（501B）代表非中国阵营追平中国开源梯队；另覆盖 Anthropic'禁止持续虐待模型'条款与 Acemoglu'10 年内仅 5% 人类工作被替代'的反主流判断。"
---

# 按任务计价的智能，与全面撞车的 Agent：Matt Wolfe 本周 AI 圈速览

> 信源说明：本文基于 Matt Wolfe 频道 2026-10-08 录制、周五发布的周更 AI 新闻视频（视频 ID NUizyuGj-kE，转写全长约 35 分钟，18 个时间段）。全部内容为 Matt Wolfe 单口陈述及其屏幕展示的官方页面/榜单，本文按"主持人称/节目展示/节目引用"归属，未做独立核实；节目中的基准成绩一律是厂商或第三方榜单转述。

## 先说结论

本周 AI 圈的两条主线，恰好是一组对偶：**模型侧，"智能"被进一步拆成按任务计价的商品**——Anthropic 用 Haiku 5.5 把"够用智力"的价格压到每任务约 21 美分，OpenAI 则反向用 6 倍价格卖 8 倍速度；**产品侧，Agent 全面撞车**——Grokbot、Hark Pro、Gemini Agent、Muse、Dots 各家能力趋同，主持人自己的判断是"最后你只是挑一家你最忠诚的公司"。夹在中间的信号是：界面本身开始被训练进模型（GPT-6 的 intelligent UI），而会议记录这个小场景一周内被 OpenAI 与 Google 同时"功能化"。对读者的启发在于选型逻辑要换轴：从"哪个模型最聪明"换成"每任务成本+速度档位+生态锁定"。

## Haiku 5.5：不追榜单，追"每任务 21 美分"

Anthropic 本周发布 Claude Haiku 5.5。主持人称其定位是"高吞吐、成本敏感任务"：摘要、压缩、数据库查询、分类，以及官方建议用作编码场景的子智能体（sub agent）。节目展示的基准成绩单边落后：知识工作 1620 对 Sonnet 的 1840（另一项 1578 对 1824），计算机使用 72 对 Sonnet 的 83.9，"几乎没有任何一项能跑赢自家 Sonnet 或 Opus"。

但价格才是卖点。节目称 Haiku 5.5 在 extra high 档的智力约等于 Sonnet 5.5 的 medium 档，而成本 28 美分对 93 美分；输入约 0.10 美分/百万 token（前 10 万）再 0.50 美分，输出 0.50 美分起、超出后 2.50 美分，整体约为 Sonnet 四分之一价、也比上一代 Haiku 便宜不少。第三方榜单 Artificial Analysis 的交叉印证更有意思：Anthropic 占据智能榜前三（Sonnet/Opus/Fable），Haiku 5.5 排到 Kimi K3、GLM 5.3、Grok 4.7 之后；但它每任务平均成本约 21 美分，远低于自家高价梯队——尽管它每任务耗 token 数全榜第二（仅次于 Sonnet 5.5），便宜的单价把量大的劣势完全吃掉了。主持人的结论：订阅用户（20 美元/月起）大概率继续用大模型，真正会用 Haiku 的是 API 调用方——自建工具、Claude Code 里的轻量步骤。

同一条"决策商品化"曲线上还有 OpenAI 的 Decisions API（节目称其在 Dev Day 已宣布、本周上线）：不走文本输出，而是三类结构化结果——谓词（判断陈述为真的概率）、选择（从预置选项中选并给置信度）、评分（按数值范围评估），底层用最便宜最快的 GPT-6 Luna，"类似 Jev（转写如此，疑为同类决策接口产品）的思路"。主持人的用法判断：开发者拿它做海量低成本判断，而非对话。

## OpenAI 的 28 天连发：GPT-6 intelligent UI 与"速度档位"定价

OpenAI 产品负责人 Thibault（转写为 Tebow/Tibo）10 月 4 日在 X 宣布：未来 28 天每天上线一项对多数 Codex/Work 用户"明确有用"的改进或一次完整重置。主持人 10 月 8 日录制时已见证四天超额发货，其中分量最重的是 **GPT-6 全员开放 + intelligent UI**：官方表述是"我们训练 GPT-6 用文本、视觉与交互元素组织回答"——节目演示了"拆解七速自行车设计"得到可交互的爆炸图、"内燃机原理"配逐步图解，对比 GPT 5.6 instant 的纯文本菜谱，GPT 6 instant 直接给图文清单甚至内嵌小计算器。速度上，节目展示的曲线称原来需 215 秒的智力水平现在 109 秒可超过。这值得注意的点不是"更聪明"，而是**模型的输出单位从文本变成了界面**——与 Anthropic 同周推出的 Claude 实时仪表盘（连接 BigQuery/Databricks/Snowflake/Salesforce）与动画解说工具 Claude Motion 属同一方向：答案越来越以可操作的面板/图形/交互件交付。

后续几天的更新偏开发者：默认速度在 GPT-6 Astra 与 GPT-6.1 Sol 上提升 50%（10 月 5 日）；auto review 对所有登录用户免费，"approve for me"权限模式大幅减少打扰（10 月 6 日）；API 定价档从五档并为 Build/Launch/Grow 三档（档位越高限流越松）；以及 **GPT-6.1 Sol 的 Ultra fast 模式**——标准价 2 美元/百万输入、10 美元/百万输出，fast 翻倍，ultra fast 6 倍价换同智力 8 倍速，需 500 美元/月 Pro 档。主持人坦言想为视频剪辑试一把（配合 computer use 让它"在屏幕上飞快地剪"），但还没咬牙升档。第四天的更新是把"steer"（中途改指令）做成即时，与 Ultra fast 组合时尤其有用——代价结构很直白：**速度成了单独出售的维度**。

Anthropic 侧一个对应的小别扭：Claude Motion 动画功能只给 Team/Enterprise 试用，主持人作为付 200 美元/月 Max 档（卖点就是"高级功能抢先体验"）的用户反而拿不到，"有点烦"。他的公允判断是：仪表盘和动画用 JS 库（hyperframes、remotion）本来就能做，这次只是内置化、少了插件环节——新意在便利而非能力边界。

## Agent 洪水：能力趋同，生态与记忆成为胜负手

本周至少三个新/更新 Agent 入场，且主持人明确给出了"趋同"判断。

**Grokbot** 宣布两件事：一是"按任务选用最优后端模型"，不再默认 Grok 自家，而是路由到 Opus 5.5、Midjourney、Suno 等外部最优 API——主持人特意留了个悬念：鉴于马斯克与 Altman 的宿怨，若 OpenAI 模型重新登顶，Grok 会不会也接？"谁知道，大概不会。"二是 Grokbot 可直接搜索、读取与监控 X：他的 X bot 分析了自己最近 75 条帖子，找出最佳/最差 AI 帖、总结出太平洋时间上午 10 点到下午 1 点发帖效果最好、还基于当下热点（"Gemini 4 Argon 上手""GPT-6 免费开放""OpenAI 营收抛售""新加坡人形机器人格斗夜"等 X 上的传言话题）给出选题建议——他顺手核实了"Gemini 4 Argon 当天发布"一条，结论是 10 月 7 日的社区猜测、当时仍只对早期测试者开放。Grokbot 的组织方式也与 Muse/Dots 的"单一聊天"不同：多个专职 bot（邮件分诊、研究、X 监控）之上可设"chief of staff"总调度，由它代为派活。

**Hark Pro** 来自 Figure Robotics 创始人 Brett Adcock（节目称其"也是 Figure 的创建者，现在进入 Agent 赛道"），演示视频一镜到底：Target 补货下单、生成含消息量/交接统计的演示文稿、把收据提交进 Ramp、在 LinkedIn 找候选人并发好友邀请、叫 Uber、打印 Amazon 退货标签、DoorDash 订 Pad Thai、检索投资人名单群发订阅邀请、乃至"退订 ChatGPT"。主持人点评："又一个 computer use 代理"——与 Muse、Dots、Grokbot、Instinct、Google 的 Agent 干的是同一批事，"等它们全部拉平，你最终只是选一家你最忠诚的公司"。

**Google** 的 Gemini Agent 则走企业路线：跨设备（web/iOS/Android/Windows/Mac）、接 Google Workspace、Microsoft 365 与 Slack、云端多智能体编排。主持人把它放进 rapid fire 而非头条，理由正是"面向企业而非消费者"；他预计等 Gemini 4 Argon 正式落地后，Google 会把 I/O 上提过但沉寂已久的 Spark 一起推出来。

最能说明"趋同后拼什么"的是**会议记录三重奏**：OpenAI 的 meetings 插件（节目评价"基本就是内置版 Granola"——监听麦克风与系统音频、转写并存入 ChatGPT 记忆，可事后追问）与 Google 的离线笔记应用 Google AI Edge Foresight（macOS、全程本地转写，"数据不出电脑"）同周出现，加上原玩家 Granola，一个单点场景一周内被两家巨头功能化。主持人为 Granola 说了句公道话：它是专精工具，但巨头的分发优势就在那里。这里可以推断（非节目原话）：会议记录是高粘性数据资产，谁把它喂进自己的记忆系统，谁就锁住了用户切换成本——Google 选择用"隐私/本地"做差异化，OpenAI 选择用"记忆网络效应"。

其余小更新：Meta 开源了自制 Muse AI 小硬件（keynote 上那个"电子宠物机"）的代码，可拿树莓派复刻；Google 上线游戏生成器 Playground（文字描述即出桌面/移动游戏，美国 18+，可玩他人作品）；Google 还推出 Synth ID Detector，可上传文件检测是否 AI 生成，但节目称仅覆盖 NVIDIA、OpenAI、Google 与"Cacao（转写疑似，原词未确认）"家的生成内容。

## 开源权重：非中国阵营追平，买家从极客变成机房

两个新开源权重模型都拿到了"对标中国梯队"的定位。Mistral Large 4（绰号 Le Chonk）为 1 万亿参数开源权重，官方基准直接对标 Kimi K3、GLM 5.3、DeepSeek V4；主持人的自有基准 Bussy Bench 上它 2.5 分钟出结果、耗 9,675 token，AI 评分中游。Reflection AI 的 Beam 为 5010 亿参数（501B），对标名单加上 NemoTron 3 Ultra，agentic 编码与推理"略逊但基本同档"，目前仅早期测试者可用。

主持人的综合判断值得记录：这些模型大到个人电脑根本跑不动，真正的目标客户是**想自建服务器、全本地部署、保数据主权的企业**——开源权重的价值主张正从"极客本地跑"迁移到"企业机房"。

## 边角但重要：善待模型条款，与 5% 的反主流判断

两条收尾新闻虽小、信息量不低。其一，Anthropic 更新使用政策，新增禁止"对我们的模型实施持续且无必要的辱虐或残忍行为"（节目引用 Andrew Karan 在 X 上的发现）；官方澄清仅限无 discernible purpose 的极端重复辱虐，正常的挫败表达、反驳、黑暗创作主题与测试研究不在此列——但理论上，持续无端辱骂 Claude 可能被封号。其二，微软 AI 的 Mustafa Suleyman 转述诺贝尔经济学奖得主 Daron Acemoglu（主持人自嘲念不准名字）在 Humanist Review 的观点：**十年内只有约 5% 的人类工作会被 AI 取代**，与"AI 取代一切"的叙事正面相反。主持人明确不背书："信不信，留给下一期视频。"

## 这一周意味着什么

把账本合起来看：模型层在把智能切成"智力档位 × 速度档位 × 单任务成本"的三维价目表（Haiku 压下限、Sol Ultrafast 抬上限、Decisions API 干脆按决策计价），产品层在把彼此的能力互相抄齐（computer use、会议记录、多 bot 编排人手一份）。主持人自己已经给出了观察指标：接下来看 OpenAI 28 天连发的后 24 天还剩什么、Gemini 4 Argon 落地时 Google 的 Agent 与 Spark 是否合流、以及各家 Agent 拉平之后用户到底按什么选择——他个人的答案是"还在用 Codex 为主，最近加了一点 Grokbot"，而 Muse 目前唯一的显性差异是免费。对做选型的读者，本周的可行结论有三条：按每任务成本而非榜单分选模型；评估模型时把"它生成的界面质量"纳入维度；选 Agent 平台时先想清楚你愿意把会议记录、邮件、社交数据这些记忆资产交给谁锁定。

> ASR 与归属备注：Thibault（转写作 Tebow/Tibo）、Bussy Bench（节目自有基准名，转写如此）、Jev、Cacao 均按转写保留并标注疑点；Fable、Luna、Astra、Sol、Kimi K3、GLM 5.3、Grok 4.7、Dots、Muse、Instinct 等为节目原词的模型/产品名。所有基准数字均为节目屏幕转述，未独立核实。
