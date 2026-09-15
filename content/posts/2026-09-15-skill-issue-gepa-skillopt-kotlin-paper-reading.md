---
title: "Skill Issue: Lessons from Optimizing Repository SKILLs for Coding Agents 精读"
date: 2026-09-15
draft: false
tags: ["论文精读", "Harness", "Agent", "Coding", "学术调研"]
categories: ["paper-reading"]
summary: "TU Munich × JetBrains Research 的产学研负结果研究：在真实 Kotlin 仓库的合并 PR 反向挖掘任务上，GEPA 优化 SKILL 文档仅 +4.9pp（统计不显著）、SkillOpt 仅 +0.1pp——此前文献自报的巨大增益（55%→82%）是在弱模型弱 harness 配置下测出的。论文进一步证明 pass-rate 增益量级与二元判决本身的误标率（10.7% 盲重试通过）同阶，测量仪器而非优化器才是瓶颈。maintainer 盲读却确认 SKILL 含真实项目知识——分数之外的价值。"
---

> **论文链接**：[Skill Issue: Lessons from Optimizing Repository SKILLs for Coding Agents](https://arxiv.org/abs/2609.12742)
> **发表时间**：2026年9月
> **机构**：Technical University of Munich × JetBrains Research（**产学研合作**：JetBrains 提供真实 Kotlin 仓库、maintainer 评审与 JVM 工程约束；TUM 出测量方法学）
> **领域标签**：cs.SE — Agent Skill/Harness 优化

## 一、论文背景

**什么是 SKILL？** 随代码版本化管理的 .md 知识文件（agentskills.io 规范、Anthropic 支持、45+ 工具兼容），harness 在每个任务前把它注入 agent 上下文，相当于"这个仓库的岗位操作手册"。AGENTS.md 格式已被 6 万+ 开源项目采用。

**为什么要优化 SKILL？** 裸仓库没有现成 SKILL，手工撰写昂贵。近期工作（gskill/SkillOpt）声称可以自动合成：用优化器对 benchmark 迭代编辑文档。但这里有个被忽视的前提——**优化需要任务集和评分器，而裸仓库两者皆无**。

**既有评估的隐患**：gskill 报告 55%→82% 的 resolve rate 提升，却是在 gpt-5-mini + mini-swe-agent 的弱配置上测的；换 Claude Code + Sonnet 4.5，无 SKILL 即达 100%/94.8%——任务太简单时，分数天花板抹平了文档的价值。

## 二、论文定位和关联工作

| 谱系 | 工作 | 关键区别 |
|------|------|---------|
| SKILL 自动合成 | gskill（GEPA 2026）、SkillOpt（Yang et al. 2026） | 它们在合成任务/弱配置上评估；本文用真实 PR 挖掘+强配置复现 |
| Prompt 优化 | GEPA（Agrawal et al. 2025） | GEPA 自由重写整个候选；本文将其作为 proposer 之一受控对比 |
| 仓库数据集挖掘 | SWE-smith（缺陷注入） | 注入式缺陷太小太局部；本文挖真实开发者变更 |
| 测量学批判 | Sahoo et al.（10.7% 盲重试通过） | 本文量化"增益量级 ≈ 误标率"的仪器问题 |

本文定位：**首个在"裸仓库+强 agent"真实条件下复现 SKILL 优化并系统分析其测量极限的工作**。

## 三、问题定义

具体场景：一个没有 benchmark 的仓库，想自动生成能帮 coding agent 的 SKILL 文档。

抽象问题：**在评估信号被 agent 自身 run-to-run 方差淹没的条件下，自然语言工件的优化何时还能产生可检出的效应？**

形式化：给定仓库 R 的任务集 D（从合并 PR 反向挖掘）、冻结 agent A、候选文档 s，优化目标 max E[score(A(x; s))]——但 Var[score | 同一 s] ≥ 效应量 θ 时，检出力不足。

精妙之处：把"优化器是否有效"的问题转化为"测量仪器能否检出"的问题——并给出单仓库历史所能提供的任务量上界（kotest 660 PR → 119 任务存活）。

## 四、问题解法

**Reverse-PR mining（反向 PR 挖掘）**：
- 正向（SWE-bench 式）会把任务锚定在不同历史时点，依赖缺失导致测试不编译，且优化出的 SKILL 描述"几个月前的仓库形态"（模块路径、API 已漂移）；
- 反向：把仓库 checkout 到单一冻结 base commit，把每个合并 PR 的实现**反向回滚**，FAIL_TO_PASS 集合 = 回滚后由通过转失败的测试——一次修复了跨历史漂移与评分目标缺失两大问题。

**Pairwise 评分**（绝对分失败后的替代）：seed SKILL 与候选在同任务同条件下对打，三步判决：①改测试文件或谎报成功者输；②隐藏 FAIL_TO_PASS 全过者赢；③测试通过率/诚实度/diff 大小/工具调用数加权破平。seed 对自己恰为 0.5——候选必须打败 0.5。

**两个 proposer 的受控对比**：GEPA（Pareto 前沿采样+反思 LM 自由重写）vs SkillOpt（受限编辑+严格改进门+拒绝编辑缓冲）。

## 五、评估指标与实验证据

| 实验 | 结果 | 证明力 |
|------|------|--------|
| RQ1 挖掘产出 | koog 660→119 任务、ktor 452→131、kotest 截取 100（约 1/5 存活率） | 真实仓库可挖任务量有硬上界 |
| RQ2 held-out 增益 | GEPA +4.9pp、SkillOpt +0.1pp（每仓 20-26 测试任务，均不显著） | 复现了 gskill 量级但落在噪声内 |
| RQ3 方差分析 | 增益量级 ≈ agent 自身 run-to-run 波动 ≈ 10.7% 盲重试误标率 | 仪器分辨率不足是根因 |
| RQ4 maintainer 盲读 | koog maintainer 确认两优化器 SKILL 均含"只有做项目才知道的知识"，open issues 上用时减半、成本更低 | 分数之外的独立价值证据 |
| 成本 | 单 rollout $0.84；200 次 GEPA 优化 = 数百美元/仓 | 该范式的经济边界 |

为什么这套设计能证明论点：pairwise 评分排除了"任务本身难度"的干扰（同任务对打）；反向挖掘排除历史漂移（SKILL 描述的仓库形态 = agent 面对的形态）；maintainer 盲读提供了不依赖 pass-rate 的外部效标。

## 六、效果优势的根源解释

**为何既有 SKILL 优化论文的增益在真实条件下消失？**

因果链：合成任务（SWE-smith 注入式缺陷）小而局部（方法差异）→ 强 agent 无文档也能解，分数饱和在 94.8-100%（机制变化：天花板效应）→ 弱配置下测出的 27-37pp 增益不可迁移（指标差异）；真实 PR 任务下任务量只有 20-26/仓（方法差异）→ score 的方差 = agent 方差 × 任务采样方差，标准误 > 效应量（机制变化：检出力不足）→ +4.9pp 落在 CI 内（指标表现）。

**外部交叉验证**：SkillOpt 原论文（Yang et al. 2026）自报增益与本系列前轮精读的 COBRA-Skills 复现的增益方向一致，但两者均在合成/半合成任务上评估；Harness 效应研究（2609.11987）证明 harness 更换可引入 ±24pp 波动，佐证"agent 方差是一阶噪声"；SWE-bench Verified 审计发现 7.8% "正确"补丁过不了开发者自己的测试，与 10.7% 盲重试率同量级——多项独立研究共同指向"二元 pass-rate 在强 agent 区间分辨率不足"。本次检索范围内未发现"强配置+真实 PR 任务下 SKILL 优化显著有效"的正面反例——这本身是重要的空白。

## 七、必要知识反推

- **领域知识层**：JVM/Kotlin 工程生态（kotest/ktor/koog 的构建与测试结构）；SKILL/AGENTS.md 规范。
- **方法论知识层**：GEPA 与 SkillOpt 两类反思驱动的自然语言工件优化机制；pairwise 与绝对评分的统计性质；检出力与样本量分析。
- **工程知识层**：Docker 内逐任务验证（gold patch 反向应用、测试翻转集合提取）；rollout 成本核算；单冻结 base 的版本控制。
- **融合关键节点**：把"优化算法评估"与"心理测量学的效度/检出力框架"融合——认识到 proposer 的选择空间受限于评分器的分辨率，是全文的方法论支点。

## 八、通用性灵感

1. **评估饱和是隐形的零空间**。论文证据：强 agent 在合成任务上无文档即达 ~100%，任何文档优化都测不出效果。推广：模型压缩评估用已饱和的 benchmark、推荐系统用头部用户评估长尾策略，都会落入同一陷阱。
2. **方差预算决定可优化空间**。论文证据：单仓库历史只够 20-26 个任务，效应量 < 噪声。推广：任何"小样本+高方差"的优化场景（小团队 A/B 测试、罕见病疗法）都应先算检出力再启动迭代。
3. **被优化工件的价值可以用独立效标衡量**。论文证据：maintainer 盲读与 open issue 实操（时间减半）绕过了 pass-rate。推广：文档/提示词/流程的优化可引入专家盲评、下游时间成本等外部效标，避免单一指标过拟合。
4. **反向回滚是构造"真实但可评分"任务的通用技术**。论文证据：reverse-PR 同时解决历史漂移与评分目标。推广：从任何有版本历史+测试的资产（数据管道、API、配置）都能机械构造带 ground truth 的变更任务。
