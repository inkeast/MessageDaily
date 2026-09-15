---
title: "Harness or Model? Isolating the Harness Effect in Agentic Coding with a Contamination-Controlled Private Suite 精读"
date: 2026-09-15
draft: false
tags: ["论文精读", "Harness", "Agent", "Benchmark", "评测", "学术调研"]
categories: ["paper-reading"]
summary: "evolutionID GmbH 用 256 个私有任务的污染控制套件，首次把 agent 编程中 harness（驱动模型的软件层）作为唯一变量隔离测量。结论颠覆直觉：厂商原生 harness 无平均能力优势（±1.25pp 统计不显著），但按任务类型剧烈分化（仓库任务落后 9pp、竞赛任务领先 23.7pp）；中立 harness 每解一题成本反而高 1.3-1.6 倍。论文还自曝自家成本遥测存在缺陷并全量重算——测量诚实度的范本。"
---

> **论文链接**：[Harness or Model? Isolating the Harness Effect in Agentic Coding with a Contamination-Controlled Private Suite](https://arxiv.org/abs/2609.11987)
> **发表时间**：2026年9月（v2 修订版 9月8日）
> **机构**：evolutionID GmbH（德国企业，独立研究）
> **领域标签**：cs.SE / cs.AI — Agent 评测测量学

## 一、论文背景

**什么是 harness？** 在 LLM agent 语境里，harness 指"驱动模型干活的软件层"——它决定 agent 怎么读代码库、用什么工具、上下文里放什么、失败后怎么重试。Claude Code、Codex CLI、deepagents 都是 harness。模型是引擎，harness 是整辆车的传动与操控系统。

**为什么要研究 harness 效应？** 当下 agent 编程评测有一个结构性盲区：要么"固定 harness 只换模型"（如 SWE-bench 排行榜），要么"报告厂商的模型+harness 捆绑包"（如各家产品宣传）。一个团队已经选了模型、要选驱动软件时，这两个设计都回答不了"换 harness 到底值不值"。

**污染与成本两大拦路虎**：公开 benchmark 会泄入训练语料（contamination），厂商可能在不知不觉中针对已见任务优化；而 agent 评测普遍用"harness 自报的 token 用量"折算美元成本，但不同 SDK 对缓存 token 的计法不同，成本数字本身可能是错的——这篇论文用自己踩的坑证明了这一点。

## 二、论文定位和关联工作

| 研究谱系 | 代表工作 | 与本文关键区别 |
|---------|---------|---------------|
| 固定 harness 换模型 | SWE-bench 系列、Terminal-Bench | 无法回答 harness 选择问题 |
| 厂商捆绑评测 | 各产品自报成绩 | harness 与模型效应混杂 |
| 污染控制方法 | PrivateBench、截止日期注册表 | 本文将其系统化：机器可读 cutoff 注册表 + runtime drift gate |
| 成本测量批判 | 各家 telemetry 讨论 | 本文自曝 usage-semantics 缺陷并全量重定价 |

本文定位：**首个在污染受控私有套件上、以预注册协议把 harness 作为唯一变量做配对对比的研究**，同时给出成本遥测的方法学警示。

## 三、问题定义

具体场景：企业已选定前沿模型（Opus 4.8 / GPT-5.5），要在厂商原生 harness 与可移植中立 harness（deepagents）之间做选择。

抽象问题：**在模型固定、任务分布受控、污染受控的条件下，harness 对"解题率"与"每解一题成本"的因果效应是多少？**

形式化：给定任务池 T、模型 M、harness H ∈ {native, neutral}，估计 E[solve(t; M, H_native) − solve(t; M, H_neutral)] 及成本比 Cost/solved(M, H_native) / Cost/solved(M, H_neutral)，并以任务级 bootstrap 给出置信区间。

精妙之处：把"正确性"（patch 通过隐藏测试）与"自主完成"（在墙钟上限内自然结束）分离为两个端点——22/81 被超时取消的 run 其实已经写出了通过补丁，混淆两者会系统性误判能力。

## 四、问题解法

**套件构造**（256 任务双轨）：
- Track A（179 任务）：4 个私有生产代码库（AccessCtl 110 / IdentityApp 27 / FieldSvc 40 / BookingSvc 2），SWE-bench 式 FAIL_TO_PASS + PASS_TO_PASS 隐藏测试 + gold patch；
- Track B（77 任务）：截止日后发布的 LeetCode weekly 与 AtCoder 题，隐藏测试共 2.0 GB。

**三重污染控制**：
1. **冻结 cutoff 注册表**（2026-06-24，先于任何 scored run）：资格日 = 最严模型截止日（Opus 4.8: 2026-01-31）+ 30 天缓冲 = 2026-03-02；
2. **Runtime drift gate**：每轮 scored 运行前验证服务端模型身份（曾发现 DeepSeek 自报 cutoff 跨探测漂移，故自报仅作参考）；
3. **确定性池冻结**：screening 轮 C1/C3 不一致的 24 题全进主池，其余按 sha256 排序交替补足 80 题，主池由哈希锁定。

**评分与统计**：任务级 bootstrap 置信区间 + McNemar 检验 + 混合效应 logistic 模型（nativeness × vendor 交互）+ BH-FDR 校正。

**诚实度设计**：v1 版成本结论建立在错误遥测上（把一家 SDK 的缓存 token 约定套到全部三个 harness），post-study review 发现后，v2 从原始 per-turn 事件全量重定价，并公开修订记录。

## 五、评估指标与实验证据

| 对比 | 指标 | 结果 |
|------|------|------|
| Opus 4.8 native vs neutral | 解题率 | 48.8% vs 50.0%（Δ=−1.25pp, CI[−10.0, +7.5]，不显著） |
| GPT-5.5 native vs neutral | 解题率 | 55.6% vs 54.4%（Δ=+1.25pp, CI[−4.4, +6.9]，不显著） |
| Opus 仓库任务（n=61） | 分层解题率 | native 落后 9.0pp（CI[−17.2, −0.8]） |
| Opus 竞赛任务（n=19） | 分层解题率 | native 领先 23.7pp（CI[+2.6, +44.7]），交互 p=0.003 |
| 每解一题成本 | 重定价成本比 | neutral 为 native 的 1.3–1.6×（Opus）/ 1.2×（GPT-5.5） |
| 自主完成 vs 正确性 | 超时取消 run | 22/81 取消时已有通过补丁；neutral 触顶远多于 native（32 vs 1） |
| 工具调用数 | 中位数 | native 44.5 次 vs neutral 90.5 次（约 2×） |

为什么这套设计能证明论点：私有任务 + 冻结资格日排除"模型见过题"；同模型配对消除模型效应；分层分析揭示平均数掩盖的结构。作者明确标注分层结果是 post-hoc（分区是看数据后选的），需要设计性复制验证——这种克制本身是测量学成熟度的标志。

## 六、效果优势的根源解释

**为何原生 harness 竞赛强、仓库弱（而中立 harness 反之）？**

因果链（论文实验已支持的部分）：native harness 与自家模型在服务端深度耦合（如 prompt caching、工具协议优化）→ 竞赛类短任务受益于低延迟快路径（中位墙钟 392s vs 596s）；但仓库任务需要长程状态追踪与工具编排，native 的专有优化反而引入路径依赖 → 落后 9pp。中立 harness 工具调用翻倍（90.5 vs 44.5）→ token 消耗上升 → 每解一题贵 1.3-1.6×（阅读者对成本机制的部分为推测，作者标注 Anthropic 账户 58 个 run 无用量记录，计费顺序未定）。

**外部交叉验证**：本系列前轮精读的 Harness Effect（4.3× 接口效应，09-08 日报）与 Subagents vs Agent Skills（Cornell×MSR）均发现 harness 层差异可达数倍；SWE-bench 审计（Chowdhury et al. 2024）证明公开榜单的 grading harness 缺陷足以改变结论——多项研究共同支持"harness 是一阶变量而非二阶细节"。本次检索范围内未发现与"harness 无平均优势"直接矛盾的污染受控研究；但注意该结论条件性强：80 题主池、2 个模型、特定任务混合比，外推需谨慎。

## 七、必要知识反推

- **领域知识层**：agent harness 工程的构成（工具协议、上下文管理、缓存计费）；SWE-bench 式任务构造与隐藏测试设计。
- **方法论知识层**：预注册与池冻结的实验设计；任务级 bootstrap 与 McNemar 检验；交互效应的 permutation test；contamination 控制的 cutoff 算术。
- **工程知识层**：Docker manifest 驱动的评分环境（PostgreSQL/MSSQL/mock-OIDC sidecar）；token 遥测的 per-turn 事件流水与重定价；sha256 锁定主池的可复现性工程。
- **知识融合关键节点**：把"临床试验式预注册"移植进 agent 评测，与"成本遥测必须从原始事件重算"结合——两者的融合点在于承认测量仪器本身是研究对象。

## 八、通用性灵感

1. **捆绑报告掩盖归因**（核心思想：任何"系统级成绩"都应拆解到可归因变量）。论文证据：native/neutral 差异在分层后从零效应变成 ±24pp 双向分化。推广：模型选型、数据库选型、云厂商迁移评估都应做"单变量配对"而非读捆绑 benchmark。
2. **测量仪器要先于测量对象被审计**。论文证据：自家 cost telemetry 的缓存 token 约定错误曾支撑了错误结论。推广：任何 dashboard 指标（转化率、推理成本、延迟 P99）都应先验证采集语义再下业务结论。
3. **把"完成"与"正确"拆成两个端点**。论文证据：22/81 超时 run 已产出正确补丁。推广：人效评估中"按时交付"与"交付质量"分离；自动化运维中"任务结束"与"目标达成"分离。
4. **post-hoc 发现要标注为 post-hoc**。论文证据：竞赛/仓库分层 p=0.003 但作者明确标注"数据后分区，需设计性复制"。推广：A/B 测试中的 subgroup 分析、医学观察性研究同理——诚实标注探索性结论是可信度的来源。
