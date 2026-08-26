---
title: "领读Kimi K3技术报告：一个清华架构博士眼中的注意力谱系与「有效scaling」"
date: 2026-08-26
draft: false
tags: ["灰色信源", "论文精读", "Kimi K3", "注意力机制", "线性注意力", "MoE", "蒸馏", "AI Infra", "开源模型", "AI趋势", "持续学习"]
categories: ["podcast-summary"]
summary: '一集面向技术读者的Kimi K3技术报告领读播客，嘉宾孙宇涛（清华计算机系博士生、上海创智学院pre-doc，研究方向LLM架构与预训练）从K3出发串联起十多篇前作，把KDA线性注意力的每一项公式还原成RetNet→Mamba→DeltaNet→Gated DeltaNet的历史叠加，讲清channel-wise衰减、low-rank dk与BF16 tile的kernel co-design，MLA+QK-norm式门控的稳定性逻辑，Latent MoE对通信开销的削减，以及Quantile Balancing如何用线性规划一步求出负载均衡bias。预训练侧K3反潮流回归cosine decay、在混合注意力里用NoPE让长上下文免调参外推；后训练侧on-policy蒸馏成为多teacher多reward的「多模型合板」方案。嘉宾的暴论：大模型架构没有本质创新了，K3最核心的变量是size——2.8T总参、百B激活、K2的2.5倍scaling效率，而把size做work才是真创新。'
---

> 基于播客「领读Kimi K3技术报告：从架构创新聊起，注意力美学、多教师蒸馏和开源MoE」完整转写整理。主持人：**小俊**；嘉宾：**孙宇涛**（清华大学计算机系博士候选人、上海创智学院pre-doc学者），博士期间研究方向为LLM架构与预训练。文章中的技术判断和观点除注明外均出自嘉宾口述，K3数据均以「技术报告称」口径转述。

## 先说结论：K3是谱系的集大成，真正的变量是「有效scaling」的魄力

这集播客听起来像一门压缩版的「现代注意力架构史」：嘉宾孙宇涛从2023年自己的第一篇线性注意力工作讲起，一路串过RetNet、Mamba、DeltaNet、Gated DeltaNet、Qwen团队的QK-norm式门控注意力、DenseNet与Hyper Connection的残差谱系、英伟达的Latent MoE、苏剑林博客里的Quantile Balancing，最后收束到K3技术报告本身。他反复强调一个读论文的方法论：只看K3你会知道它做了什么，但不知道每一步「为什么」——因为K3几乎每一个设计项背后都有一条清晰的研究脉络，K3做的是把这些被社区反复验证的零件装进一个2.8T的模型并让它们全work。

由此引出嘉宾的核心判断，也是整期播客最有信息量的地方：**K3真正难的不是单点创新，而是「有效scaling」这个非技术决定**——总参数2.8T、激活量约百B、上下文原生1M，比K2的scaling efficiency高出2.5倍（技术报告口径）。用他自己的话说，「扩大size本身不是创新，把这个东西做work中间才有创新」。以及一句暴论收尾：大模型架构可能没有本质创新了，后面都是改良性进步——这是他本人转向世界模型研究的原因之一。

## 注意力的谱系：KDA不是发明，是四层历史结构的叠加

嘉宾的分享结构本身就是一课「怎么读架构论文」。他先声明自己讲talk的逻辑：把论文每个贡献背后的历史脉络打开，而不是复述结果。于是线性注意力部分变成了这样一条链：

- **RetNet阶段**：线性注意力最初只是「k和v做外积再累加」，比ResNet还简单。RetNet引入与token位置相关的衰减项，把线性注意力从位置无关变成位置有关——因为语言的特性是越近的位置权重越高，有限上下文里衰减是必不可少的。
- **Mamba与DeltaNet阶段**：Mamba把位置无关衰减变成位置有关的输入依赖衰减；DeltaNet（概念非苏剑林原创）把固定外积更新改成了Delta规则——本质上是在相同KV cache大小下，从算法上提高了线性注意力的上下文容量。苏剑林的贡献是把看似不可并行的DeltaNet变成了GPU可计算的chunkwise recurrent形式，嘉宾称之为「从infra上本质性的解决」。
- **Gated DeltaNet阶段**：既然衰减有用、Delta规则容量更高，为什么不结合？Gated DeltaNet就是把两者相加，得到更优的线性注意力表达。
- **KDA（Kimi Delta Attention）阶段**：Gated DeltaNet的衰减项是每个head一个标量（写kernel方便），KDA把它细化成channel-wise的衰减——严格更强（可退化为均匀衰减），代价是kernel难写。为了在tile内做数值变换，KDA对dk做了low-rank形式的约束，使其在16-token tile内不超出BF16的动态范围——嘉宾特别强调这是算法与kernel的co-design：衰减可以像RoPE那样「q先大衰减、k反向提升」用绝对位置表示相对衰减（类似-2=3-5的分解），但除以极小的数会爆精度，所以要从数学上把tile内最大衰减量反解出来、控制住。

