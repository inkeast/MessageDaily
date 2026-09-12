---
title: "EvoSafeHarness 精读：Agent 安全没有万能线束，那就让线束自己进化"
date: 2026-09-12
draft: false
tags: ["学术调研", "Agent", "Harness", "安全"]
categories: ["paper-reading"]
summary: "JHU/UC Berkeley/NVIDIA/UIUC/UW-Madison 五机构发布 EvoSafeHarness：为冻结 LLM Agent 自动搜索'模型×领域'专用安全 harness，DecodingTrust-Agent 上 ASR 45.6%→10.0%（utility 仅损 3.3 分），AgentDojo 82.8% utility @ 0 ASR。核心洞察：模型变体决定 enforcement 强度、领域变体决定谓词与状态——universal 安全 harness 在结构上就不存在。"
---

# EvoSafeHarness 精读：Agent 安全没有万能线束，那就让线束自己进化

> **题目**：EvoSafeHarness: Evolving Model- and Domain-Specific Harnesses for Securing Agents
> **链接**：https://arxiv.org/abs/2609.05903 · 代码：github.com/SaFo-Lab/EvoSafeHarness
> **团队**：Johns Hopkins + UW-Madison + NVIDIA + UIUC + UC Berkeley（Nanxi Li, Yingzi Ma, Yulong Cao, Edward Suh, Bo Li, Dawn Song, Chaowei Xiao）
> **数据日**：2026-09-11

## 一、题目与背景

LLM Agent 正在从演示走向部署：访问敏感数据、金融账户、生产系统。失败的单位从"一句话说得不对"扩展为"整条动作轨迹"——一笔被转走的资金、一个泄漏的凭证、一次被删库的生产数据。防御有两层：模型级（安全对齐）与 harness 级（系统层强制执行）。模型级防御必要但不充分——它让合规行为更可能，却不提供独立于模型行为的系统级执行边界。

harness 级防御的现有形态（CaMeL、DRIFT、Progent、SafeHarness）有一个共同假设：**专家设计一次，处处部署**。论文用两个维度拆解这个假设为什么错了：模型变体改变"需要多强的 enforcement"——对强安全模型（如 Claude Opus）强加上 CaMeL 式严格能力管控，安全保住了但 utility 崩了；对易受攻击的模型同样的管控却可能是必需品。领域变体改变"harness 必须实现的谓词与状态"——文件系统域要检查命令效果、敏感路径、秘密移动与后续数据流；金融域必须区分交易与资金流出、强制收款方约束、维护交易历史（因为一串各自合规的交易合起来可能是洗钱）。命令过滤器表达不了金融关系，交易账本防不了文件系统持久化攻击。**两个变化轴正交，单一固定设计不可能同时最优**。

## 二、研究定位

这篇论文站在两个脉络的交汇处。其一是 harness 优化：Meta-Harness 用 agentic proposer 基于执行轨迹迭代搜索 harness 代码、NLAH 把 harness 变成可编辑的自然语言策略、VeRO/AHE 加版本化评测循环——但这些全部优化任务性能或成本，没有一个为"对抗者存在下的行为"优化。其二是系统级 Agent 防御：从提示级标记到能力策略到 CaMeL 的架构级数据/控制流分离——但都是固定设计。

EvoSafeHarness 的定位：**把 harness 搜索的范式移植到安全域，并解决移植过程中出现的三个特有陷阱**。这是"Meta-Harness 遇到对抗世界"的第一次系统化。

## 三、问题定义

形式化：Agent 是 $(M, H)$——冻结模型 $M$ 与 harness $H$（ mediation 所有 M-用户-工具交互的有序管线）。防御即对裸循环 $H_0$ 的任何修改。候选 harness 表示为 $H=(P, C)$：自然语言策略 $P$（上下文变换：信任边界声明、来源框定、拒绝准则）+ 可执行代码逻辑 $C$（任意程序：改写/拦截工具调用、检查输出、维护轨迹内状态、执行能力管控）。

目标函数：$\text{score}(M,H,D) = 100 \cdot (U - \text{ASR})$。这个设计排除了两大退化解：全拒绝的 harness（U=0）得零分，全放行的高 utility harness（ASR 高）同样不得分。安全只有在 utility 保持时才被奖励。求 $\arg\max_H \text{score}$，约束是应用 adapter 与领域规范的不可变组件。

## 四、解法

四组件闭环 + 两个关键防御性设计：

