---
title: "Apodex 1.1 姊妹篇补遗：本日精读系列导览与 2026-08-25 学术全景"
date: 2026-08-25
draft: false
tags: ["论文精读", "Agent", "Harness", "AI趋势", "学术调研"]
categories: ["paper-reading"]
summary: "本文为 2026-08-25 精读系列的导览：16 篇触发顶会标准精读的论文横跨 Agent 评测反作弊、Harness 可学习化、经验资产化、因果测量方法学四大主题。本文给出全部精读的索引、跨论文趋势综合（verifier-grounded 成为共同底座、评测从'分数多高'转向'分数测的是什么'、产学研从联合发文转向资产+方法学互换），以及按读者角色（研究者/工程师/管理者）的阅读路线图。"
---

# 2026-08-25 精读系列导览与学术全景

> **系列说明**：本日 42 篇候选论文经逐页全文阅读与顶会标准评审，16 篇触发独立精读（链接见下）。本文是系列的导航与综合。

## 一、本日精读索引

### 主题 A：Agent 评测的"反作弊"深水区

| 精读文章 | 一句话核心 |
|---|---|
| [SWE Refactor Bench](/posts/2026-08-25-swe-refactor-bench-paper-reading/) | 行为评测的 Blindness 盲区：空 diff 骗过任何行为测试，三阶段协议下最强模型仅 47/100 |
| [CatchBench](/posts/2026-08-25-catchbench-paper-reading/) | 分数在标签过程公开之前不可解释：PRE/LIVE/POST 三信息状态审计竞技场+可采性门槛 |
| [Process Evaluation 三层（ICLR 2027）](/posts/2026-08-25-process-eval-scae-paper-reading/) | 动作/任务/步骤是三个层次：judge 的步骤归因有 collider 偏置（+0.537） |
| [Signal or Noise](/posts/2026-08-25-webdev-skills-bench-paper-reading/) | 长度匹配对照证明 Skill 注入平均负收益，两种机制需相反对策 |
| [MobilePA-Bench](/posts/2026-08-25-mobilepa-bench-paper-reading/) | 能力门设计：短板以乘法杀伤端到端，Memory 维度全员不及格 |

### 主题 B：Harness 从提示词到可学习系统

| 精读文章 | 一句话核心 |
|---|---|
| [Prime Agent](/posts/2026-08-25-prime-agent-paper-reading/) | 同一模型换 harness：ARC-AGI-3 从 30.2% 到 95.5% 超人类基线 |
| [AutoSaddler](/posts/2026-08-25-autosaddler-paper-reading/) | harness 优化=离线 mini-batch 学习：三基准 +9~10pp、样本效率 10× |
| [Task-CoEvolve](/posts/2026-08-25-task-coevolve-paper-reading/) | "在哪评"也是优化变量：20% 评估预算匹配全量搜索 |
| [Risa](/posts/2026-08-25-risa-routing-paper-reading/) | MoE 路由轨迹做行为坐标系：决策 token 上的一致性是正确性信号 |

### 主题 C：经验/技能的资产化与其生命周期

| 精读文章 | 一句话核心 |
|---|---|
| [LongWoF-Bench](/posts/2026-08-25-longwof-bench-paper-reading/) | 只有验证器确认的执行经验才可复用：Gene vs Skill +8.7~15.5pp |
| [SkillAlchemy](/posts/2026-08-25-skillalchemy-paper-reading/) | 来源接地的程序准入：自动技能首次比肩人工且注入零传播 |
| [Repo2Skill-Evo](/posts/2026-08-25-repo2skill-evo-paper-reading/) | 技能静默失效：最强模型维护 F1 仅 69.7%，定位与编辑双瓶颈 |

### 主题 D：因果测量与系统正确性

| 精读文章 | 一句话核心 |
|---|---|
| [Ascp 上下文分配定律](/posts/2026-08-25-ascp-context-allocation-paper-reading/) | 窄窗多轮胜宽窗单轮 +0.144，收益全部来自反事实信号 |
| [The Mask Is Not the Model](/posts/2026-08-25-mask-not-model-paper-reading/) | 双前向审计 192/192 定位，挖出 Zamba2/Nemotron-H 真实泄漏 |
| [NFV 神经形式验证](/posts/2026-08-25-neuro-formal-verification-paper-reading/) | 无证明即弃权：92.2% 精度 vs judge 的 72% |
| [ERPO](/posts/2026-08-25-erpo-paper-reading/) | 查询分布漂移是 RL 崩溃根源：Query-KL 根治高温崩溃 |

### 系统级工作

| 精读文章 | 一句话核心 |
|---|---|
| [Apodex 1.1](/posts/2026-08-25-apodex-1.1-paper-reading/) | 双扩展面：环境构建+智能体协调作为与模型规模并列的扩展维度 |

## 二、跨论文趋势综合

**趋势一：verifier-grounded 成为共同底座。** 16 篇精读中 11 篇的核心机制与"可验证信号"有关：Apodex 的交付契约、LongWoF 的验证器确认经验、AutoSaddler 的失败信号学习、SWE Refactor 的三阶段验收、NFV 的机器检查证明、Ascp 的因果反事实、CatchBench 的可采性门槛……"从生成质量转向验证质量"已从口号变成 2026 年的方法论共识。**谁的验证器更便宜、更细粒度、更难被欺骗，谁的学习/评测循环就更快**。

**趋势二：评测的竞争维度从"分数多高"转向"分数测的是什么"。** 本日四篇评测方法学（CatchBench/Signal or Noise/Process Evaluation/MobilePA）不约而同攻击评测的有效性本身——标签泄漏、长度混淆、层次混淆、Blindness。这标志着 Agent 评测进入"元评测"阶段：**下一步的护城河不是更大的基准，而是别人无法质疑的测量方法学**。

**趋势三：产学研从"联合发文"转向"资产+方法学互换"。** 本日合作的典型结构：企业出带真实失败信号的资产（微软的 GAIA2/SWE-Bench Pro、EvoMap 的 Evolver 引擎、字节的仓库语料、阿里的生产 RL 环境），高校出可泛化的形式化与测量（POSTECH 的离线学习框架、清华的统计检验、南大的因果推断）。这种分工比论文署名互换深刻得多——**企业的数据资产在高校方法学加持下变成行业基础设施**。

**趋势四：负面证据被反复证明是最有价值的信息形态。** LongWoF 的"失败修正"是 Gene 的核心成分、Signal or Noise 的反模式 Rn 唯一可靠、SWE Refactor 的对抗验证专攻"作者没想到"、NFV 的反驳臂产出 witness——四篇独立工作从不同角度收敛到同一结论：**"什么会失败"的知识比"该怎么做"的知识更稀缺也更可迁移**。

## 三、按读者角色的阅读路线

- **研究者**（评测/Agent 方向）：主题 A 全部 + Ascp——评测有效性是当前最热的投稿空白带；
- **Agent 工程师**：主题 B 全部 + SkillAlchemy——harness 优化与技能准入是可直接落地的工程实践；
- **技术管理者**：Apodex 1.1 + MobilePA-Bench + SWE Refactor——理解 Agent 的真实能力边界与下一代基准的导向；
- **方法学爱好者**：Process Evaluation（因果推断×评测的完整示范）+ NFV（信任边界设计的教科书）。

---

*本系列 16 篇精读全部基于论文 PDF 逐页阅读撰写，数字均可在原文溯源。*
