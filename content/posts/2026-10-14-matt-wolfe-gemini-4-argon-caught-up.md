---
title: "Google 追平了：Gemini-4 Argon 的百万输出 token、DeepSweep 登顶与「生态牌」逻辑"
date: 2026-10-14
draft: false
tags: ["灰色信源", "Matt Wolfe", "AI趋势", "模型发布", "评测", "Google"]
categories: ["podcast-summary"]
summary: "Matt Wolfe 实测 Google 新旗舰 Gemini-4 Argon（两周前官宣、本次正式开放，Pro 约 $20/月、Ultra 约 $100/月起，API 介绍价 $2/$10 每百万 token、12 月起翻倍至 $4/$20）。节目称其拿下 Val's Index、Automation Bench、法律与 DeepSweep 的 SOTA，Artificial Analysis 综合 53 分仅与 Fable 持平、低于 Opus 5.5/Sonnet 5.5/Muse Wonder；差异化卖点=100 万 token 输出上限（约 75 万词，全网独一份）+ 低单任务成本。实测：3D 可打印模型生成接近最佳、游戏克隆机制完美但美术逊于 Opus 5.5/Astra、Tasks 智能体模式卡壳一小时。UP 主判断：不算世代 leap，但 Google 靠 Gmail/Chrome/日历生态随时可以一夜之间拿回用户量第一。以下均为节目陈述与 UP 主观点。"
---

# Google 追平了：Gemini-4 Argon 的百万输出 token、DeepSweep 登顶与「生态牌」逻辑

先说结论：Matt Wolfe 这期（2026-10-13 发布，约 20 分钟）对 Gemini-4 Argon 的定性是「**Google 追平了，但没有超越**」——用他的原话，这是「GPT-6.1 So 大、Opus 5.5 大」级别的模型，在部分基准上拿下 SOTA，综合指数仍略逊头部。真正属于它自己的东西只有一个：**100 万 token 的输出上限**（约 75 万词，UP 主称没有其他模型做到）。而这期视频更值得记录的，是它顺带展示了当前模型评测的几条潜规则：介绍价与正式价的把戏、单任务成本与「智能-成本象限」的读法、以及 Tasks 智能体模式在预发布版本里的真实卡壳。以下均为节目陈述与 UP 主个人观点，数字未独立核实。

## 时间线与获取门槛

两周前 Google 发博客宣布 Gemini-4 Argon（「下一时代的边界智能」）但没给任何人用——只有少数「受信任的网络防御者」拿到。本期发布当天，付费用户可用：**Google AI Pro**（约 $20/月）可在 Gemini 应用与搜索 AI 模式使用；**AI Ultra**（最低档约 $100/月）才解锁其编码平台 **Antigravity** 里的 Argon。API 介绍价为 **$2/百万输入、$10/百万输出**（约为 Opus 的一半、与 Sonnet 5.5 及 GPT-6.1 So 相当），但 UP 主特别标注：**12 月起涨到 $4/$20**。命名上 Argon 延续了 Google 一贯的浮夸传统，同时进入了搜索、Antigravity，还配了个与 Muse/Dots 竞争的新产品。

## 基准面：单项 SOTA 与综合第二梯队的分裂

节目列出的 SOTA（均据 Google 自己的发布材料）：**Val's Index**（金融/编码/法律/税务等专业任务）、**Automation Bench**、法律工作，以及编码智能体基准 **DeepSweep**——UP 主称后者过去半年被广泛视为最贴近开发者真实体感的编码基准。而第三方综合的 **Artificial Analysis 智能指数**上，Argon 为 **53 分**：与 Fable 持平，低于 Opus 5.5、Sonnet 5.5 与本周同发的 Meta Muse Wonder。**单任务成本**则是它的强项：约 $2/任务，明显低于多数对手（对比：Opus/Sonnet/Fable 约 $7.63、Astra 约 $3.26、GPT-6.1 So Max 约 $0.72）。Google 同时承诺 Argon 回答更直接、更少陈词滥调、更可靠地遵循风格指令。

这种「单项 SOTA + 综合不顶尖」的分裂本身就是观察模型的教训：模型发布会会挑自己最好看的那张图。UP 主的读法是务实的——在「智能-成本象限」里 Argon 不在最左上的理想区，但按性价比是甜点位候选。

## 两个真正的差异点：百万输出与 Tasks 模式