这条谱系解释了为什么K3报告里的KDA公式「特别长」：每一项都是一段历史遗留，是过去三四年社区探索的逐步叠加。嘉宾个人的品味偏好在此暴露无遗——他认为创新性最强的是DenseNet和早期连接方式的工作，「attention residual考虑的问题DenseNet都考虑到了，只是当时没有attention」。

## 为什么是这些零件：稳定性优先的美学

如果把K3的架构选择连起来看，会发现一条一致的决策逻辑：**模型能力可以差一点，训练稳定性不能崩**。嘉宾用几个具体案例拼出了这条线：

**MLA加QK-norm式门控。** MLA是DeepSeek V2引入、V3和K2沿用的KV cache压缩设计。K3在MLA基础上加了Qwen团队首创的QK门控注意力——嘉宾强调大家最buy in的是它的训练稳定性：pretrain一个round成本极高，训崩了整个checkpoint作废，任何团队都不可接受。这个门控设计与MLA正交，任何full attention都能用。至于「MLA未来会不会成为共识」，嘉宾和主持人有段有意思的对话：嘉宾认为MLA本质是「大号的MQA」，只是更好的trade-off，其收益大部分可以被更好的GQA参数设计拿到；DeepSeek V4转向sparse注意力后不再用MLA（sparse与MLA在prefill阶段不百分百兼容），卢福利（转述）认为MLA在Agent时代会被MTP替代——嘉宾的结论是「MLA从来不是共识，只是一个选择」，K3沿用它是「能跑的东西就不要动」的工程哲学。

**残差与连接。** 从ResNet的两个版本（CVPR/ICCV，pre-norm与post-norm之争）、BERT时代post-norm在模型变大后的梯度消失，讲到attention residual必须是pre-norm的严格超集、Hyper Connection用比hidden state更大的容量表示推理深度状态（嘉宾评价：思想简洁但paper写得太抽象所以没出圈，MHC火了「最重要的原因是DeepSeek提的」）、再到K3实际采用的block attention residual——比DenseNet轻量、用block attention解耦模型深度，推理阶段几乎免费。「推理免费的提升大家上的时候没有压力」是这节的关键机制：任何以更慢推理换更好效果的设计都会被质疑「为什么不直接扩size」。

**Latent MoE与C2GLOU。** 传统MoE的all-to-all dispatch通信量随专家数和hidden size增长，需要DeepSeek式的精细overlap优化。Latent MoE（英伟达团队年初提出，K3跟进）把dispatch的hidden state压缩2-4倍，再通过加大FFN中间维度或拆更多专家把参数量补回来——恰当设计下可以完全保持标准MoE能力，等于用更小通信量买到同等表达，嘉宾称之为「类似free lunch」。而推理latency处在critical path上、无法像训练那样用throughput掩盖，所以推理收益比训练更大。C2GLOU则承接GPT-OSS的思路：MLP中间hidden state容易激活爆炸，GPT-OSS用hard clip，tanh是天然的soft clip，C2GLOU把MLP里所有可能unbounded的环节都用tanh绑住，给激活一个严格数学上界。嘉宾顺带还原了苏剑林（他模拟其口吻「苏建林一定会反对」）的观点：任何优化器都必然出现outlier，想严格控制就得从架构下手——这正是K2时代MuonClip讨论的延续。

**Quantile Balancing与值域分桶。** 苏剑林博客提出、K3报告补全工程细节的路由方案。Loss-Free Routing用启发式更新的bias控制专家负载，问题是底层几层不work、大家只好把前几层改回dense。QB把「激活多的专家bias砍一点、少的加一点」变成线性规划直接求解、一步到位，无需调学习率式超参，第一层也能均衡。报告新增的工程细节（嘉宾标注「博客里没细讲」）：一个step几十million token的精确分位数计算工程上不可行，K3按值域（如sigmoid必在0-1间）切桶做histogram统计，常数存储、易在大规模EP下扩展。

## 预训练的三个反潮流选择：cosine decay、SFT期QAT、NoPE长上下文

