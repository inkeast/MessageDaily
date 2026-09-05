---
title: "HarnessEvo 精读：Harness 自进化的价值藏在控制槽位里"
date: 2026-09-05
draft: false
tags: ["学术调研", "Agent", "Harness", "自进化", "信用分配"]
categories: ["paper-reading"]
summary: "HarnessEvo 把 Agent harness 分解为 role/strategy/format/control 四个可独立进化的槽位，用 leave-one-in/out 协议做价值归因：整体指标'看似无效'（0.657 vs 0.642），但收益完全 localized 于 reflection/control 槽位（+0.119, p=0.0046）；等预算下多槽位同进反而互相稀释——预算分摊陷阱。本文基于全文阅读拆解其归因协议与对自进化领域的方法论警示。"
---

# HarnessEvo 精读：Harness 自进化的价值藏在控制槽位里

> **论文**：[Where Does Harness-Optimization Value Live? Localized Gains and the Budget-Splitting Trap in Self-Evolving LLM Agents](https://arxiv.org/abs/2609.02889)（arXiv:2609.02889，cs.CL）
> **作者**：Michael Nguyen, Wei Chen Tan, Nurul Aisyah Hassan, Arvind Raman, Li Hua Lim, Ahmad Faiz Razak, Jia Hui Wong
> **机构**：Universiti Malaya、Universiti Sains Malaysia、Universiti Putra Malaysia、Monash University Malaysia

---

## 一、题目

- **类型**：方法论分析论文——作者明确说 HarnessEvo 是"显微镜（microscope）"而非刷榜方法。
- **发表单位**：马来西亚四校联盟，独立学术研究；冻结槽位文本全部公开可审计。
- **一句话概括**：把 harness 当扁平字符串整体进化是主流做法，但价值归因从未被做过——本文分解出四个槽位逐一进化并做 leave-one-in/out 归因，发现收益全部来自控制槽位，且"整体进化"会因预算分摊把显著收益稀释成不显著。

## 二、背景

### 研究脉络

1. **Harness 自进化成为热点**：冻结模型、进化文本脚手架（persona、策略、格式规则、反思启发式）以提升 Agent 能力——GEPA、反思式 prompt 进化等工作的共同做法是把 harness 当**单一扁平字符串**整体优化。
2. **被跳过的归因问题**：整体进化后"总分涨了"，但涨在哪个部件？是角色设定、任务策略、工具格式规则还是反思控制？没人知道——这意味着后续研究无法定向改进，只能整体重跑。
3. **Goodhart 风险**：自进化的收益也可能来自 reward hacking（钻 benchmark 空子）而非真实能力——需要"收益是否 grounded"的证据。

### 本文的问题

> Harness 优化的价值到底 live 在哪里？如果把它分解成部件逐一归因，会看到什么整体指标上看不到的东西？

## 三、定位

| 维度 | 定位 |
|------|------|
| 问题类型 | 价值归因 + 方法论警示（非方法贡献） |
| 对象 | 冻结 LLM 的文本 harness（4 个命名槽位） |
| 协议 | 等预算坐标上升进化 + leave-one-in/leave-one-out 归因 |
| 评测 | ALFWorld（主）+ WebShop（null 验证），冻结 7B backbone |
| 目标会议口径 | ACL/EMNLP 分析 track、ICLR；对 Agent 工程社区是必读的方法论校准 |

## 四、问题定义

**形式化**：冻结模型 Agent 的 harness 分解为四个槽位：

- $c_1$ **role**：角色/persona 文本；
- $c_2$ **task-strategy**：任务策略启发式；
- $c_3$ **tool/format-rules**：工具调用与输出格式规则；
- $c_4$ **reflection/control**：反思与控制流规则（何时重试、何时停止、何时换路）。

**归因目标**：对每个槽位 $c_i$，在等预算下单独进化它，度量 held-out 成功率的增量增益（loi），再从完整 harness 中移除它度量损失（loo）——两方向交叉验证增益归属。

**预算分摊陷阱（budget-splitting trap）**：总预算 B 固定时，同时进化 4 个槽位意味着每槽位只有 B/4——显著的单一槽位收益可能被其他槽位的噪声与互相干扰稀释到统计不显著。

## 五、解法（实验协议）

### 5.1 架构：单注入点

四个槽位文本在运行时拼装注入到唯一位置，保证"改动哪个槽位"是清晰的实验变量（无交互注入点的混杂）。

### 5.2 等预算坐标上升

同一反思式优化器，按轮流调度逐槽位进化；预算严格匹配 flat-string baseline 的总 token 预算（B60/B120/B240 多档对照）。

### 5.3 统计纪律

McNemar 检验、分歧任务逐个翻转分析、WebShop 上验证 null 的偶发性（排除任务饥饿效应）、全部冻结槽位文本公开可复算。

## 六、实验结果

### 6.1 整体指标"看似无效"

ALFWorld held-out（n=134，冻结 7B backbone）：

| 方法 | 成功率 | McNemar p |
|------|--------|-----------|
| Stock harness | 0.642 | — |
| Flat-string 进化 | 0.642 | vs stock n.s. |
| HarnessEvo（全四槽） | 0.657 | 0.617 / 0.480，均 n.s. |

只看这张表，结论会是"结构化进化没有用"。

### 6.2 显微镜下的真实图景

- **仅进化 $c_4$（reflection/control）**：held-out 成功率 **+0.119（p=0.0046）**——28 个与 stock 分歧的任务中 22 个偏向进化后的控制规则；
- **$c_1/c_2/c_3$ 单独进化**：增益与噪声不可区分；
- **预算集中可恢复增益**：把总预算集中投给 $c_4$，完整拿到 +0.119；均分给四槽位则收益被稀释至不显著——**陷阱是优化器的性质，不是 harness 的性质**。

### 6.3 收益是 grounded 的

对进化出的控制规则做行为审计：增益来自真实的自我纠错行为（更早放弃死路、更准的重试时机），而非钻 ALFWorld 评测格式的空子；WebShop 的 null 也被证明是真实任务偶发性而非优化饥饿。

## 七、知识反推

1. **"整体指标不显著"≠"没有收益"**：收益可能被稀释或掩蔽——归因分析（分解+holdout）是把"无效"与"无效但被掩盖"区分开的唯一手段。这个教训适用于一切"组合优化"类研究（prompt 组合、工具组合、团队组合）。
2. **控制流规则是 harness 中最高杠杆的部件**：四个槽位中唯一显著的是"何时重试/停止/换路"——决策元层的规则比知识层（persona、策略描述）更值钱。这与人类工程经验（流程改进 > 知识堆叠）一致。
3. **预算分配本身就是实验变量**：多目标/多部件进化研究必须报告预算分配敏感性，否则结论不可比。B/4 与 B 的单部件进化根本不是同一实验。
4. **负结果论文的高产性**：本文没有提出新方法，却给出了领域最需要的校准信息——归因协议本身即可复用的贡献。

## 八、通用灵感

- **对 Harness/skill 自进化研究**（含 HarnessEvolve、DiagEvo 等近期工作）：下一步应报告"增益在哪个部件"，把 loi/loo 协议纳入标准评测流程；对 skill 库的设计启示是优先打磨"控制/元策略"类技能而非堆知识类技能。
- **对多场景自迭代评审系统**：评审 harness 的迭代优化应先把组件分解（评分规则/上下文获取策略/输出过滤/反思控制），定位高杠杆部件再集中预算——本文证明盲目全组件优化会浪费预算。
- **对实验设计**：任何组件化系统的优化研究都应内置"分解归因"环节；报告表应同时包含整体指标与组件级增益。
- **对读者**：看自进化论文时先问三个问题——预算怎么分的？收益归因了吗？有没有平凡对照（随机进化/固定模板）？
