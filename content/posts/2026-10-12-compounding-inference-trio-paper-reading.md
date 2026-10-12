---
title: "无权重经验复利：GitSwarm×AAArena×RoboRSI 三篇合读 精读"
date: 2026-10-12
draft: false
tags: ["论文精读", "Agent", "AI自进化", "具身智能", "Benchmark", "Harness", "学术调研"]
categories: ["paper-reading"]
summary: "当模型参数冻结，经验还能不能跨尝试积累？本精读合读 2026 年 10 月三篇论文：GitSwarm 把推理时计算组织成持久可分支的 Git 贡献图，回答「经验存在哪」；AAArena 用 12 个竞赛游戏与 1920 个人类程序验证密集回放信号，回答「经验学什么」；RoboRSI 用技能分支与验证发布机制，回答「经验怎么改才安全」。三篇分别代表经验积累的持久化、信号密度、验证门控三条路线，合成一幅无权重复利的完整图景。"
---

> **论文链接**：
> - GitSwarm: [arXiv 2610.04862](https://arxiv.org/abs/2610.04862)（项目页：[veds12.github.io/git-swarm](https://veds12.github.io/git-swarm/)）
> - AAArena: [arXiv 2610.12341](https://arxiv.org/abs/2610.12341)（项目页：[aaarena.net](https://aaarena.net)，代码：[THU-CST-SAST/AAArena](https://github.com/THU-CST-SAST/AAArena)）
> - RoboRSI: [arXiv 2610.12424](https://arxiv.org/abs/2610.12424)（项目页：[lab.noematrix.ai](https://lab.noematrix.ai/blog/2-roborsi-research-preview/)，代码：[nssmd/RoboRSI](https://github.com/nssmd/RoboRSI)）
> **发表时间**：2026 年 10 月（分别为 10-04 / 10-08 / 10-08 提交）
> **机构**：GitSwarm 由 Meta Superintelligence Labs 主导，联合 Mila、蒙特利尔大学、哥伦比亚大学、CMU、普林斯顿大学；AAArena 为清华大学计算机系与智能产业研究院 26 位作者，获 Z.ai 赞助；RoboRSI 来自 Noematrix 创业团队，Cewu Lu（上海交通大学）与 Chuan Wen 为双通讯——注意其论文正文首页未列机构栏，机构信息以项目页与团队公开信息为准
> **领域标签**：cs.AI / cs.RO，推理时计算、Agent 自进化、技能学习

# 〇、三篇为什么合读：无权重经验复利

三篇论文各自独立发表，却精确互补地回答了同一个问题的三个侧面：**当模型参数 θ 冻结时，经验如何在尝试之间积累？**

- GitSwarm（基础设施答案）：经验存在哪？——存在持久、可分支、可归因的 Git 贡献图里。
- AAArena（评测信号答案）：经验学什么信号？——密集回放（dense replay）远胜二值胜负。
- RoboRSI（工程落地答案）：经验怎么改才安全？——归因到技能分支、验证通过才发布。

我们把这个统一主题称为「无权重经验复利」（compounding experience without weight updates）。它与 Best-of-N 式推理时扩展的本质区别在于：BoN 的每次采样互相独立、互不可见，算力花完即蒸发；而这三篇工作的每次尝试都留下可被后续尝试检查、复用、挑战的持久痕迹。GitSwarm 的数字最直观：编程任务中最终选中解的谱系平均覆盖 81.6–92.6% 的历史贡献图——大部分「学习」发生在尝试之间的传递里，而非单次尝试内部。

# 一、论文背景

## 1.1 推理时计算与它的「蒸发」问题

2024 年以来，推理时计算（inference-time computation）成为与训练时扩展并行的第二条 Scaling 轴线：Best-of-N 采样、自我一致性、树搜索、过程奖励模型引导搜索等方法证明，参数固定的模型也能靠「多想一会儿」换来性能提升（Snell et al. 2024, arXiv:2408.03314）。但这条路线有个隐含前提：好的回答已经存在于采样分布中，只等验证器把它挑出来。

这带来一个结构性浪费：无论单次尝试多聪明，episode 结束后中间工作全部蒸发——探索过的死路、验证过的发现、甚至整个失败假设，都不会留给下一次尝试。第 N+1 次采样与第 1 次一样从零开始。用 GitSwarm 作者的话说，推理时计算缺少「复利」：每次尝试是单利，不是利滚利。

## 1.2 经验外置：从 Reflexion 到 Heuristic Learning

另一条脉络试图把经验存到模型外。Reflexion 把失败复盘写成文字存入情景记忆；ExpeL 从成功/失败轨迹对比中提炼跨任务原则；Voyager（arXiv:2305.16291）在 Minecraft 里把技能存成可执行 JavaScript 代码，160 轮发现的物品数是基线 3.3 倍，且把技能库带进新世界可零样本解新任务。

2026 年 5 月，OpenAI 后训练工程师翁家翌在博客《Learning Beyond Gradients》中把这条路线命名为 Heuristic Learning（HL）：coding agent 读失败、改代码、跑环境、看回放，循环迭代。他的 Codex 实验在 Atari Breakout 上从 99 分一路改到 864 理论满分，全程零参数更新——知识以代码、测试、日志的形式沉淀，可读、可改、可审计。这波 HL 浪潮正是 AAArena 的直接理论前提：该论文把 AHL（Adversarial Heuristic Learning）形式化为 HL 在对抗博弈上的实例化（基础模型 θ 固定，仅修订可执行策略 P 与开发状态 C）。

## 1.3 机器人自改进与「改坏」风险

在物理世界，同一问题更尖锐。ASPIRE（NVIDIA GEAR，arXiv:2607.00272）让机器人持续写控制程序并把验证过的修复蒸馏成跨形体技能库，在 LIBERO-Pro Long 上零样本 31% vs 前方法 4%；ENPIRE（arXiv:2606.08321）让 coding agent 在真机上闭环改策略代码，训练出 99% 成功率的灵巧操作策略；CaP-X 则把「代码即策略」做成机器人控制的基准框架。但真机上的失败会砸到真实的物体——「改哪、怎么验证、错了怎么回退」从学术问题变成安全问题。RoboRSI 正是冲着这个痛点：自进化必须 stable（不破坏已有能力）、efficient（少浪费算力）、reusable（经验可跨任务复用）。

三条脉络合起来就是本合读的背景：算力可以买到尝试次数，但买不到尝试之间的记忆、信号与安全。三篇论文分别把这三样东西做成了系统。

# 二、论文定位和关联工作

## 2.1 GitSwarm：分布式 Agent 系统谱系的「Git 原生」一极

GitSwarm 处在多条谱系的交汇点：

- **搜索式推理**：Tree-of-Thoughts、LATS 把推理组织成搜索树，但树在 episode 内，不跨尝试持久；
- **经验学习**：Reflexion/ExpeL 的文字记忆不可执行、无结构版本控制；Voyager 技能库只有平面追加，无分支竞争；
- **多智能体框架**：MetaGPT/AutoGen 预设角色与对话流，共享对话易过早收敛单线；GPTSwarm 可组合计算图但无持久记忆；
- **版本控制记忆**：AgentGit 给 MAS 工作流加 commit/revert/branch，但定位是基础设施层，不承载语义依赖；
- **自动化科研**：AlphaEvolve（Google DeepMind）用进化搜索改代码，验证靠评估器级联——GitSwarm 论文 Table 1/2 直接与之对比，指出 AlphaEvolve 单种群单目标，无跨分支语义依赖声明。

GitSwarm 的定位结论：它是第一个同时满足七项设计维度（共享记忆/持久分支/自主选择/无固定角色/异步/跨分支依赖/物理起点与语义来源分离）的系统（论文 Table 1/2，p21-22），把「科研团队用 Git 协作」这一人类实践完整移植给异步 agent 集群。

## 2.2 AAArena：博弈评测从「对战」到「学习」的范式切换

Table 5（p13）梳理了协议族：GameBench/TextArena 只测直接对战；GACL one-shot 生成策略；CodeClash/CC:Ladder 测持续 coding 对战；EvoPolicyGym 测策略进化。AAArena 的差异点：12 个定制竞赛游戏 + 1,920 个冻结人类程序的参照天梯 + agent 自主调度评估（自己决定何时修订、何时验证）+ 双预算协议。它不是又一个排行榜，而是把「学习引擎」本身作为被测对象。

## 2.3 RoboRSI：技能库路线的「版本工程」补全

ASPIRE/ENPIRE 因协议不兼容（并行种群搜索 / 物理策略精化循环）无法同协议对比，RoboRSI 选择与最强 code/tool 执行 agent 对比：CaP-X、Maestro、OpenETA（上海创智学院邱锡鹏团队的开源 ETA 框架，LIBERO 上 Pass@5 达 90% 的直接竞品），统一 GPT-5.6-SOL backbone。RoboRSI 在 OpenETA 最强的 LIBERO-PRO 上反超 11.0 pp，靠的不是更强的模型，而是技能层级 + 验证发布这层「版本工程」。

## 2.4 三篇合起来在谱系中的位置

| 路线 | 之前的方法 | 三篇的突破 |
|---|---|---|
| 持久化 | BoN/树搜索：尝试间无记忆 | GitSwarm：尝试间共享贡献图，94.7% 贡献被复用 |
| 信号密度 | 天梯排名/胜负二值反馈 | AAArena：dense replay 使 Elo 大幅领先 binary |
| 验证门控 | 直接改代码、信任工具返回 | RoboRSI：分支内修订 + 验证发布，假成功被挡在门外 |

# 三、问题定义

三篇面对的具体场景不同，但抽象出的是同一个数学结构。

## 3.1 共同抽象

**给定**：冻结参数 θ 的基础模型；一个可验证（或部分可验证）的任务环境 T；交互预算 B。
**求**：一个随尝试次数增长而单调不退化的外部经验载体 E_t，使任务得分 f(E_t) 随 t 提升。
**约束**：不更新 θ；每次尝试的成本有界；修改不能破坏已验证的能力（RoboRSI 明确要求，GitSwarm 通过分支实现，AAArena 通过 retained-champion 机制实现）。

## 3.2 三篇的实例化差异

| 维度 | GitSwarm | AAArena | RoboRSI |
|---|---|---|---|
| 经验载体 E | Git 贡献图（原子 commit + informed_by 语义依赖） | 可执行游戏程序 π + 开发状态 C | 技能层级图 G_t（compound/atomic/base 三层） |
| 学习信号 | 后续 worker 的检查/复用/提名 | dense replay（状态-动作-后果）+ Elo | 执行路径上的最早失败节点 v* |
| 安全机制 | 分支隔离 + 原子性 | retained-champion 快照 | 验证算子 V：review+测试+回放无回归才发布 |

这个抽象的精妙之处：把「学习」从参数空间搬到版本空间。三个系统本质都在回答版本控制理论的老问题——如何让并行修改不互相破坏、让历史可追溯、让好的变更存活。GitSwarm 论文的类比是直接的：informed_by ≈ Git 的语义化 parent，技能分支 ≈ feature branch，验证发布 ≈ code review + CI 门禁。而 AAArena 与 RoboRSI 是这个抽象在两个方向的退化实例：AAArena 只有单线版本（一个策略程序的历史快照链），把全部复杂性放在信号设计上；RoboRSI 保留完整层级版本图但串行开发，把复杂性放在影响面控制上。三篇合读可以看到同一抽象在不同约束（并发度、物理成本、反馈类型）下的形态适配。

# 四、问题解法

## 4.1 GitSwarm：把推理组织成去中心化 Git 协作

**类比**：一个异步开源社区接了一个长期悬赏任务，所有 PR 永久可见、无人指挥、最终由社区投票选出最佳。

四大机制（§2, p2-5）：

1. **共享可分支仓库**：B 个同质 worker 异步工作，各自检查已有贡献，自主选择推进方式（探索/实验/验证/精炼/合成），成果以原子 commit 发布。三大设计原则：persistence（中间发现活得比作者久）、branchability（竞争方案并行发展）、attribution（跨分支依赖可识别）。
2. **informed_by= 语义依赖**：每个 commit 除物理 Git parent（工作起点）外，必须声明语义来源集合 U_i——「本工作依赖了哪些跨分支发现」。这是与普通分支模型的关键差异：无需 merge 即可继承一个分支同时引用另一个分支的结论（p3 公式(1)）。
3. **异步 episode 协议**：每个 episode 以三种动作之一结束——Contribute（发布经验证的原子 commit）、Nominate（提名现有候选）、Abstain（自愿弃权不耗预算，ProgramBench 中约 1/6 worker 弃权，94.9% 判定合理）。
4. **去中心化读出**：预算耗尽后获最多提名的候选胜出，无中央裁判。dashboard 只报告状态（gs_dashboard/gs_inflight），不分派任务。

worker 不读全图：编程任务每个 worker 平均只显式打开 5-7 个候选 commit（B100 运行中有 72 个候选可用时），仓库是可搜索记忆而非全量转录。

最有说服力的是三个 GPU 架构研究任务（§3.3, p6-8）——这是「复利」在开放研究场景的直接检验：

- **Residual Matrix Transformer**：两个 12-worker swarm（Codex GPT-5.6-sol，72 小时，8×H200）在 1.3B token 训练预算下 held-out loss 达 2.890/2.894，比参考实现 3.147 好 0.25+；Forced GPT 单智能体用 2.3 倍 token（3.0B）只到 2.927。最佳架构声明依赖跨 24 个 commit、24 个 worker——最终模型 6 个机制中 4 个源自其他分支。
- **Looped Transformer**：12-worker swarm 26 小时内做了 395 个架构实验、12 个 idea 家族，protected-dev loss 3.187 vs 参考 3.265；parameter-matched control 3.212（保留 2/3 增益），说明增益不纯靠加参数。胜出模型声明依赖跨 89 commits/89 workers。
- **NanoChat**：20-worker swarm 在 24 活跃小时内经 1,465 次成功评估把验证 BPB 从 0.993 降至 0.905（−8.81%），incumbent 从 50.3M 增至 1.72B 参数。trigram 失败链最典型：2.18h 失败（0.9299）→ 23.08h 另一 worker 复用其 hash 做 shared-table → 放大成 24 小时 incumbent 组件。

## 4.2 AAArena：AHL 对抗启发式学习协议

**类比**：棋手请了位教练（LLM），教练不动手对弈，而是读规则书、看录像、改棋手的「作战手册」（可执行程序），再约训练赛验证。

- **输入侧**：agent 读游戏资源、回放、笔记，修订代码策略 π_{t-1}→π_t；禁止联网搜索、禁止读对手代码——考察的是「吃透规则与反馈」的能力，不是检索能力。
- **验证侧**：两级反馈。Small Match 对自选对手打，返回 dense replay（每次决策的状态/动作/后果）；Full Eval 对冻结人类程序池打，返回 Elo/rank/胜负。双预算 B_s=128 个小赛单元、B_l=16 次全池评估，agent 自主分配——这个设计把「何时学习、何时考试」也交给了被测 agent，考察的是元层面的调度能力。
- **计分**：retained-champion——取运行内官方评估的最高 Elo，保证探索性修订回退时不丢已有增益。
- **基准规模**：12 个真实竞赛游戏，规则复杂度从 Pacman 的 1,127 AST 节点到 AntWar2 的 6,368（参照：国际象棋 1,551、Go 仅 501）；对手池 44-323 个存档人类程序，共 1,920 个。

## 4.3 RoboRSI：TSR 技能层级 + 四角色流水线

**类比**：软件公司的分层 bug 修复——实习生报「页面崩了」，定位到具体模块（atomic skill），改完先跑回归测试，再由经理 review 合入，绝不许实习生直接改整个代码库。

- **TSR 层级**：任务分解为 compound（复合）/ atomic（原子）/ base（基础）技能三层，每技能有 scoped 责任与显式输入-输出契约。归因规则：分解不当改任务结构、组合不可靠改复合技能、操作失败改原子/基础技能。这个「三层归因路由」是整个系统的骨架——任何执行失败先归因到层，再归因到分支，最后才动手改。
- **形式化**：Reviewer 沿执行路径返回最早失败节点 v*（Eq.1）；修订范围 S_t 限定为 v* 拥有的子层级，patch 只能改 S_t 内节点；验证算子 V 要求通过 review、功能测试与近期历史 H_t 回放无回归，否则返回空 patch（Eq.2）；修订影响面有依赖局部性上界（Eq.3）。
- **多智能体**：Manager（分解/审查/决定发布）、Planner（单原子任务生成计划）、Engineer（工具执行+补写技能）、Reviewer（判断达成、定位偏离、写 patch）。每个 agent 只看自己步骤相关的层级部分，上下文短且修改可溯源。这条流水线的本质是把「执行者不能当自己的裁判」制度化为架构——Engineer 亲手写的代码，必须经 Reviewer 独立核验才能进发布流程。
- **双向抽象**：人可以任务级介入纠偏（每次纠偏保留为该技能的持久指导）；复合技能固化条件：≥3 次成功执行且工具序列共享 ≥60% 最长公共子序列。固化后的 compound skill 运行时重算所有参数、不存坐标——存的是经过验证的调用顺序骨架，而非特定场景的快照，这是它能跨场景复用的原因。
- **真机**：WheelSingleArm M1 平台（Athena Pro Max 移动底座 + RealMan 机械臂 + Zhiyuan 夹爪，手持+底盘双 RGB-D），家庭地面清理任务，104 轮真机迭代开发、累计 24 小时运行。真机验证 = 触发 run + 最近 4 次合格 run 的离线回放 + 回归测试（Round 25 一次验证含 172 放置 + 863 manager + 97 真机接口测试）。

## 4.4 全景对比

| 组件 | GitSwarm | AAArena | RoboRSI |
|---|---|---|---|
| 谁改经验 | 任意 worker 异步改 | 被测 agent 自己 | Engineer 写、Manager 发布 |
| 改的单位 | 原子 commit | 整个策略程序 | 技能分支内 patch |
| 隔离机制 | Git 分支 | 快照 retained-champion | 子层级 S_t 限定 |
| 合入门控 | worker 提名 | 本地验证通过 | review+测试+回放三重 |
| 失败的价值 | 显式保留可引用 | 复盘进开发状态 | 定位最早失败节点 |

# 五、评估指标与实验证据

## 5.1 GitSwarm：复利用数字说话

| 基准 | GitSwarm | 最强 baseline | 差距 |
|---|---|---|---|
| ProgramBench 50 题（B=500, Codex workers） | **79.4%** | Forced Codex 65.1% | +14.3 pp |
| ProgramBench（GPT-5.5, B=300） | 71.2% | 同上 | +6.1 pp |
| IMOProofBench 30 题（Gemini-3.1-Pro, B=160） | 29.0 完全正确 | bash agent 26.5 / mini-SWE ≤23 | +2.5 / +6 题 |
| RMT 架构研究 held-out loss（1.3B tokens） | 2.890/2.894 | 参考 3.147（Forced GPT 2.927 需 3.0B） | −0.25+ |
| Looped Transformer dev loss | 3.187 | 参考 3.265 | −0.078 |
| NanoChat 验证 BPB（24h） | 0.905（−8.81%） | 初始 0.993 | 24 活跃小时 1,465 次评估 |

注意作者自己的诚实标注：GPT-5.5-high workers 在 IMOProofBench 一次运行解出全部 30 题（B=40/80），但带 oracle verifier 的 Best-of-N 在更低 token 消耗下也达上限——饱和基准不构成独有优势；GitSwarm 的真正证据在复用指标：**94.7% 的贡献随后被构建引用**；选中解 ancestry 覆盖 81.6–92.6% 贡献图，其中 Git-parent 血缘只占约 1/4，其余全靠跨分支 informed_by 连接。

失败复用是第二个证据簇：NanoChat 70.1% 负标签 commits 被后续贡献引用、Looped Transformer 74% 负架构结果被引用、RMT 69/120 负 commits 中约 30% 被引用为证据。三个复活实例：RMT 的单 token 记忆失败 49.5 小时后被推广为 multi-lag temporal mixer 进入最终模型；Looped Transformer 被 128 样本小评估误杀的 adapter 经 512 样本重评翻案成为胜出机制；NanoChat 的 trigram 失败 hash 方案 21 小时后被复用为 shared-table 组件。

行为模式的自适应分工也值得注意（§3.5, p10-11）：编程任务中各活动（构建/验证/精炼）持续到终止，winner 中位出现于 93.1-98.0% 运行进度——后期仍在改进；证明任务则早期转向选择，选择活动份额从 B=20 的 44.2% 涨到 B=160 的 69.7%，winner 出现于 8.6-17.5% 进度——一旦有人解出，后续 worker 自发转为评估与提名。没有人指派角色，分工从任务结构中涌现。消融：异步比同步波快 1.30×（38.0 vs 49.3 min）且仅 −1.05 pp；central selector 读出 +2.34 pp 但 central aggregator −3.87 pp——去中心化不是全面占优而是工程权衡。

## 5.2 AAArena：信号密度与复杂度天花板

主结果（7 个模型配置×12 游戏×3 次取中位）：Opus5.5+Claude Code 拿 **6 块金牌**（Pacman 2703.9、SnakeGo 1860.9 等 Elo rank-1），9 个游戏 Elo 最高或并列最高；GPT6-sol 4 金、GLM-5.3 2 金。**规则复杂度与成绩强相关：低复杂度 6 游戏中 5 个被登顶，高复杂度 6 个仅 1 个**（AntWar2 AST 6,368 vs Pacman 1,127）。

关键消融（GLM-5.3）：

| 消融 | dense vs binary | 对手选择 | 3 倍预算 |
|---|---|---|---|
| Pacman | 2236.1/rank1 vs 1710.1/rank11 | model 2096.8 vs top-4 1897.0 | — |
| AntWar | 1118.7/rank9 vs 880.6/rank22 | — | 1977.1/rank9→2117.0/rank6 |
| Generals | — | — | 1441.2/rank17→无变化 |
| LOTA | — | — | 2147.4/rank22→无变化 |

两个结论直接支撑核心主张：(1) dense replay 三盘全胜 binary——决策级证据让 agent 能定位具体错误；(2) 3 倍预算（384/48）在 4 个卡壳游戏上 0 登顶、2 个无变化——瓶颈不是交互量而是博弈理解，单纯堆尝试次数换不来突破。off-policy 回放补充：只看别人对局（128 次回放）Pacman 也能 rank10→1，Miracle 上 off-policy(15) 反超 on-policy(26)——他人对局提供自身分布之外的策略样本。Token 效率差异超 5 倍（GPT6-sol 260M 拿 4 金 vs Qwen3.8 1454M 拿 1 金），金牌数与 token 不成正比。

轨迹模式分析（§4.2）把 12 个 GLM-5.3 run 分为三类：Success（4 个游戏登顶并提前停）、Progress（5 个波动上行）、Stagnation（3 个长期平台）。案例 Dorado 最能体现学习引擎的工作方式：从 rank41 爬到 rank2，最大单步增益来自集中升级两核心英雄（rank14→3，+147.7 Elo）——这是从回放中定位到「英雄强度分配」这一关键杠杆后的针对性修订。

冠军策略的算法族构成（Appendix E）同样有信息量：84 个 retained champions 中规则+启发式打分平均占 79%，图搜索 13%，前瞻搜索（minimax/MCTS）仅在 7/84 超过 10%——agent 们自发收敛到「快而可编辑」的策略形态，而非「深而慢」的搜索形态，这与 HL「保持可解释可编辑」的原则设计一致。

## 5.3 RoboRSI：验证门控的直接证据

四大基准全 SOTA（统一 GPT-5.6-SOL backbone）：LIBERO 56.0%（超 OpenETA 5.3 pp）、LIBERO-PRO 49.5%（**+11.0 pp**，其中 Object 扰动 +16.5）、LIBERO-Plus 42.1%（+5.7）、RoboTwin 24.0%（+2.7）。1,744 评估 episodes + 2,770 消融 episodes + 真机 104 轮。

消融链条完整指向每个机制：

- **失败构成**：OpenETA/Maestro 在 LIBERO-PRO 的 66%/64% 失败是「提前宣布成功」（信任工具返回值），RoboRSI 仅 10%/14%——新鲜观测核验后置条件是设计内机制，直接解释成功率差。
- **角色分离**：单 agent（Engineer 一体）RoboTwin 9/50 vs 多智能体 36/50——独立审查让失败被独立复核而非执行者自判。
- **TSR vs 平铺重写**：同一失败任务，TSR 归因 2 个节点、改 3 文件 50 行、2/2 seeds 通过并发布；flat 候选改 104/63 行，且在另一 seed 上「假成功」（对象未在目标上却宣布成功）被拒发布——精准归因让「改哪里」和「验证什么」都正确。
- **复合技能固化**：600 匹配 episode 对，Code on 29.0% vs off 21.5%（+7.5 pp，McNemar p<10⁻⁴），同时 token −29.4%、调用 −27.2%——固化验证过的调用顺序，错误随步数累积的机会被消除。
- **一日自迭代**：无人类纠偏自迭代 5 轮内，LIBERO 120 任务从 32 解增至 71、LIBERO-PRO 从 43 增至 80，5 轮共 7.0 小时主动执行——自进化速度可量化。
- **分扰动细看短板**：LIBERO-Plus 上 background +13、language +12 领先，但 layout −2、camera −8 落后 OpenETA——感知层面的扰动（相机视角）仍是技能库路线的弱项，诚实呈现了边界。
- **真机**：104 轮开发，换场景零代码修改，5/6 技能发布；grasp_object 单任务开发后被 42 次其他任务成功调用——复用是真实发生的。

# 六、效果优势的根源解释

## 6.1 根源机制与证据链

三篇的因果链可以统一表述为：

**baseline 的根本局限**：独立采样/整体重写/信任工具返回，导致每次尝试的中间认知（探索发现、决策错误、失败位置）在尝试结束时被丢弃，且修改的影响面无界。

**三篇的根本性改变**：把中间认知变成持久的、有结构的、可验证的对象——信息的流动方向从「episode 内蒸发」改为「跨 episode 复利」。

分系统看机制变化（每步标注证据等级）：

- GitSwarm：原子 commit + informed_by → 中间工件存活且跨分支可引用 → 94.7% 贡献被复用、70%+ 负结果被引用【论文实验已支持】。合成对照实验进一步分离三原则：Hidden Rulebook 场景中共享持久记忆的高相关记录覆盖率远超预算更大的独立基线（约 70-75% vs 20-23%，k=4）【论文实验已支持】。但因果归因的干预实验（同候选、有/无前史、等剩余算力）论文自己承认未做【论文自述局限，p11-12】——复用率证明「用了」，尚未完全证明「用了因此变好」。
- AAArena：dense replay → 反馈从结果级（win/lose）变成决策级（哪步错了）→ 修订有靶点。三盘消融全胜 + off-policy 反超实例【论文实验已支持】。复杂度天花板说明该机制的适用边界：规则理解链条太长时，再密的信号也不够【论文实验已支持】。
- RoboRSI：最早失败节点归因 + S_t 限定 + 三重验证 → 「改哪里」「验证什么」「影响面多大」三件事同时被约束 → flat 基线的 1/2 假成功被挡在发布前【论文对照实验已支持】。

**反事实推理**：如果去掉 GitSwarm 的持久性（退化为独立采样），Hidden Rulebook 对照显示覆盖率掉到 20-23%；如果去掉 RoboRSI 的 TSR 归因（退化为平铺重写），同一失败任务从 3 文件 50 行精准修复膨胀为 104 行重写且引入假成功；如果去掉 AAArena 的 dense replay（退化为 binary），Pacman 从 rank1 掉到 rank11。三个反事实共同反证：持久化、归因、信号密度各自必要【论文实验已支持】。

## 6.2 相关工作检索与对照

| 研究 | 相似尝试 | 相关结论 | 与本文差异 | 对根源解释的影响 |
|---|---|---|---|---|
| [Voyager](https://arxiv.org/abs/2305.16291)（NVIDIA+Caltech, 2023） | 技能库存为可执行代码，零样本迁移 100% | 移除技能库物品发现 −46%，技能库是核心资产 | 平面追加无分支、无验证门控、单 agent | 支持持久化论点；GitSwarm 将其推广到多 worker 竞争分支 |
| [AlphaEvolve](https://deepmind.google/discover/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/)（DeepMind, 2025） | 进化搜索改代码，50 数学问题 20% 超 SOTA | 可执行验证的进化循环可产出研究级发现 | MAP-elites 单种群、评估器级联验证、无语义依赖图 | 支持验证驱动进化；GitSwarm 增加跨分支归因与去中心化读出 |
| [AgentGit](http://xiaoweiz.github.io/publication/agentgit/)（2025） | MAS 工作流的 commit/revert/branch | 版本控制降低冗余计算、支持并行探索 | 定位基础设施层，LangGraph 状态而非科研贡献 | 方法相似：版本控制原语有效；结论层面未测复用率 |
| [Useful Memories Become Faulty](https://arxiv.org/abs/2605.12978)（2026） | LLM 持续改写 consolidate 记忆 | **相反结论**：记忆效用先升后降、可跌破无记忆基线；GPT-5.4 从 ground-truth 整合后 54% 原本能解的 ARC 题反而失败；episodic-only 对照与 consolidator 持平 | 任务设定为文本记忆库持续重写，非原子 commit/技能分支 | **限定**：复利不自动成立——持续重写式整合是退化源；侧面支持 GitSwarm 不可变原子 commit 与 RoboRSI 空 patch 门禁的设计（不覆盖证据、拒绝未验证修改） |
| [OWASP Agentic Top 10 / 记忆投毒分析](https://redis.io/en/blog/context-poisoning-agent-reasoning)（2025-2026） | 记忆投毒/上下文污染安全性分析 | 错误一旦入库被复用，爆炸半径从单次请求扩到全历史 | 安全视角，非性能评测 | 限定：持久化放大错误传播；支持 attribution（GitSwarm）与 review 门控（RoboRSI）作为必要防线 |
| [Snell et al.](https://arxiv.org/abs/2408.03314)（2024） | 推理时计算最优分配 | 困难问题上搜索式扩展优于 BoN | 无跨尝试记忆 | 支持「独立采样有天花板」的前提假设 |
| [ASPIRE](https://arxiv.org/abs/2607.00272) / [ENPIRE](https://arxiv.org/abs/2606.08321)（NVIDIA 系, 2026） | 技能库蒸馏 / 真机策略代码闭环 | 技能库带来零样本泛化（31% vs 4%）；真机 99% 成功率 | RoboRSI 与之协议不同，无法直接对比 | 支持 skill-library 路线整体有效性；RoboRSI 补上版本工程层 |
| [Heuristic Learning（翁家翌博客）](https://wengjiayi.com)（2026.5） | Codex 改规则代码 Atari 满分 | 维护成本曲线改变使规则系统重获竞争力 | 个人博客实验、非同行评审 | 支持 AAArena 的 AHL 形式化前提；结论相近但证据等级低于论文 |

## 6.3 综合判断与未决问题

**多研究共同支持的机制**：(1) 可执行/结构化的经验持久化优于自然语言记忆（Voyager、ASPIRE、GitSwarm、RoboRSI 四线一致）；(2) 验证门控挡住假成功（RoboRSI 对照实验、AlphaEvolve 评估级联、Voyager 自我验证消融 −73%）；(3) 决策级反馈优于结果级反馈（AAArena 三盘消融，与 PRM 引导搜索文献一致）。

**仍属推测的机制**：GitSwarm 的跨分支 informed_by 本身带来多少增益——论文自述需干预实验隔离，informed_by 是 worker 声明而非独立因果确认；AAArena 的冠军策略算法族与 Elo 的 Spearman 0.35 不构成因果；RoboRSI 的串行累积协议在并行部署下的行为未见检验【阅读者推测：并行多机器人同时修订同一技能库时，S_t 局部性上界是否仍成立，需要类似 GitSwarm 的分支合并语义，这是两篇工作天然的结合点】。

**优势成立条件与失效条件**：复利在任务可验证、失败有信息量（AAArena 卡壳游戏显示信号再密也受理解上限约束）、修改可归因时成立；在持续重写记忆（2605.12978 的场景）、不可验证领域（Voyager 作者自己指出 sandbox 是被低估的礼物）、或对手分布错配（top-four 恒最差）时退化甚至反噬。GitSwarm 的成本问题同样真实：仓库增长使检查历史消耗输入 token，B=500 时增益递减。

**未决问题的优先级排序**（观察者视角）：最高优先是 GitSwarm 自述的因果归因干预实验——它决定「复利」是真实机制还是相关性叙事；其次是跨系统验证——GitSwarm 的贡献图与 RoboRSI 的技能树本质是同构结构，一个在数字域、一个在物理域，谁的机制能迁移到对方域尚未有人测试；最后是 AAArena 指出的 HL/RL 混合学习器——策略结构靠 HL 改、感知组件靠 RL 训，两条路线的接口标准化是开放问题。

# 七、必要知识反推

假设让一个空白智人从零做出这三篇论文，最少需要什么？

**领域知识层**：
- 软件工程实践：Git 分支模型、code review、CI/CD、回归测试——GitSwarm 的全部设计原语与 RoboRSI 的验证发布都直接来自这里。不理解「为什么人类团队用分支而不是互相覆盖」，就设计不出 branchability 原则。
- 博弈论与 Elo/Bradley-Terry 评级：AAArena 的 Elo 估计需防无穷（加虚拟平局）、retained-champion 计分需理解评级噪声。
- 机器人学：移动操作栈、RGB-D 感知、sim2real gap——RoboRSI 需知道物理失败常源于观测前几步而非观测点。

**方法论知识层**：
- 推理时计算谱系（BoN/自洽/树搜索/PRM）：知道独立采样的天花板，才能定位「跨尝试记忆」这个空位。
- 经验学习谱系（Reflexion→ExpeL→Voyager→HL）：AAArena 的 AHL 形式化直接建立在这个脉络上。
- 多智能体系统与去中心化协议：GitSwarm 的提名投票、异步协议、平票裁决需要分布式系统知识。
- 消融与因果推断方法论：三篇都重度依赖对照设计（flat vs TSR、dense vs binary、async vs sync）；GitSwarm 作者对因果归因缺失的自我批评说明作者必须理解干预实验与观察性证据的区别。

**工程知识层**：
- 大规模 GPU 编排（RMT 任务 8×H200 节点、NanoChat 用 GB300）与计费 episode 预算控制。
- Agent harness 开发：系统提示词设计（GitSwarm 的 p54-58 系统提示词）、工具接口契约、沙盒隔离。
- 基准构建：从真实竞赛平台（AI9/Saiblo）接入 12 游戏与 1,920 个人类程序；LIBERO 系列四基准的协议设计。

**知识融合的关键节点**：
1. GitSwarm 的核心洞察 = 「开源协作实践」×「推理时计算」：不是让 agent 更聪明，而是给它们人类的协作基础设施。融合要求作者同时深入两个世界——一边是 Git 与开源社区的工程细节，一边是 BoN/搜索式推理的理论局限。
2. AAArena 的核心洞察 = 「HL 假设」×「竞赛平台基础设施」：清华计算机系学生科协办过的真实 Agent 竞赛（AI9/Saiblo 平台），直接变成了可控实验环境——1,920 个存档人类程序是任何实验室都无法从零标注的资产。这是学术问题与社区资源的化学反应。
3. RoboRSI 的核心洞察 = 「版本局部性理论」（Eq.3 影响面上界）×「机器人失败归因」：把软件工程的 blame/影响分析变成物理世界的修复协议。做出这个融合的前提是同时理解：代码修改的影响传播规律（软）与物理失败的观测滞后规律（硬）——两者结合才知道为什么必须归因到「最早失败节点」而非「观测到失败的节点」。

# 八、论文中可以提取的通用性灵感

**灵感一：失败是一等公民资产，不是垃圾**。GitSwarm 70%+ 负结果被引用、NanoChat 的失败 hash 21 小时后复活成组件、ASPIRE 从失败轨迹蒸馏技能。推广场景：科研管理（负结果登记与引用制度）、医院不良事件库、事故复盘工业标准、个人知识管理（错题本结构化）、投资决策日志（记录被证伪的假设供后续复盘）。

**灵感二：归因到源头，修复才有界**。RoboRSI 的最早失败节点 + 子层级限定（改 3 文件 50 行 vs flat 重写 104 行还引入假成功）；物理失败常源于观测前几步。推广场景：系统运维（根因分析 vs 表层重启）、组织诊断（流程哪一环坏 vs 全员再培训）、医疗（致病位点 vs 全身调理）、教育（概念误解的最早形成点）。

**灵感三：信号密度决定学习速度**。AAArena dense replay 对 binary 的全面优势；off-policy 回放破 on-policy 平台期。推广场景：代码 review（逐行评论 vs LGTM）、教练反馈（动作级录像复盘 vs 只报比分）、A/B 测试（过程指标 vs 只有转化率）、客服质检（对话级标注 vs 满意度分）。

**灵感四：发布需要门禁，回退需要保底**。RoboRSI 验证三件套 + retained-champion + Git 分支——探索性修改失败不丢已有增益。推广场景：灰度发布与 feature flag、宪法修正案程序、药物审批分期、个人决策（试新方法时保留旧流程直至新法验证）。

**灵感五：读写分离的「选择性检索」优于全量转录**。GitSwarm worker 只开 5-7 个 commit 而非读全图；2605.12978 证明持续全量重写记忆反而退化。推广场景：组织知识库（按需检索 vs 全员通读手册）、上下文工程（检索增强 vs 无限长上下文）、教育（问题驱动学习 vs 百科通读）。

**灵感六：预算堆不出理解，结构才能**。AAArena 3 倍预算 0 登顶 vs RoboRSI 结构化归因一次修复；GitSwarm B=500 增益递减。这是对「Scaling 万能论」的精确反例集：算力买到的是尝试次数，尝试之间的组织结构决定尝试能否复利。推广场景：题海战术 vs 错题归因、堆人力的项目 vs 改了流程的项目、粗读百篇论文 vs 精读十篇并建联系。

**灵感七：去中心化的提名比中心裁判更鲁棒——但有代价**。GitSwarm 去中心化提名 62.06% vs central aggregator 58.19%（后者反而更差），但 central selector 确实再高 2.34pp；异步比同步快 1.30× 仅损 1.05pp。启示：去中心化不是意识形态而是工程权衡，在「中央聚合器会过早收敛」的场景下分散提名是结构优势，在「选择器足够聪明」的场景中心化仍占优。推广场景：组织决策（委员会 vs 强领导）、推荐系统（大众评分 vs 编辑精选）、科学资助（同行提名 vs 主任拍板）。

# 九、局限与观察者评论

**GitSwarm 的最大软肋是因果归因**：94.7% 复用率证明 worker「用了」历史，但「用了因此更好」需要同候选有/无前史的干预实验——作者自己诚实指出（p11-12）。此外 IMOProofBench 满分是单次运行、ProgramBench 只测 200 任务中固定 50 题、部分 BoN 基线同达上限，评估条件不完全一致。仓库增长的输入 token 成本与 B=500 的增益递减提示：复利也有利息成本。

**AAArena 的金牌定义需精确理解**：rank-1 是对冻结人类池的排名，不等于击败人类冠军；全池评估在开发期可见，故不衡量对未见对手的泛化；7 配置混合了模型与 harness 差异。最有价值的发现反而是否定性的——复杂度天花板与预算无效性，为整个 HL 路线画出了当前边界。

**RoboRSI 的协议是自定的**：技能库串行累积协议下 ASPIRE/ENPIRE 无法同协议对比；真机 104 轮是开发轨迹而非冻结策略的重复试验，Figure 4 的 10 个 run 是事后挑选；camera/layout 扰动上仍低于 OpenETA（−8/−2 pp）；53 个物理修订中 4 个需人类知识介入。它与 GitSwarm 共享一个未解问题：技能/贡献图变大后的检索与冲突，论文只给了初步答案。

**三篇合读的整体观察**：2026 年 10 月这个时间点，「无权重经验复利」正在从博客级假说（HL）走向系统级实证。GitSwarm 证明了复利在 10² 次尝试尺度上真实发生（复用率、负结果复活、跨分支合成三重证据），AAArena 证明了信号密度是复利的燃料上限，RoboRSI 证明了验证门控是复利在物理世界不翻车的刹车。三者拼起来才是一台完整的「经验复利机」：持久账本 + 密集信号 + 版本安全。缺的那块拼图三篇都指向但都没做完：GitSwarm 结论里说的「用 RL 训练 agent 学会做被后续工作引用的贡献」——即让模型本身内化复利行为，从「环境支持复利」进化到「agent 主动追求复利」。也许明年这时候，这个清单上会多一篇把三件事端到端缝合的工作。

# 附录：速查卡

**一句话记忆三篇**：

- GitSwarm：给一群异步 agent 一个共享 Git 仓库，94.7% 的贡献会被后来者复用，70% 的失败会变成证据——中间工作不再蒸发。
- AAArena：12 个竞赛游戏、1,920 个人类程序的天梯上，看得见每步决策的回放比只看得见胜负多拿 500+ Elo，但 3 倍预算也攻不下规则最复杂的 6 个游戏——信号密度有上限，理解没有捷径。
- RoboRSI：机器人每次失败归因到技能树的最早失败分支，改 50 行而不是重写 104 行，验证三件套（review+测试+回放）通过才发布——自进化第一次有了工程意义上的「刹车」。

**关键数字速查**：

| 数字 | 含义 | 出处 |
|---|---|---|
| 79.4% vs 65.1% | ProgramBench GitSwarm 对 Forced Codex | GitSwarm p1/p6 |
| 94.7% | 贡献被后续构建引用率 | GitSwarm p8 |
| 81.6-92.6% | 选中解 ancestry 覆盖贡献图 | GitSwarm p8-9 |
| 6 金 / 12 游戏 | AAArena 最好成绩（Opus5.5） | AAArena p5 |
| 5/6 vs 1/6 | 低/高复杂度游戏登顶数 | AAArena p6 |
| +11.0 pp | RoboRSI 对 OpenETA（LIBERO-PRO） | RoboRSI p12 |
| 66% vs 10% | baseline vs RoboRSI 提前宣布完成率 | RoboRSI p13 |
| 36/50 vs 9/50 | 多智能体 vs 单 agent（RoboTwin） | RoboRSI p15-16 |

---

> **三篇论文的 arXiv 编号速查**：GitSwarm 2610.04862（62 页）/ AAArena 2610.12341（26 页）/ RoboRSI 2610.12424（32 页）。文中所有数字均标注论文页码，详见各节引用。