**学习率策略上弃WSD回归cosine。** WSD（miniCPM提出）的核心卖点是decay阶段学习最快、把好数据放在decay阶段，且前段学习率不变、与总token量无关，中途可以自由切换训练长度。但K3报告指出了被忽略的问题：任意选token量≠任意选学习率——跑10T最优6e-4，跑20T最优可能就变成3e-4，实际调节难度和cosine一样；而cosine只有两个变量（总token量和最大学习率），更好调参。在所有团队都用WSD的当口，K3选了更古典的方案。

**低精度训练推迟到SFT阶段引入QAT。** DeepSeek V3从头FP8、V4直接W4A8原训，K3则先高精度预训练、SFT阶段再引入量化感知训练。嘉宾给出两层理由：项目管理上，大规模训练用高精度是无论如何不会错的保险方案，scaling中的低精度风险难以提前充分验证；技术上，其实验（嘉宾个人实验口径）表明低精度推理能力从头引入并不带来模型能力提升，「过了定量训练，什么时候引入结果都差不多」。

**NoPE：混合注意力送给长上下文的礼物。** K3原生支持1M上下文，位置编码的关键选择是：full attention层去RoPE用NoPE。这条脉络来自Qwen团队早先的工作（嘉宾「看到第二天就去复现了」，还在会议审稿时给了strong accept——「我近几年可能都没给过几个strong accept」）：混合架构里线性注意力已引入位置信息，full attention去掉位置编码反而更好，且扩长上下文不需要调任何架构参数。嘉宾顺手纠正了一个流行误解：RoPE带来的本质是recency bias，对短上下文建模极有效，但对长文没有帮助甚至有害——「RoPE并不带来任何长文能力」。

顺带一提多模态：K3没有用现成vision encoder（如SigLIP）零样本接入，而是从头训练，因为零-shot接入的gradient norm稳定性显著更差，原生训练「多用些算力但结果无损、兼容性更好」——这与全Muon优化方式的选择逻辑一致。

## 后训练与infra：on-policy蒸馏是「多模型合板」，架构决定分布式方案

**on-policy蒸馏（OPD）的动机迁移**是这期播客对K3后训练最有洞察的解读。OPD最初（微软研究院董领导、MiniLM，嘉宾注明「玉贤做的，去年清华特奖」）是作为白盒蒸馏提出的：不是teacher生成答案学生SFT，而是学生自己做题、teacher逐步verify纠正（reverse KL）。但工程实践中蒸馏设定（大模型教小模型）在「一家公司一个旗舰模型」的时代不存在了，于是OPD的动机变成了：**把不同能力、不同reward model的RL专项模型简化成不同的teacher模型，做多teacher合板**。嘉宾点破了两层原因——技术层面，RL把math verifiable reward、preference reward model等异质reward合到一起比「模型同构、数据异构」难得多；组织层面，post train团队管理被大幅简化。他注明「这不是K3首创，小米、智谱等大家现在都是这么做的」。

**投机解码与draft model。** 嘉宾认为这是K3「目前优化得不完善」、还有延伸空间的一块。小米（基于TensorRT-LLM系方案加DiffuFlash投机推理）做到过1000 TPS的参考案例被展开讨论：EAGLE/MTP利用大模型hidden state把draft model做小、接受率做高；DiffuFlash则把diffusion language model（小batch高throughput）与AR模型统一。陈建（DiffuFlash作者，嘉宾与本人交流过）的观点被引用：draft model的收益依赖pretrain阶段就留好MTP接口，且draft model需用KL loss替代next token prediction loss做特化微调，对齐verify过程。

**infra部分的核心洞察：架构选型反过来决定分布式方案。** 这是整期播客把「模型架构」与「AI Infra」串起来的关键环节：

