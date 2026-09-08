---
title: "What Does Multi-Harness RL Learn? — 评测 Harness 是 Agent RL 的主导变量 精读"
date: 2026-09-08
draft: false
tags: ["论文精读", "Agent", "Harness", "Benchmark", "评测", "强化学习"]
categories: ["paper-reading"]
summary: "本论文在同一 Qwen3-8B 热启动上回放相同任务-harness 记录（Aider/OpenHands/Qwen Code/SWE-agent），对比 GRPO 的 Within/Cross 两种分组规则，用 24,000 次密封 SWE-bench Verified 评估发现：评测 harness 使解决率从 2.14% 摆到 9.27%（4.3 倍），训练配方仅移动 1.16，分组规则不显著（+0.25pp，CI 含 0）。harness 工程对 Agent RL 的影响碾压算法选择。"
---

> **论文链接**：[What Does Multi-Harness RL Learn? Credit Assignment and Portability in Coding Agents](https://arxiv.org/abs/2609.04518)
> **发表时间**：2026年9月（arXiv:2609.04518）
> **机构**：高校研究团队（仓库级 coding agent 与 RL 评测方向）
> **领域标签**：cs.SE / cs.LG / 智能体强化学习 / 评测方法学 / Harness 工程

## 一、论文背景

Agent RL 正在成为 coding 智能体训练的主流路线：让模型在**完整执行 harness**（工具、提示、环境、解析器）里跑任务，用解决与否做奖励。社区最近流行一个"多 harness 配方"：让策略同时 exposure 到多个 harness（Aider、OpenHands、Qwen Code、SWE-agent），期望学到跨 harness 的可迁移能力。

但这个配方其实捆绑了**两个完全不同的选择**：

1. **暴露（exposure）**：让策略见过多个 harness 的观察/动作空间；
2. **分组（grouping）**：GRPO 的相对优势在一个组内计算——多 harness 时，是每个 task-harness 对一个组（**Within**），还是同任务跨 harness 合并成组（**Cross**）？

更根本的问题是：**当你在多 harness RL 后报告提升时，提升到底来自训练配方，还是仅仅来自你恰好用哪个 harness 做评测？**

这个问题为什么致命：harness 之间的差异远比想象的大——工具集、提示模板、输出解析、环境反馈格式全都不同。同一模型在 Aider 里跑和 OpenHands 里跑，可能是两种完全不同的"考试"。

概念铺垫：

- **GRPO（Group Relative Policy Optimization）**：组内 rollout 的奖励做归一化得到相对优势，无需价值网络；
- **Within/Cross 分组**：组内相对性是 RL 的"参照系"——参照系选错，优势估计就系统性偏；
- **密封评估（sealed evaluation）**：评测代码对训练方不可见、不可调，杜绝评测侧的隐性过拟合。

## 二、论文定位和关联工作

| 谱系 | 代表 | 关注点 | 与本文的区别 |
|------|------|--------|-------------|
| 多环境 RL | 跨域泛化研究 | 环境多样性带来的鲁棒性 | 未分离"暴露"与"分组"，未量化 harness 主效应 |
| SWE-bench harness 研究 | SWE-bench 官方 harness 分析 | 评测标准化 | 静态分析；本文在 RL 训练语境下量化 harness 主效应 |
| GRPO 分组研究 | 组构造对优势估计的影响 | 算法层面 | 本文在真实 harness 维度上检验 |
| 评测信用危机系列 | 本专栏此前 SWE-Gate、PatchBench 等精读 | 评测虚高 | 本文聚焦 RL 训练报告的 harness 依赖性 |

**定位结论**：这是一篇**方法学监护论文**——它不提出新方法，而是用受控实验揭示：Agent RL 文献里被归功于训练配方的提升，可能大部分是评测 harness 的选择效应。

## 三、问题定义

具体问题：多 harness RL 中，暴露、分组、评测 harness 三个因素各自贡献多少解决率变化？

抽象化：**归因问题**——把最终评测解决率 R 分解为：

R = f(评测 harness) + g(训练配方：暴露×分组) + ε

- **给定**：单一 Qwen3-8B 监督热启动检查点（冻结起点）、4 个训练 harness（Aider/OpenHands/Qwen Code/SWE-agent）的任务-harness 记录、密封的 SWE-bench Verified oracle；
- **控制**：相同更新步数、相同任务集、相同回放记录（把"暴露"变量做成完全相同的输入）；
- **操纵**：分组规则（Within vs Cross）与评测 harness（4 个训练 harness + 1 个 held-out 最小 harness）；
- **求**：各因素的效应量。

精妙之处：用"回放相同记录"把暴露变量锁死，任何差异只能来自分组规则或评测 harness——一个完美的受控归因设计。

## 四、问题解法

实验设计（这是论文的全部"方法"，也是其精髓）：

1. **单一热启动**：所有变体从同一个 Qwen3-8B SFT 检查点出发，排除起点差异；
2. **记录回放**：使用完全相同的冻结 task-harness 记录（不是重新采样），保证训练信号输入一致；
3. **两分组规则**：Within（每 task-harness 一组，参照系=同 harness 的尝试）vs Cross（同任务跨 harness 合并，参照系=跨 harness 尝试池）；
4. **密封 oracle**：所有检查点（含训练中所有中间点）用同一套对训练方密封的 SWE-bench Verified 评估，在 4 个源 harness + 1 个 held-out 最小 harness（训练时从未见过）上执行；
5. **规模**：24,000 次密封评估，95% 置信区间报告。

## 五、评估指标与实验证据

| 变量 | 效应量 | 统计细节 |
|------|--------|---------|
| **评测 harness** | 解决率 2.14% → **9.27%**（**4.3×**） | 跨 24,000 次密封评估的主效应 |
| 训练配方（含分组规则） | **1.16** 个百分点 | 与 harness 主效应差一个数量级 |
| Cross − Within（held-out harness） | **+0.25 pp** | 95% CI [−0.? , +?]，**含 0，不显著** |
| Held-out 最小 harness 可迁移性 | 训练 harness 增益不能自由迁移 | 跨 harness portability 有限 |

**实验设计如何证明论点**：

1. 效应量对比是论文的心脏：harness 移动解决率的幅度（4.3 倍）是训练配方（1.16pp）的**数量级以上**——这意味着读任何 Agent RL 论文时，"在哪个 harness 上评"比"用了什么算法"更值得先看；
2. held-out harness 上的不显著性直接回答标题问题：多 harness 分组规则**没有**产生可迁移的信用分配改进；
3. 密封评估+全检查点扫描排除了 cherry-picking 的解释空间。

## 六、效果优势的根源解释（机制：为什么 harness 效应如此巨大）

**harness 为什么是主导变量**：harness 不只是"跑代码的外壳"，它定义了策略的整个感知与动作接口——

- **观察空间**：工具返回格式、错误信息详略、文件树呈现方式；
- **动作空间**：可用工具集、命令语法、补丁格式；
- **参照系**：GRPO 组内归一化的基线分布。

机制因果链：harness 决定观察/动作分布 → 同一权重在不同接口下的行为能力完全不同（如同同一棋手换一套棋规）→ 密封评测显示的 4.3 倍摆动是**接口依赖性**的直接读数；而 Within/Cross 只是改变优势估计的参照系，不改变接口——所以其效应（0.25pp）被接口效应淹没。

**为什么 held-out 迁移有限**：RL 学到的相当一部分是 harness 特定的行为模式（如何解析该 harness 的反馈、何时调用其特有工具），这类知识绑定在接口上，不随权重迁移。

**对本领域报告实践的推论**：一篇 Agent RL 论文若只报告单一 harness 的提升，其效果量的解释力被 harness 选择效应主导——除非报多 harness 密封评估。

## 七、必要知识反推

**领域知识层**：
- 各主流 coding harness（Aider/OpenHands/Qwen Code/SWE-agent）的接口差异细节；
- SWE-bench Verified 的评估协议与 harness 实现。

**方法论知识层**：
- GRPO 组构造与相对优势估计的统计性质；
- 受控实验的因子分离设计（锁死输入、操纵规则）；
- 大规模密封评估的置信区间纪律。

**工程知识层**：
- 多 harness 统一回放基础设施（记录-重放架构）；
- 检查点全量扫描的算力组织（24,000 次评估的调度）。

**融合的关键节点**：作者意识到 GRPO 的"组"本质上是一个**参照系选择问题**，而 harness 是另一个**接口选择问题**——两者在"多 harness RL"的流行叙事里被混为一谈；把这两个变量在一个受控设计里拆开，是全文的枢纽洞察。

## 八、论文中可以提取的通用性灵感

1. **接口效应 > 算法效应是智能体领域的默认假设**
   - 核心思想：在比较任何智能体训练方法前，先假设 harness/接口的选择贡献了主要方差。
   - 论文证据：4.3× vs 1.16pp 的效应量对比。
   - 推广场景：Agent 论文审稿标准（本读者有顶会审稿经验，可直接用作审稿 checklist 项）、企业 agent 选型评估、self-evolving 系统的归因分析。

2. **"暴露"与"参照系"是捆绑变量的常规嫌疑**
   - 核心思想：流行配方常常捆绑多个机制不同的选择，归因研究必须拆开。
   - 论文证据：Within/Cross 不显著而 harness 显著。
   - 推广场景：训练配方消融设计、A/B 测试的变量分离、技能组合策略评估。

3. **密封评估是训练-评测隔离的工程标准**
   - 核心思想：评测 oracle 对训练方密封+全检查点扫描，是可信报告的底线配置。
   - 论文证据：24,000 次密封评估的可信度来源。
   - 推广场景：基准组织的评估协议、竞赛平台、内部模型发布门禁。

4. **held-out harness 是 portability 的试金石**
   - 核心思想：能力是否"学会了"而非"记住了接口"，只能在新接口上检验。
   - 论文证据：held-out 最小 harness 上分组规则不显著。
   - 推广场景：评审系统的跨场景泛化测试（本文读者 V5 系统的多场景验证可直接借鉴）、机器人 sim-to-real、跨语言迁移。
