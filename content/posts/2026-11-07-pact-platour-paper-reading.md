---
title: "On the Opportunities and Challenges of Prefill-to-Autoregressive Cross-Model Transfer 精读"
date: 2026-11-07
draft: false
tags: ["论文精读", "模型推理", "学术调研", "评测", "AI趋势"]
categories: ["paper-reading"]
summary: "KCL+腾讯优图+英国国家物理实验室研究 PACT（Prefill–to–Autoregressive Cross-model Transfer）：大模型只做 prefill、把内部表示经桥接传给小模型独立生成，兼顾能力与解码成本。论文开源 PLatour 框架——零初始化线性投影桥、GRPO 任务奖励直接训练（不经 SFT 对齐/蒸馏）、传 hidden states 而非 KV cache 以支持混合注意力架构。生成任务上 C2C/KV-Lingo 均低于 Receiver Alone 而 PLatour 全基准提升：Qwen3.5-4B 接收者配 35B sender，AIME-80 59.22→75.63、HumanEval 83.16→94.97、MBPP 65.08→89.93，超过 Mostik 用 753B sender 报告的结果；397B 变体因训练中 clipping 尖峰反低于 35B。更重要的贡献是 PACT-Check：Dummy/Irrelevant Transfer、Self-Communication、Trained Receiver 四对照揭示部分任务（GSM8K PACT-Score 仅 +0.23、MATH500 为负）的增益可由接收侧自适应复现——可训练桥本质上是参数高效适配器，基准分数不能证明真实信息转移。"
---

# On the Opportunities and Challenges of Prefill-to-Autoregressive Cross-Model Transfer —— 精读