- **KDA的CP（上下文并行）**：full attention的CP概念简单（ring/zigzag attention），线性注意力的CP则利用chunk可任意拆分的数学性质做双层chunk——GPU间以8K大chunk递归、GPU内再做tile级并行。
- **MoE EP的冗余专家方案**：dropless EP下各专家负载不均导致GPU互相等、通信多一个「先告知token数」的阶段；带drop的EP是infra最优但模型能力损失明显。K3的方案是加少量冗余专家、online planning动态规划每张卡放哪些专家，数学上可证明少量冗余即可保证卡级token数完全对齐——嘉宾称之为「经典的数据结构与算法问题」；但卡级均衡≠专家级均衡，要专家级均衡只能上有损策略，「上限就卡到这里」。
- **通信overlap：K3与DeepSeek分道的地方。** DeepSeek V3用极细粒度的MoE overlap（把上一个batch的backward和下一个batch的forward重排融合，需要把forward/backward拆到原子计算手工重排）加DualPipe缓解PP气泡。K3因为Latent MoE已把通信量砍小，只需把MoE前后向通信与shared expert计算overlap——单batch内部就能藏住通信，不需要跨batch的复杂重排，还顺带给推理critical path latency带来免费收益。PP的处理同样更轻：不追求DualPipe的对称，而是把内存压力大的PP rank的activation直接转移到压力小的rank上。
- **内存与optimizer切分**：大规模offload到CPU、全部offload项用FP8存储「省一倍才能跨过不等式边界」；Muon优化器需要全矩阵做牛顿-舒尔茨迭代，不能像Adam那样element-wise摊平切分，需特殊处理。
- **多模态与RL的细节**：vision encoder内部无压缩、输出前才压缩（32K→8K token），主干不用CP时encoder可能要开CP；VL token比例不均会同时破坏DP和PP，K3把vision encoder放在PP中间或尾部，用前段闲置的中间stage提前算encoder后续步骤。RL侧一个「时不时蹦出有意思的东西」：reference model无梯度，可与gradient buffer复用同一块memory——「gradient buffer reuse for non-policy model forwarding」。

## 读技术报告的方法论与行业判断

除了技术内容，这期播客对「怎么读前沿模型报告」本身给出了可迁移的方法。嘉宾的读法是：把每个设计项还原到它的研究谱系里（KDA的四层叠加、残差连接的DenseNet源头、QB的Loss-Free前作），区分「创新」与「沿用的调优」（MLA和MuonClip是K2沿用、K3未动），并注意报告明确写出工程细节的价值——「如果团队组织密度不够，大家就会沉迷于小道消息、私下打听这些是怎么处理的；写出来就不需要找小道消息了」。infra复现难度的问题他也直接回答：「infrastructure本质上没有什么难的，它是个高度确定性的东西，难的是自己提出来——这对你分析pretraining的方式和组织密度要求很高。」

对行业格局，几个判断值得记录。K3与DeepSeek V4的对比：DeepSeek出1.2T和更小两档、主打性价比（flash模型做得好），Kimi目前优先提升开源模型能力上限而非性价比——「现在K3的API相当贵」。K3的历史地位：嘉宾认为是国内第一个把模型scale到这个程度并全流程打通的（此前最大是千问72B量级的开源模型），「有size才有智能」。对Kimi「研究方式科学」的认同：源自模型内科（pretraining introspection）能力——行为trace可可靠获取、collapse可明确归因。以及组织观察：「Kimi有个特点是所有人好像都不怕杨植麟——有道理谁说了算」「它不是个人英雄主义的工作」；新团队重做模型的机会窗口取决于leader的taste和组织氛围能否达成，「这个条件大概率不存在，所以机会也就不存在」。

最后的展望部分是暴论时刻：模型size毫无疑问继续扩大，但受限于人类互联网信息总量有限、数学上不可能无限；纯语言scope内「只要能力能被well defined就能达到」，AGI的问题恰恰是没有被well defined——如果定义为具身、真实物理世界交互，gap会大得多。嘉宾本人转向世界模型是职业生涯判断：「大模型不存在太大改进空间，对个人来说不是能做出大贡献的领域了」。

## 三条可迁移的判断

1. **架构-infra co-design已成主战场。** KDA的low-rank dk为BF16 tile而生、Latent MoE让通信overlap从跨batch重排降级为单batch内shared expert overlap、线性注意力chunk可任意拆分催生双层CP——每一个架构选择都在改写infra的最优解。观察一个团队的架构品味，看它愿意为kernel改多少算法、为通信改多少模型。
2. **「推理免费」是架构创新的准入门槛。** 残差连接改进、NoPE这类推理零开销（或近零）的设计被迅速采纳；任何用推理减速换效果的方案都会面对「为什么不扩size」的质疑。这条筛选逻辑解释了为什么K3的零件清单长这样。
3. **稳定性优先、能力上限靠size。** 训崩一轮的checkpoint损失不可接受，所以QK-norm式门控、C2GLOU上界、全Muon原生训练被选中；而模型能力的本质变量仍是参数量——「benchmark有无数种方式达到满意结果，但参数大小是智能最本质的载体」。做技术选型时先问：这个设计是保稳定性的还是提上限的？前者优先级永远更高。