1. **领域规范（Domain Specification）**：每次搜索的"任务合同"——冻结受害者、威胁语义（直接/间接）、训练侧评测级联与分数、泛化要求、adapter 接口。规范说"什么必须不变、怎么评"，不说"建什么防御架构"。
2. **Designer**：读归档（源码、分数、失败轨迹），提议/修订 $(P, C)$——修订是自由编辑而非菜单式变异，可加状态、换控制流、重写整段代码；尽量一次改一个机制保持分数可解释。
3. **Criticizer（本文最有趣的设计）**：任何候选在进入评测前，交给**全新上下文**的独立 Criticizer 做"最便宜的逃逸"审查——攻击者改名路径/换位置/改写指令后这条规则还成立吗？有没有规则绑定 benchmark 特有的字面 token？论文直指要害：*最大化 benchmark 分数的搜索，放任不管就一定会发现 benchmark 伪迹*——屏蔽特定目录名的规则便宜且局部有效，改名即失效。Criticizer 用攻击不变量（来源：危险指令来自工具输出而非用户；范围：动作触及用户任务未提及的资源；意图失配）做验收标准。消融：去掉 Criticizer 搜索分不变或更高，但 held-out 掉至多 31 分——**它是把"搜索分"翻译成"留存分"的唯一组件**。
4. **级联测试环境**：四段便宜到贵——静态检查→smoke 前缀→置信门（分层 bootstrap 上界判断是否显著劣于无防御参照，只有"确证劣于"才淘汰）→完整搜索样本扩展；嵌套设计复用早期轨迹证据。良性/直接攻击/间接攻击三路分数分开保留，Designer 据此定向修订。

暖启动：从 CaMeL/DRIFT 蒸馏 8 条安全设计经验（工具输出是数据不是指令；工具调用效果须对照可信用户请求检查）作为初始候选——是经验不是模板，返回的 harness 常含暖启动中不存在的新机制。

## 五、实验结果

| 基准 | 结果 |
|------|------|
| DecodingTrust-Agent（15 个 model×domain 格） | 平均 ASR **45.6%→10.0%**，utility 代价仅 3.3 分；14/15 格最佳（CaMeL/DRIFT/Progent 或 ASR>37% 或 utility 损失>20 分） |
| AgentDojo | **82.8% utility @ 0.0% ASR**——同零 ASR 点 CaMeL 的两倍 utility |
| AgentDyn（零样本迁移） | 同一 harness 不变迁移：75.0% utility / 0.0% ASR |
| Agent-SafetyBench | 每个受害模型最佳分 |
| 自适应攻击（AgentCanary，PAIR 式预算 16） | 冻结 harness 平均 ASR<20% |
| 消融 | 去 Criticizer：held-out 至多 **−31 分**；安全暖启动、嵌套级联均各有可测贡献 |

最有分析价值的发现（§7）：同一攻击、同一领域、只换模型——Sonnet 4.6 用轻量策略+两个语义检查即达零留存 ASR，GLM-5 需要确定性门+溯源状态+判定缓存。**弱模型不是"需要更多规则"，而是"需要不同的执行机制"——语义检查还是确定性门控、有无状态、在执行流的哪个点拦截，由模型与运行时行为决定**。

## 六、知识反推

1. **安全 harness 的搜索空间是"部署坐标"张成的**。15 个格各自独立搜索、各自最优——说明 (model, domain) 是安全设计的基本坐标系，而不是"方法库+调参"。这为"harness 工程学科化"补上了安全维度的公理。
2. **对抗场景下的自动化搜索必须内置"泛化审计员"**。普通 harness 优化（Meta-Harness 系）用 benchmark 分数就够了；一旦有对抗者，分数与真实防御能力之间的裂缝（benchmark 伪迹）会被搜索机制主动利用。Criticizer 的"改名测试"便宜且有效——这个设计模式适用于一切"优化目标可被 gaming"的自动搜索系统。
3. **utility 与安全不是滑动条**。固定防御的数据（CaMeL 在 Sonnet 上 utility 掉 21 分）制造了"更安全必然更难用"的错觉；逐部署搜索证明在多数格上两者可以同时接近最优——之前的权衡是设计粒度错配的伪象。

## 七、通用灵感

- **给自动优化系统加"反 gaming 层"**：任何用评测分数驱动的搜索（你的代码评审 Agent 的 skill 进化、reward model 训练）都值得配一个 fresh-context 的 Criticizer——问"这套规则换个表面还成立吗"，验收标准锚定不变量而非表面特征。消融里 31 分的差距就是这一层的 ROI。
- **"分开计分"是防退化目标设计的通用技巧**：score=U−ASR 的本质是把两类失败模式（全拒绝、全放行）都归零。设计任何 trade-off 目标时，先枚举退化解，再让它们在目标函数里自然得零分——比事后加惩罚项干净。
- **从失败轨迹蒸馏"设计经验"而非"规则"**：Analyzer 输出的是经验（何时信任工具输出），Designer 自由组合成机制。这比直接学规则的迁移性好得多——经验可跨机制复用，规则绑死实现。