> **论文链接**：[On the Opportunities and Challenges of Prefill-to-Autoregressive Cross-Model Transfer](https://arxiv.org/abs/2611.06976)
> **代码仓库**：[github.com/zhenyi4/platour](https://github.com/zhenyi4/platour)
> **发表时间**：2026年11月
> **机构**：King's College London（Zhenyi Shen、Junru Lu 等）、Tencent Youtu Lab（腾讯优图）、National Physical Laboratory（英国国家物理实验室）
> **领域标签**：cs.CL / 潜空间通信 / 模型推理效率

---

## 一、论文背景

### 1.1 大小模型协作的经济学

大模型能力强但解码贵（每个生成 token 都要过一遍大网络），小模型便宜但应付不了复杂任务与长指令。**PACT（Prefill–to–Autoregressive Cross-model Transfer）** 是潜空间通信（latent communication）家族中专为这个权衡设计的一支：**大模型 sender 只处理 prompt 的 prefill 阶段**，把内部表示传给**小模型 receiver**，receiver 随后独立自回归生成全部响应。

这个分工的妙处在于成本结构：prefill 是一次性开销（随 prompt 长度线性），解码才是逐 token 的持续开销（随生成长度增长）——把贵的解码交给小模型，能力瓶颈（理解问题）留给大模型。且与现有 prefill-decode 分离式服务基础设施（Splitwise、DistServe）天然兼容。闭源系统 Mostik 曾报告关闭 753B sender 与 4B receiver 之间 47% 的性能差距，展示了该方向的上限。

### 1.2 研究动机的两个缺口

1. **系统缺口**：Mostik 闭源无法复现；已有开源方法（C2C、KV-Lingo 等）要么只在选择题式任务上有效、要么无法支撑长生成、大容量差距（>100:1 参数比）与混合注意力架构——PACT 的潜力边界无人能系统探明。
2. **归因缺口（本文最有价值的洞察）**：**任务性能提升本身不能证明 receiver 受益于 sender 的信息**。桥接模块是可训练参数——它可能只是充当了参数高效适配器（adapter/prefix），把 receiver 微调得更好，而 sender 沦为「昂贵的噪声源」。此前没有人用控制实验区分「真信息转移」与「接收侧自适应」。

### 1.3 概念铺垫：为什么模型间能「说潜空间语言」

- **Platonic Representation Hypothesis（Huh et al. 2024）**：独立训练的模型（越强）越可能收敛到共享的表示几何——这为跨模型线性翻译提供了理论动机。
- **OPRD（Yang et al. 2026）**：模型 hidden states 投影到共享低秩空间后余弦相似度很高。
- **模型缝 合（model stitching）**：relative representations（Moschella et al. 2023）等方法证明零样本缝合可行。
- 但作者清醒地指出：**结构相似 ≠ 有效信息转移**——sender 必须以 receiver 能利用的形式提供任务相关信息；这一点只能经验性检验（这为后文 PACT-Check 埋下伏笔）。

---

## 二、论文定位和关联工作

### 2.1 潜空间通信谱系

| 方法 | 传输介质 | 机制 | 与 PACT/PLatour 的关系 |
|---|---|---|---|
| Interlat（Du et al. 2026） | 压缩 hidden-state 消息 | 多 agent 生成+压缩消息 | 通用 agent 通信 |
| LatentMAS（Zou et al. 2026） | KV 记忆 | 自回归潜推理+记忆迁移 | 多 agent 系统 |
| KVComm（Ye et al. 2025） | KV cache | 跨 agent 上下文复用、减少重复 prefill | 同构上下文复用 |
| C2C（Fu et al. 2026, ICLR） | KV cache | 任务监督的 KV 融合（门控残差加到 receiver cache） | 主要开源对照；SFT 训练 |
| Latent cache flow（Rossi et al. 2026） | 低维瓶颈 | C2C 轻量化 | |
| KV 对齐类（Dery et al. 2026 等） | 共享 KV 空间 | 学跨模型共享空间 | |
| KV-Lingo（Castin et al. 2026） | KV cache | 闭式线性回归初始化 + KL 精炼；**替换** receiver 的 KV | 生成任务上崩坏的主要对照 |
| KV 迁移类（Heo/Qu/Qiu/Li/Yun 等 2026） | KV cache | 模型切换免重 prefill | **以对 receiver 自身 prefill 的保真度评估**——目标与 PACT 不同 |
| Mostik（2026，闭源） | 潜表示 | 桥接（细节未公开） | 报告值的对照（753B→4B） |
| **PACT/PLatour（本文）** | **hidden states** | 零初始化线性残差注入 + GRPO | sender 只 prefill、receiver 独立生成 |

### 2.2 归因审计谱系（PACT-Check 的前驱）

Zhang & Emu 2026 与 Cheng et al. 2026 通过**推理时干预**（参数冻结）审计潜通道是否真的在通信。PACT-Check 的差异化：面向**带可训练桥**的 PACT 系统，在推理时干预之外补充**单独训练的对照组**（receiver-only / self-communication），从而区分「信息依赖」与「可被自适应复现的增益」。

### 2.3 PEFT 谱系（理解「假通信」的理论透镜）

- Prefix tuning（Li & Liang 2021）：学习连续前缀激活；
- Adapter（Houlsby et al. 2019）：hidden states 的残差变换；
- He et al. 2022 的统一视角：adapter/prefix/LoRA 都是「学到的 hidden-state 修改」的不同形式。

论文的关键连接：**把 sender 表示换成与问题无关的固定信号时，Wℓ·nℓ,t 就是学到的 prefix；self-communication 时桥退化为 (I+aW)h 的线性残差 adapter**——这解释了为什么没有 sender 信息也能涨分。这个「通信-适配连续谱」是全文最深刻的理论视角。

### 2.4 定位总结

| 维度 | KV 融合（C2C） | KV 翻译（KV-Lingo） | 闭源（Mostik） | PLatour |
|---|---|---|---|---|
| 传输介质 | KV cache | KV cache | 潜表示 | **hidden states** |
| 训练目标 | SFT | 线性回归+KL | 未公开 | **GRPO 任务奖励直接训练** |
| 长生成 | ✗（低于 receiver） | ✗（严重崩坏） | ✓ | ✓（16K 生成预算） |
| 大容量差距 | 部分 | ✗ | ✓（753B:4B） | ✓（>100:1） |
| 混合注意力 | ✗（KV 绑定全注意力） | ✗ | ？ | ✓（隐藏态天然兼容线性注意力） |
| 开源 | ✓ | ✓ | ✗ | ✓ |
| 归因审计 | ✗ | ✗ | ✗ | **PACT-Check** |

---

## 三、问题定义

### 3.1 形式化

给定 prompt x，sender prefill 得到 hidden states H^S(x)；可训练桥 B_φ 把它注入 receiver 的初始状态：

z^φ_R(x) = Prefill_R(x; B_φ(H^S(x)))，  p_PACT(y|x;φ) = Π_t p_R(y_t | y_<t, z^φ_R(x))

**目标**：学 φ 使得 E[r(x,y)] 高于 standalone receiver 的 E[r(x,y)]——注意这个不等式只要求「更强」，不要求「用了 sender 的信息」。这正是归因问题的数学根源：**目标函数对信息来源不加约束**。

### 3.2 核心抽象：通信-适配对偶

论文的深层洞察是：同一个桥架构横跨一个连续谱——

| 极限情形 | 桥的形态 | 等价的 PEFT |
|---|---|---|
| sender 表示 → 固定问题无关信号 n | Wℓnℓ,t 是学到的条件信号 | **prefix tuning** |
| sender = receiver 自身（identity 层匹配） | (I+aW)h 的线性残差 | **adapter** |
| 无外部 sender | LoRA / 全参微调 | 常规 PEFT |

因此「PACT 增益」是两个混杂成分之和：**真实跨模型信息转移** + **接收侧自适应**。论文要回答的抽象问题：**给定一个训练好的 PACT 系统，其增益中有多少真正依赖 prefill 阶段传递的信息？** 答案必须逐任务、逐训练目标地经验给出。

### 3.3 附带的结构问题

- 桥该传什么（KV vs hidden states）？怎么训（SFT 对齐/蒸馏 vs 任务奖励）？层怎么配对？如何在混合注意力（线性注意力层无 KV cache 概念）下工作？sender 多大合适（35B vs 397B）？——这些是 PLatour 作为「强基线」要回答的工程问题。

---

## 四、问题解法

### 4.0 全景：PLatour 桥 + GRPO 训练 + PACT-Check 审计

```
Sender(冻结❄) --prefill hidden states--> [零初始化线性投影 Wℓ × 每层]
                                          │ 残差注入（仅 prompt 位置、除最后 token）
                                          ▼
                                     Receiver(冻结❄) --独立解码-->
                                          ▲
                              GRPO 只训练投影（梯度穿过冻结 receiver）
```

### 4.1 组件一：桥架构——极简线性残差注入

对 receiver 每层 ℓ 配一个独立无偏置线性投影 Wℓ∈R^(dR×dS)：

h̃^R_{ℓ,t} = h^R_{ℓ,t} + a_t·Wℓ·h^S_{m(ℓ),t}

其中 a_t=1 对所有 prompt token（除最后一个），生成阶段 a_t=0——**通信严格限于 prefill**。四个设计决策各有论证：

1. **残差加法而非替换**（继承 C2C）：零初始化使训练前 receiver 行为完全不变（与 LoRA 的 B=0 初始化同原理）；直接替换没有这个性质且需要额外对齐阶段。
2. **线性而非 MLP**：简化机制便于分析；消融显示线性 ≥ 两层线性/两层 MLP（88.05 vs 85.40/85.80，MATH500）——共享表示空间假设下线性表达力已够，且更易优化。
3. **传播（propagation）**：注入后的 hidden states 被 receiver 继续处理并向高层传播——低层注入的结果影响高层状态（而非每层独立修改预先算好的状态），保持 receiver 序贯信息流的一致性。消融：开传播 +2.35~2.76pp。
4. **相对深度层匹配**：m0(ℓ)=round(ℓ(LS−1)/(LR−1))，保归一化深度对齐；**混合注意力约束**——full-attention 的 receiver 层必须配 full-attention 的 sender 层（线性注意力层接受两类），直觉是线性注意力局部性偏强，全局上下文信息应从 full-attention 层转移。

### 4.2 组件二：训练目标——直接 GRPO，拒绝对齐与蒸馏

sender/receiver 双冻结，只训桥，GRPO（verl 框架）直接最大化任务奖励，**无前置 SFT 或蒸馏热身**。两个否决论证：

- **否决表示对齐目标**：无约束的双双侧对齐存在平凡解——两侧投影都塌缩到零，达成完美一致却零信息。表示一致本身不保证任务相关信息保留。
- **否决模仿 sender 分布**：大容量差距下小 receiver 模仿大 sender 的输出分布很难甚至有害（与 on-policy 蒸馏的已知困难一致，Li et al. 2026a）。实证：SFT 与 OPD 变体在 MATH500 上都低于裸 receiver（83.55/83.20 vs 84.60），只有 GRPO 稳定超越（88.05）。

**心智模型**：「把 sender 的 hidden states 当作 latent tools，训练桥学会『使用』这些工具去拿高奖励」——receiver 不必成为 sender，只需学会借力。

工程细节：>100B 的 sender 用 FP8 降内存；投影用 FP32 计算保留小更新精度再 cast 回 BF16；实现为 HuggingFace prefill + vLLM 解码的混合 cache 传递；Qwen3.5 训一个 epoch（DAPO 264 步 / DeepCoder 295 步），16,384 token 响应上限，lr 1e-5，截断响应零奖励。

### 4.3 组件三：PACT-Check——四重对照的归因协议

| 对照 | 类型 | 做什么 | 检验什么 |
|---|---|---|---|
| **Dummy Transfer (DT)** | 推理时干预（零额外训练） | 把问题相关 sender 特征换成离线算好的固定模板均值特征（500 训练 prompt 的逐层逐模板位置均值），不加载 sender | 增益中有多少**不需要输入特定** sender 信息 |
| **Irrelevant Transfer (IT)** | 推理时干预 | sender 处理一道**无关题目**（严格匹配渲染后 token 长度、seed 42 确定性供体），receiver 处理原题 | 性能是否依赖** transferred 内容相关性**（大幅下降=对错配敏感，但敏感性≠语义理解） |
| **Self-Communication (SC)** | 单独训练 | receiver 既当 sender 又当 receiver 训投影；退化为线性残差 adapter | 没有**更大 sender** 时桥的自适应容量能到哪 |
| **Trained Receiver (TR)** | 单独训练 | LoRA/全参 × SFT/GRPO 直接微调 receiver | 标准**接收侧微调**这一最强替代方案的高度 |

**PACT-Score = A_PACT − max_c A_c**：对最强对照的准确率裕度。>0 = 超越所有对照，是 sender 信息受益的证据；≤0 = 至少一个对照追平或超过，增益不能唯一归因于有用 sender 信息。

---

## 五、评估指标与实验证据

### 5.1 指标与设置

- **主指标**：avg@k（k 次采样的平均单响应准确率，估计单次能力而非 best-of-k）；贪心任务 k=1。
- **两套实验**：Qwen3 家族（235B/8B→1.7B，全注意力，与 KV 基线兼容）+ Qwen3.5 家族（35B/397B→4B，混合注意力，对标 Mostik）；数学/编码分别训独立桥（DAPO-17k 去污 / DeepCoder-24k 清洗为 18,873 训练）。
- **数据卫生**：AIME-135 中识别 55 题与 DAPO 重叠（53 train + 2 val），报告**去污 AIME-80**（2/4/30/30/14 题/年）；DeepCoder 对三个编码测试集去污；TextComm 消息做答案泄漏筛查（逐条人工核查旗标）。
- **对照参考**：Mostik 报告值仅限 GLM-5.2(753B)→Qwen3.5-4B 冻结配置，作为参考而非受控比较（其训练数据未公开、桥是否分域未知）。

### 5.2 结果一：生成任务——只有 PLatour 稳定超越裸 receiver

Qwen3-235B→1.7B（Receiver Alone：Chem 41.43 / GSM8K 80.86 / MATH500 73.40）：

| 方法 | Chem-CoT | GSM8K | MATH500 | 结论 |
|---|---|---|---|---|
| C2C | 41.01（−0.4） | 75.09（−5.8） | 64.60（−8.8） | 全线低于裸 receiver |
| KV-Lingo | 40.95 | **17.06（−63.8）** | **11.95（−61.5）** | 灾难性崩坏 |
| PLatour-SFT | 57.80 | 85.63 | 70.65（−2.8） | 两升一降 |
| **PLatour (GRPO)** | **69.29（+27.9）** | **89.35（+8.5）** | **78.20（+4.8）** | **全部提升** |

KV-Lingo 崩坏归因：直接 KV 替换在 235B:1.7B 大差距下，线性回归初始化无法充分对齐两个模型的 KV 表征（8B sender 下它反而略好于 C2C）。SFT 在 MATH500 掉分的假设：模仿更长的 sender 轨迹对小 receiver 太难（receiver-only SFT 同样掉）。**答案只有 GRPO 直接奖励最大化。**

### 5.3 结果二：规模化对标 Mostik（Qwen3.5-4B receiver）

| 方法(sender) | AIME-80 | AIME-135 | HMMT | IMO-Ans | 数学均分 | LCB v6 | HumanEval | MBPP | 编码均分 |
|---|---|---|---|---|---|---|---|---|---|
| Receiver-Alone | 59.22 | 59.12 | 46.35 | 41.13 | 48.90 | 40.36 | 83.16 | 65.08 | 62.87 |
| PLatour (397B) | 71.88 | 72.73 | 61.77 | 49.25 | 60.97 | 52.50 | 93.67 | 85.43 | 77.20 |
| **PLatour (35B)** | **75.63** | **74.63** | **64.58** | **49.75** | **63.32** | 54.29 | **94.97** | **89.93** | **79.73** |
| Mostik (753B)* | – | 70.74 | – | – | – | 54.14 | 86.13 | 82.00 | 74.09 |

- **35B sender 全面超过 753B sender 的 Mostik 报告值**：AIME-135 +3.9、HumanEval +8.8、MBPP +7.9、LCB v6 +0.2——sender 缩小 21 倍反而更强（注意非受控比较的保留）。
- **397B 反而弱于 35B**（AIME-80 71.88 vs 75.63）：训练动态分析（Appx. I）显示 397B run 在 ~step 180 出现一次显著的 clipping 尖峰（平滑截断率 ~40%，平均 critic 分从 ~0.85 跌破 0.60，响应长度激增），此后训练指标恢复但验证精度只从峰值 ~86% 部分恢复到 ~84%——**一次瞬态训练扰动造成持久性能损失**。（论文诚实声明：这些统计不构成因果证明。）另一处更极端的失稳：Qwen3 235B→1.7B 的 MATH 训练中 PLatour 精度从 78.2% 直接塌到 0%（step 177，clipping 54.75-59.40%）而 LoRA-GRPO 一路涨到 82.25%——桥训练的稳定性是真实短板。

### 5.4 结果三：推理效率

PLatour 需要双模型 prefill（额外 TTFT），但这是固定前置成本，**随生成变长被摊薄**：解码主导总延迟时，PLatour 的总推理时间接近 standalone receiver（归一化总时间曲线 0.99-1.01 区间）；解码时间本身与 receiver 相同（sender 不参与）。

### 5.5 结果四：PACT-Check——增益归因的泼冷水报告（核心贡献）

**Qwen3 侧（Table 3/A20）PACT-Score**：

| 方法 | ARC-C | MMLU-Redux | Chem(no-CoT) | Chem-CoT | GSM8K | MATH500 |
|---|---|---|---|---|---|---|
| C2C | +11.69 | +22.14 | **−16.67** | −24.94 | −12.68 | −14.35 |
| KV-Lingo | +10.15 | +16.34 | **−34.29** | −25.00 | −10.18 | −12.40 |
| PLatour-SFT | +14.50 | +21.36 | −0.95 | −8.15 | −2.90 | −10.15 |
| **PLatour** | – | – | – | **+3.34** | **+0.23** | **−4.05** |

- **答案选择类（ARC-C/MMLU）有真实转移证据**：三种方法都保有 +10~+22 的 PACT-Score——sender 在 prompt 位置注入的信息直接影响下一个 token 预测，信息路径短而直接。
- **Chemistry answer-only 的警示**：PLatour-SFT 总增益 +39.52 甚至超过了 sender 自己的增益 +17.14，但 PACT-Score 为 −0.95——**连「超过 sender」都不能证明受益于跨模型信息**（TR 全参 SFT 77.62 比它还高）。
- **生成任务的泼冷水**：Chem-CoT 的 PACT-Score +3.34（DT 复现 52.44、IT 62.68、SC 60.95、TR 65.95 vs PLatour 69.29——四对照全部大幅低于但都接近）是生成侧最强证据；GSM8K 的 +0.23 形同虚设（DT 87.76、IT 89.12 几乎追平 89.35）；MATH500 干脆为负（LoRA-GRPO 82.25 > 78.20）。
- **Qwen3.5 侧同样**：PLatour(35B) 数学均分 63.32 < TR Full-GRPO 64.51，编码 79.73 < 80.72——**域平均上两个 PACT 系统都没超过 TR**。
- **细节差异**：PLatour-SFT 对 DT/IT 更敏感（IT 把它打到 0.6-25 分），PLatour(GRPO) 更鲁棒——机制线索：GRPO 桥的投影范数只有 SFT 的 1/4.32~1/8.32（Table A18），小范数桥对替换 sender 特征不敏感。
- TextComm 佐证「信息确实可传但量级有限」：GLM-5.3-Flash sender 用 Understand 提示写 ~300 token 理解笔记，AIME +5.23 / MATH500 +2.35（显著的对照），但远小于 PLatour 的 +15.5/+13.6；而 T2T（一句话背景知识）几乎无效（CI 含零）。

### 5.6 消融链（设计演化叙事，Appx. E）

hidden-state 转移 ≥ KV 转移（所有组合，如 88.05 vs 86.00）；GRPO ≥ SFT/OPD；相对深度匹配（GRPO 下）89.35 vs last-n 86.50（GSM8K，SFT 下相反）；full-to-full 约束 +0~1.55；线性 > MLP；零初始化在直接 GRPO 下 step 0 即 84.7（随机初始化 79.3）、120 步后 88.1 vs 85.1。

---

## 六、效果优势的根源解释

### 6.1 根源机制与证据链

**解释 1：为什么 PLatour 能在生成任务上成功而 KV 系方法全线失败？**

因果链：生成任务要求 receiver 自建长链思维、无后续 sender 支持 → KV 替换/融合破坏了 receiver 自身 prefill 产生的 cache 语义（大容量差距下线性初始化对齐不了两模型的 KV 空间）→ receiver 的第一 token 就在「别人替它写的上下文」上起步，长生成中误差累积 → 低于裸 receiver；PLatour 传 hidden states 且**残差加法+零初始化**——receiver 自己算 K/V，sender 信息只是残差修正，训练前行为恒等 → 借力而不越位 → GRPO 直接优化端到端奖励。
【论文实验已支持：Table 1 生成三基准的方向性分裂；Table A14 中零初始化+传播把 KV 变体从 75.63 拉到 82.37 仍不及 hidden-state 的 83.62——共享单一 hidden 映射再经 receiver 原生投影生成 K/V，比独立学两套 K/V 映射是更合适的接口。】

**解释 2：为什么 GRPO 训出的桥增益更「像适配」而 SFT 桥更「像通信」？**

因果链：GRPO 奖励只看结果 → 学到的桥范数小（SFT 的 1/4-1/8）→ 对 sender 特征的依赖弱（DT/IT 干扰小）→ 增益更多来自温和的接收侧调整；SFT 模仿 sender 轨迹 → 必须从 sender 表示里抽取更多问题特定内容 → 桥范数大、DT/IT 下崩坏（IT 把 PLatour-SFT 打到 0.80）。
【论文实验已支持：Table A18 范数比 + Table A20/A22 敏感度对比。】【推测标注：范数与信息依赖的因果方向（是大范数导致依赖还是依赖导致大范数）论文未断言。】

**解释 3：为什么答案选择类有真转移、长生成类难有？**

因果链：答案类任务的信息路径短——sender prefill 表征 → 注入 → 直接影响下一 token 的选项分布，任何与任务相关的特征注入都能即时兑现；生成长链任务中 receiver 必须**在几十步生成中持续利用**注入的初始状态信息，而奖励同时也能靠 receiver 自己被桥「调好」获得 → 优化轻易滑向适配路径（SC/TR 都能追上）→ 信息转移的边际贡献被淹没。
【论文实验已支持：PACT-Score 的任务模式（ARC/MMLU +10~22 vs GSM8K +0.23/MATH500 −4.05）；Fig. 1b 的对照图。】

**解释 4：397B 为何不如 35B？**

因果链：桥训练梯度穿过冻结 receiver，sender 表示的尺度/分布随规模变化 → 更大 sender 的注入更难被小学习率下的线性投影稳定吸收 → 一次 clipping 尖峰（响应集体顶到 16K 上限、critic 崩落）把策略推开 → 后续恢复不了峰值精度。35B 的表示动态更温和，训练全程稳定在 ~87%。
【论文实验已支持：Fig. A1 的训练动态四联图。】【推测标注：论文明确说「plotted metrics alone do not establish clipping 为因」；机制层面（为何更大 sender 更易触发失稳）未解。】

### 6.2 相关工作检索与对照（本次检索范围：论文 Related Work 及其引用文献，按任务约束未做外部网络检索）

| 研究 | 相似尝试 | 相关结论 | 与本文的差异与适用边界 | 对根源解释的影响 |
|---|---|---|---|---|
| Mostik 2026（闭源） | 753B→4B 桥接 | 关闭 47% 差距 | 训练细节/数据未公开 | **支持（弱）**：证明大 sender 桥接可大幅提升；本文显示 35B 即可复现并超越其报告值——修正了「sender 越大越好」的直觉 |
| C2C（Fu et al. 2026, ICLR） | KV 门控残差融合+SFT | 选择题任务大幅提升 | 生成任务失效；本文定位其边界 | **支持**：解释 1 的 KV 接口不匹配论据 |
| KV-Lingo（Castin et al. 2026） | 闭式回归+KL 的 KV 翻译 | 同构模型切换可用 | 以「对 receiver prefill 保真」为目标，非任务增益 | **限定**：其目标函数（保真）与 PACT（增益）根本不同，生成任务崩坏部分源于此 |
| Zhang & Emu 2026 / Cheng et al. 2026 | 推理时干预审计潜通道 | 潜通道未必真通信 | 参数冻结的审计；PACT-Check 增加单独训练对照 | **支持**：与本文「DT/IT 复现大部分增益」的发现同向——通信-适配混淆是潜空间方法的系统性问题 |
| Huh et al. 2024（Platonic 假设）/ OPRD | 表示趋同的证据 | 模型间存在共享几何 | 理论动机；本文指出结构相似≠信息转移 | **限定**：正面（线性桥够用）与反面（PACT-Score 常接近零）都对其适用范围给出经验边界 |
| Li et al. 2026a（on-policy 蒸馏再思） | 大到小在线蒸馏 | 容量差距使模仿困难 | 解释 SFT/OPD 失败的外部佐证 | **支持**：解释 1 的「拒绝模仿」设计 |
| He et al. 2022（PEFT 统一视角）/ prefix / adapter | 统一 PEFT 形式 | 皆 hidden-state 修改 | 本文借其构造 SC=adapter、DT=prefix 的极限情形 | **支持**：通信-适配连续谱的理论骨架 |
| Moschella et al. 2023（相对表示缝合） | 零样本模型缝合 | 跨模型零样本可迁移 | 无任务奖励训练 | **补充**：共享结构存在性的又一证据 |

### 6.3 综合判断与未决问题

- **多研究共同支持的机制**：(1) hidden-state 残差注入优于 KV 直接映射（本文全部消融 + C2C 的门控残差遗产）；(2) 任务奖励直接优化优于模仿类目标（GRPO vs SFT/OPD/KL，与蒸馏文献互证）；(3) 零初始化+恒等初始是桥训练稳定性的关键（LoRA 同原理）；(4) 潜通道增益中混有大量可由适配解释的成分（与因果审计文献同向）。
- **仍属推测/单源的机制**：GRPO 桥范数小→依赖弱的因果方向；397B 失稳的根因（clipping 相关但未证）；「生成任务信息路径长导致转移被适配淹没」是从 PACT-Score 模式反推的解释，无直接干预证据。
- **优势成立条件**：PLatour 的生成任务优势在同家族（Qwen→Qwen）模型对上验证——跨家族异构表示空间的泛化未测（作者自己列出）；对标 Mostik 非受控（数据/桥策略未知）；「35B>397B」是单次训练事故下的结论，重跑未必复现。
- **可能的失效条件**：receiver 已被同域 GRPO 充分训练时（TR 追平）；任务的主要瓶颈是解法搜索而非问题理解时；sender 与 receiver 表示空间差异过大（跨家族）时；桥训练在大 sender 下不稳定（已实证两种失稳模式）。

---

## 七、必要知识反推

### 7.1 领域知识层

- **Transformer 推理的两阶段解剖**：prefill（并行、一次开销、产生 KV/循环状态）与 decode（自回归、逐 token、延迟主导）——不理解成本结构就看不懂 PACT 的分工价值，也看不懂「为什么传 hidden states 就能兼容线性注意力」。
- **KV cache 机制**：K/V 的含义、RoPE 位置编码施加于 K、key normalization——KV-Lingo「在 RoPE 前翻译 K」这类细节的取舍依赖它。
- **混合注意力架构**：线性注意力层用循环状态而非 KV cache——这是「KV 传输路线天然不支持混合架构」这一设计约束的来源。
- **GRPO/RL 后训练**：组相对优势、clipping、长度截断惩罚——PLatour 的训练与失稳分析全建立在其上。

### 7.2 方法论知识层

- **潜空间通信研究版图**：传输介质（soft embedding/激活/KV/hidden state）×训练目标（对齐/蒸馏/任务奖励）×系统目标（保真/增益）的分类学——定位任何新方法的前提。
- **PEFT 统一视角**：adapter/prefix/LoRA 皆为学到的 hidden-state 修改——「桥=适配器」这个归因洞察直接来自它；没有这个视角，PACT-Check 的 SC/DT 对照设计不出来。
- **对照实验方法论**：推理时干预 vs 单独训练对照的互补性；「最强对照」基准的选择（PACT-Score 对 max 而非平均）；非受控比较的措辞纪律（对 Mostik 的每一处限定）。
- **Platonic 假设与表示对齐文献**：什么时候结构相似性可用、什么时候会塌缩到平凡解（双侧对齐的零解反例）。

### 7.3 工程知识层

- **数值精度管理**：FP8 sender、FP32 投影计算、BF16 注入、FP64 累积统计（KV-Lingo 初始化）——跨模型桥接的精度预算分配。
- **训练基础设施**：verl 框架、梯度穿过冻结 receiver、HuggingFace prefill + vLLM 解码的混合 cache 传递、KV cache 从训练栈迁移到推理栈的工程。
- **数据卫生与评测协议**：训练池与评测集的重叠识别（AIME 55 题剔除）、近重复归入同 split、固定 per-question/per-sample 种子跨方法共享、avg@k vs pass@k 的语义区分、答案泄漏的人工筛查流程。
- **超参敏感度**：lr 按参数规模分档（全参 1e-6 / 桥+LoRA 1e-5）、warmup/decay 方案的消融纪律。

### 7.4 知识融合的关键节点

1. **「latent tools」的心智模型**：把 sender hidden states 当工具而非教师——融合了 RL 的工具使用视角与蒸馏的失败教训（模仿大模型分布难），一举解释了为何 GRPO 成、SFT/KL 败。这是全文方法设计的支点。
2. **「通信-适配连续谱」的理论构造**：用 PEFT 统一视角推导出 DT→prefix、SC→adapter 两个极限情形，把「增益归因」从哲学问题变成可操作的对照实验——领域知识（PEFT）与方法论（对照实验）在这一点上化学反应。
3. **「接口选择」的架构判断**：传「共享的 hidden 表示再让 receiver 用原生投影自产 K/V」优于「替它算好 K/V」——融合了表示学习（单一共享语义）与系统工程（谁拥有计算权）的考量。
4. **「诚实归因」的评测伦理**：把「超过 sender」也不能证明转移（Chemistry 案例）写成论文的招牌结论——强者是知道自己的方法何时可能没在真正「通信」的作者。

---

## 八、论文中可以提取的通用性灵感

### 灵感 1：增益归因审计——引入可训练组件时必须问「提升来自组件声称的机制吗」

- **核心思想**：任何新增可训练模块（桥/适配器/提示）带来的性能提升都是「声称的机制」与「参数高效适配」的混杂；需要设计「移除机制但保留训练」的对照组族来归因。
- **论文证据**：Dummy/Irrelevant Transfer 复现 GSM8K 几乎全部增益（89.12/87.76 vs 89.35）；TR 在 MATH500/Qwen3.5 域平均上反超 PLatour；「超过 sender 的 +39.52 增益」在 TR 对照下 PACT-Score 为 −0.95。
- **推广场景**：(1) RAG 系统引入检索器增益 vs 微调效应的归因；(2) 多 agent 通信协议是否真在传递信息；(3) 提示工程提升 vs 隐式分布偏移；(4) 添加噪声/虚拟模块的 placebo 基线制度化；(5) 药物临床试验的安慰剂对照思想在 ML 组件评估中的移植。

### 灵感 2：残差+零初始化——新组件应该从「什么都不做」出发

- **核心思想**：给冻结系统加新组件时，用零初始化残差连接保证初始行为与原系统恒等——训练从原点出发而不是从随机扰动出发。
- **论文证据**：零初始化 step 0 即 84.7（随机 79.3），120 步 88.1 vs 85.1；LoRA 的 B=0 同原理；直接替换式（KV-Lingo）在大容量差距下崩坏。
- **推广场景**：(1) 控制系统的增量执行器设计；(2) 数据管道的新变换默认 no-op；(3) 组织流程新增审批环节的「零摩擦启动」；(4) 数据库 schema 迁移的默认透传视图；(5) 教育中渐进式脚手架（从学生现有能力恒等处接入）。

### 灵感 3：直接优化最终奖励，而非模仿演示者

- **核心思想**：当弱执行者与强演示者容量差距大时，模仿其输出分布往往失败；更稳健的是把演示者的中间产物当「工具」，直接以最终结果奖励训练「使用工具」的能力。
- **论文证据**：SFT/OPD 在 MATH500 低于裸 receiver（83.55/83.20 vs 84.60），GRPO 88.05；蒸馏目标在 PACT 场景的系统性失败与 on-policy 蒸馏文献互证。
- **推广场景**：(1) 员工培训：给新手「专家的中间笔记」+结果考核而非「照抄专家操作」；(2) 检索增强生成：学「用检索结果」而非模仿检索语料风格；(3) 机器人示范学习的残差策略学习；(4) 模型合并/级联中的组件协作训练；(5) 体育教练的「喂球感」辅助（工具化示范）。

### 灵感 4：理解与搜索可分离计价——「贵的计算一次性，便宜的计算循环」

- **核心思想**：把任务分解为「一次性的理解/规划」（用强模型）与「迭代式的搜索/生成」（用弱模型），两者的成本结构不同，可分别选配硬件。
- **论文证据**：sender 只 prefill（TTFT 增加、可摊薄），decode 全归 receiver——总时间在长生成下接近裸 receiver（0.99-1.01 归一化）；大模型理解 + 小模型长搜索的两个独立扩展轴。
- **推广场景**：(1) 编译器（一次性优化）vs 解释器（循环执行）的资源分配；(2) 建筑设计（贵的一次性结构计算）vs 施工迭代；(3) 法务（一次高质量尽调）vs 日常合规检查自动化；(4) 长上下文 agent 的共享系统提示预计算缓存（论文自己提出的 amortize 机会）；(5) 教育分层：名师讲概念（一次）+AI 陪练（循环）。

### 灵感 5：信息路径长度决定可验证转移的上限——短路径好审计，长路径易被稀释

- **核心思想**：注入的信息要在多长的推理链上「兑现」决定了它能否被检测与保真——短路径（直接影响下一步）转移真实可证，长路径（需在几十步生成中持续起效）的边际贡献易被其他因素淹没。
- **论文证据**：ARC-C/MMLU（注入→下一 token）PACT-Score +10~22；GSM8K/MATH500（注入→长 CoT）+0.23/−4.05；Chem-CoT 居中 +3.34。
- **推广场景**：(1) 知识管理系统：即时可用的显性知识 vs 需长期内化的隐性知识分开设计；(2) 药物起效路径长短与临床终点选择的匹配；(3) 组织信息传达：直接影响下一步的简报 vs 塑造长期文化的培训分开评估；(4) API 设计：同步短路径调用 vs 异步长流水线的可观测性差异；(5) 教育：即时反馈练习 vs 延迟收益概念学习。

### 灵感 6：开源强基线 + 审计协议比单点 SOTA 更有生态价值

- **核心思想**：当一个方向的头部系统闭源时，社区最需要的不是又一个高分，而是「可复现的强基线」+「能辨真伪的评测协议」的组合——前者定地板，后者防虚高。
- **论文证据**：PLatour 用 1/21 的 sender 超越 Mostik 报告值，同时 PACT-Check 揭示包括自己在内的 PACT 系统增益中适配成分之重；作者把「何时不工作」写成与「何时工作」等重的章节。
- **推广场景**:(1) 基准维护组织发布开源参考实现+作弊检测套件；(2) 安全领域标准的攻击/防御基线套件；(3) 新兴技术采购中的第三方可复现验证；(4) 科学的阴性结果发表渠道；(5) 竞赛平台附带 sanity-check 提交（全零/随机基线）防刷榜。

---

## 附录

### A. 关键数字速查

| 项目 | 数值 |
|---|---|
| Qwen3 生成（235B→1.7B） | Chem 41.43→69.29（+27.86）/ GSM8K 80.86→89.35（+8.49）/ MATH500 73.40→78.20（+4.80） |
| 同设置 C2C / KV-Lingo | 全部低于裸 receiver；KV-Lingo GSM8K 17.06 / MATH500 11.95（灾难） |
| Qwen3.5 35B→4B | AIME-80 59.22→75.63 / AIME-135 59.12→74.63 / HMMT 46.35→64.58 / IMO-Ans 41.13→49.75 / LCB 40.36→54.29 / HumanEval 83.16→94.97 / MBPP 65.08→89.93；数学均分 63.32、编码均分 79.73 |
| Mostik（753B→4B 报告值） | AIME-135 70.74 / LCB 54.14 / HumanEval 86.13 / MBPP 82.00 / 编码均分 74.09 |
| 397B 变体 | AIME-80 71.88 < 35B 75.63；DAPO 训练 ~step180 clipping 尖峰（平滑 ~40%、critic 0.85→<0.60），验证 86%→83%→84% 部分恢复 |
| MATH 训练塌缩 | 235B→1.7B PLatour 78.20%→0%（step 177，clipping 59.40%）；LoRA-GRPO 同期涨至 82.25% |
| PACT-Score（Qwen3） | ARC-C/MMLU：C2C +11.69/+22.14、KV-Lingo +10.15/+16.34、PLatour-SFT +14.50/+21.36；Chem-CoT PLatour +3.34；GSM8K +0.23；MATH500 −4.05 |
| Qwen3.5 域平均 vs TR | 数学 63.32 < Full-GRPO 64.51；编码 79.73 < Full-GRPO 80.72 |
| Chem-CoT 四对照（PLatour 69.29） | DT 52.44 / IT 62.68 / SC 60.95 / TR 65.95 |
| GSM8K 四对照（PLatour 89.35） | DT 87.76 / IT 89.12 / SC 88.06 / TR 88.53 |
| 桥范数比 | SFT/GRPO = 4.32–8.32×（GRPO 桥更小） |
| TextComm（Understand） | GLM-5.3-Flash：AIME +5.23 / MATH500 +2.35（vs PLatour +15.5/+13.6）；T2T 无效（CI 含零）；4B 自写笔记 AIME −3.89 |
| 消融要点 | hidden-state 88.05 > KV 86.00；GRPO 88.05 > SFT 83.55 / OPD 83.20；零初始化 88.1 > 随机 85.1（120 步）；传播 +2.35~2.76 |
| 推理效率 | 双模型 prefill 增 TTFT；长生成下总时间归一化 0.99–1.01（≈裸 receiver） |
| 数据卫生 | AIME-80 = 135 题剔除 55 个 DAPO 重叠（53 train+2 val）；DeepCoder 18,873 训练对三测试集去污；TextComm 逐条泄漏筛查（3 例泄漏剔除后结论不变） |

### B. 术语备忘

- **PACT**：Prefill–to–Autoregressive Cross-model Transfer，大 sender 仅 prefill、小 receiver 独立生成的潜通信形式。
- **PLatour**：Prefill-only LATent cOmmUnication for Reasoning，本文开源框架（零初始化线性 hidden-state 桥 + GRPO）。
- **PACT-Check / PACT-Score**：四对照归因协议 / 对最强对照的准确率裕度（>0 才是 sender 信息受益的证据）。
- **DT/IT/SC/TR**：Dummy Transfer（固定模板均值特征）/ Irrelevant Transfer（无关题目特征）/ Self-Communication（自通信=线性 adapter）/ Trained Receiver（LoRA/全参微调）。
- **avg@k**：k 次采样的平均单响应准确率（非 best-of-k）。
