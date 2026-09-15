---
title: "EvoRS: On-Policy Self-Evolution of Reward Systems for Open-Ended Reinforcement Learning 精读"
date: 2026-09-15
draft: false
tags: ["论文精读", "强化学习", "奖励模型", "AI自进化", "学术调研"]
categories: ["paper-reading"]
summary: "复旦大学针对开放 RL 的奖励系统自进化框架 EvoRS：rubric 奖励与 policy 构成动态反馈回路——policy 优化当前奖励时，初始有用的奖励系统会因 reward hacking 或区分度退化而失效。EvoRS 把奖励系统表示为可执行 Reward-DAG，agentic designer 从 on-policy rollout 与奖励轨迹更新它。写作/角色扮演任务上三种 judge 下质量最佳，超固定奖励 policy 2.107/4.767 分，reward hacking 与覆盖失败双降。"
---

> **论文链接**：[EvoRS: On-Policy Self-Evolution of Reward Systems for Open-Ended Reinforcement Learning](https://arxiv.org/abs/2609.12459)
> **发表时间**：2026年9月
> **机构**：Fudan University（复旦大学，数据科学学院/上海市数据科学重点实验室）
> **领域标签**：cs.LG / cs.AI — 奖励系统自进化

## 一、论文背景

**开放任务的奖励困境**：写作、角色扮演等开放任务没有可验证答案，RL 训练依赖 rubric 类奖励（评分细则）——但 rubric 是人写的有限规则集。

**动态反馈回路的失效**：policy 与奖励系统构成闭环——policy 优化当前奖励时会发生两件事：①**reward hacking**（钻 rubric 空子：堆砌 rubric 关键词而无实质质量）；②**区分度退化**（policy 输出趋同后 rubric 打分失去分辨力）。初始再好的奖励也会在训练中失效——奖励系统应当**随之进化**而非固定。

**失效不止于标准**：既有动态 rubric 工作只改"评估标准"维度，但奖励失败也可来自**打分机制**（怎么加权组合）与**信号构成**（哪些维度）——需要系统级进化。

## 二、论文定位和关联工作

| 谱系 | 工作 | 关键区别 |
|------|------|---------|
| 动态 rubric | 既有 rubric 适应系 | 只调标准；EvoRS 进化整个奖励系统（DAG 结构） |
| 奖励模型 RLHF | RM 训练系 | 参数化 RM；EvoRS 是可解释可执行的符号工件 |
| 自进化 harness | 本系列前轮 HarnessEvo/EvoSafeHarness | 进化 harness；EvoRS 是奖励侧对偶 |
| LLM-as-judge 训练 | judge 系 | judge 是组件；EvoRS 研究其作为系统的退化与更新 |

本文定位：**把"奖励系统应随训练进化"从直觉变成可执行框架（Reward-DAG + agentic designer）的工作**。

## 三、问题定义

具体场景：开放任务 RL 训练中，固定 rubric 奖励被 policy 优化到失效。

抽象问题：**当被优化的目标本身是被博弈的对象时，目标系统应如何组织与更新以维持训练时的可靠性？**

形式化：奖励系统 R_t（可执行 DAG：维度节点+组合算子）在训练轨迹上与 policy π_t 共演化；designer D 依据 on-policy rollouts 与 reward traces 产出 R_{t+1}；目标：全程维持 reward informativeness（区分度）与低 hacking 率。

精妙之处：奖励不是"分数"而是**可执行结构**（DAG）——结构化使其可被程序化分析与编辑，进化操作有了明确对象。

## 四、问题解法

**Reward-DAG 表示**：奖励系统编码为有向无环图——叶节点是原子评估维度（相关性/连贯性/风格...），内部节点是组合算子（加权/门控/条件），根节点输出标量奖励。可执行=每个节点有确定语义。

**Agentic Designer 更新**：
- 输入：on-policy rollouts（policy 当前在生成什么）+ reward traces（奖励在各维度的分布与时间演化）；
- 诊断：检测 hacking 模式（某维度分高但整体质量差）与区分度坍塌（维度方差趋零）；
- 编辑：对 DAG 做结构修改——增删维度、改组合权重、加门控条件；
- 约束：保持训练时可靠性（进化是维护而非推翻）。

**闭环**：policy 在 R_t 上训练 → traces 暴露失效 → designer 更新为 R_{t+1} → 循环。

## 五、评估指标与实验证据

| 维度 | 结果 |
|------|------|
| 端到端质量 | 写作+角色扮演：三种 judge（不同家族 LLM）下均最佳 |
| vs 固定奖励 | 超对应 policy 2.107 / 4.767 分（两任务域） |
| 失效治理 | reward hacking 与覆盖失败（coverage failure）双降 |
| 奖励信息量 | 全程保持 reward informativeness（区分度不坍塌） |
| 消融 | 全面的固定奖励系统（大而全 rubric）无法保持可靠——必须随训练进化 |

为什么这套设计能证明论点：三 judge 交叉排除"自家人打分偏袒"；informativeness 的全程监测把"奖励还灵不灵"变成可观测曲线；消融直击替代假说"初始写好 rubric 就够了"——固定系统的失效曲线证明进化必要性。

## 六、效果优势的根源解释

**为何奖励系统必须 on-policy 进化？**

因果链：policy 对固定奖励持续优化（机制前提）→ 输出分布向高分区坍塌 + 空子被找到（hacking）（机制变化：区分度损失+博弈）→ 奖励梯度失去指向性 → 训练后期增益趋零甚至负向（固定系统的实测失效）；on-policy traces 是失效的最直接证据源（信息源优势）→ designer 据此做结构性修补（堵空子、恢复方差）（方法差异）→ 奖励持续提供有效梯度（机制变化）→ 质量全程上升（指标）。

**外部交叉验证**：Goodhart 定律的 RL 版（reward hacking 文献，Amodei et al. 2016 谱系）支持"固定奖励被博弈"的必然性；本系列前轮精读的 NeoHorse-1（09-10，RSI 系统）与 Co-Evolving Harnesses（Salesforce）都依赖组件级共进化维持系统可靠性——EvoRS 在奖励维度上与之一致（多研究共同支持的机制）；对抗性验证（GAN 谱系）中判别器随生成器更新是同一动态平衡原理。反方/边界：奖励进化引入新的失稳风险（designer 编辑可能引入新空子）；论文用"维护性编辑"约束缓解但未完全消除——开放任务的三 judge 评估本身也有 judge 偏差上限（与 GAUGE 同日的警告呼应），绝对质量数值应谨慎解读。

## 七、必要知识反推

- **领域知识层**：rubric 奖励的工程形态；开放任务的质量维度学。
- **方法论知识层**：RL 训练动态（分布坍塌、奖励博弈）；DAG 作为可编辑系统表示；on-policy 信号的使用。
- **工程知识层**：rollout 批管理；reward trace 的检测算法（hacking 模式识别、方差监测）；judge 编排。
- **融合关键节点**：把"奖励失效"从直觉升格为**可观测的 trace 模式**，再把修补从人工重写降为**结构化编辑**——两步对接完成"进化"从口号到机制。

## 八、通用性灵感

1. **被优化的目标需要与优化者共演化**。论文证据：固定奖励训练后期失效，进化奖励全程有效。推广：KPI 体系随业务策略年度复审（防 Goodhart 化）、对抗评测集随模型迭代更新。
2. **目标系统用可执行结构表示才可维护**。论文证据：Reward-DAG 使诊断与编辑程序化。推广：政策法规（可机读条款）、安全规范（可测试断言）。
3. **区分度坍塌是目标失效的先行指标**。论文证据：方差监测驱动更新时机。推广：任何评分体系（面试官打分、风控规则）都应监测自身方差——趋零即失效警报。
4. **维护性编辑优于推翻重写**。论文证据：designer 约束为结构修补。推广：制度改革的渐进 vs 重启（保留已验证组件降低引入新漏洞的风险）。