**100 万 token 输出**值得单独讲：多数模型的百万级上下文指的是输入+输出共享的窗口，且大头在输入侧；Argon 能**输出**百万 token。UP 主认为这是它最有分量的进步——意味着整本书级的一次性生成物、超长代码文件、完整报表在同一响应里完成。他对「这是否改变生活」的判断很克制：除这一点外，Argon 不算对 Anthropic/OpenAI 的世代跨越。

产品侧的变化：Gemini 应用新增 **chat 与 Tasks 双模式**（原 Gemini Spark 智能体改名并入），Tasks 用于让模型代办事；连接 Instacart、OpenTable、Adobe、Shopify 等应用的生态与其他大厂看齐。Antigravity 的编码界面也将进入 macOS 的 Gemini 应用（类似 ChatGPT 应用里切 Codex/聊天）。但 UP 主的实测泼了冷水：让 Tasks「研究过去 7 天最重要的 5 个 AI 发布」跑了一个多小时无产出、「做一份给非技术小老板的 prompt injection 六页幻灯片」同样卡死——点开 Plan Steps 才发现幻灯片其实已生成，只是任务状态停摆。他归因于预发布版 bug，希望正式版修复。

## 实测部分：SVG 强、3D 打印惊艳、游戏美术一般

UP 主的标准测试链：**Gary Busey SVG**——可能是他测过最好的一批（因提前测试未进排行榜，LLM 裁判评分与 token 成本待补）；**Mega Bonk 游戏克隆**——一次通过、六个角色两张地图可选、自动射击/升级/跳跃/滑行俱全，机制「近乎完美」，但美术明显逊于 Opus 5.5（他心中的设计美学冠军）与 GPT-6 Astra；**物理沙盒**——大炮+地球/火星/月球重力预设+角度/速度/风力/目标距离滑杆实时联动，做得很扎实，火星模式弹道明显更长、还自带明暗主题；**排程推理**——两人任务的约束满足题，正确给出 14:00 最早完成时间与逐约束核对；**订阅对比研究**——ChatGPT Plus/Claude Pro/Google AI Pro 的 API 权益对照表做得清楚（Google AI Pro 每月送 $10 GenAI/Cloud 开发者额度是差异点）。

最惊艳的是 **3D 打印**：Google 团队预沟通时称 Argon 擅长生成 3D 可打印文件，UP 主扔给它一张 Gary Busey 照片要求生成可送 Bambu 打印机的 STL——它不仅生成并自检通过，还主动命名为「Gary Busey 半身像」（他故意没说是谁，模型认出来了）。他真的打印了出来，正面效果满意，宣布要在背景墙给它留永久位置。

**机器人彩蛋**：Google 用 Argon 教机器人玩 cornhole（沙包投洞）——机器人用左眼摄像头看人示范、自己练习，最终连续 7 次投进（虽然距离只有约两英尺）。UP 主把这视为「看视频+听指令学会物理任务」路线的又一证据。

## 机制链与读者启发

**链条一（介绍价的算术）**：$2/$10 介绍价 → 与 Sonnet/GPT-6.1 So 打平的迁移诱因 → 12 月翻倍后性价比叙事自动衰减 → 启发：任何「比竞品便宜一半」的对比都先查价格时效；重度 API 用户应在介绍期窗口内完成成本敏感型任务的迁移评估。

**链条二（生态 > 单模型）**：Argon 单项强、综合第二梯队 → 但 Gmail/日历/Chrome/搜索的分发覆盖是 Anthropic 与 OpenAI 没有的 → 一旦模型「够好」，默认渠道决定用户量 → Google 可以一夜之间回到「最多人使用的模型」位置。这也是 UP 主「永远不把 Google 除名」的原因。对从业者：评估 Google 的 AI 竞争力时，模型榜单是次要变量，产品入口渗透率才是主变量。

**链条三（评测的象限读法）**：综合指数（AA 53）与专项 SOTA（DeepSweep、法律、Automation）给出相反信号 → 榜单选择本身成为营销武器 → UP 主的实操结论：先确认自己的高频任务落在哪个专项（编码/法律/自动化/长输出），再选模型，而不是追综合分。100 万输出 token 对长文档生成场景是当前独占的硬差异，值得单独验证。

留给下期的悬念：Anthropic 与 OpenAI 几周或几个月内再甩出碾压级新模型的概率，UP 主认为「相当可能」——这也是理解本轮发布节奏的背景板：Argon 是「追平局」，不是「终结局」。
