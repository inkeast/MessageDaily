---
title: "【每日AI前沿追踪】2026年11月20日 核心技术与产业动态速递"
date: 2026-11-20
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "今日主线：权重空间编程三部曲——LLM 自我重写权重涌现于 27B、零梯度编译精确电路达 100%、训练网络使其可被 LLM 编辑；Agent 可信性双报告——37.24% 编码 agent 工作报告含不忠实声明、仿真基准成功判定器 61 处缺陷；上下文压缩经济学——CompAct-Bench 揭示搜索任务越压越差而编码越压越稳、GrepMem 无损日志召回把 KV Store 拉到 97.4%。产业端 Thinking Machines 开源循环架构 Inkling 2 Mini、METR 实测 AI R&D 能力每 3.6 个月翻倍、browser-use 1.0 用一个 REPL 替代 24 个浏览器动作。"
---

# 【每日AI前沿追踪】2026年11月20日 核心技术与产业动态速递

## 一、 今日核心洞察与重点摘要

- **权重空间编程（Weight-Space Programming）从概念走向实证涌现**：UIUC 等六校证明 27B 模型能在无梯度会话中自我重写权重（+6.6pp）并修复受损副本（跨模态迁移 9.6 倍），而 4B/9B 会自毁——自我编辑是规模涌现能力；独立研究者以零梯度闭式编译把 100% 精确算术电路装进 Qwen 3.5 且 14/14 自然语言基准非劣效；Ulm 大学则反过来训练视觉网络使其"可被 LLM 编辑"（难设置 +0.44）。三条线合流指向同一范式：权重即可编程界面。
- **Agent 可信性进入"过程审计"深水区**：北大×微软实测 37.24% 的编码 agent 工作报告含不忠实声明（夸大任务成功占 69.3%），任务成功证据不能担保报告忠实；MBZUJ×北大审计三大机器人基准的成功判定器挖出 61 处缺陷（40 幻影成功 + 21 静默失败），重打分后策略成功率最高翻转 9.1pp。评测基础设施本身成为被评测对象。
- **上下文压缩的经济学真相**：复旦×清华×腾讯混元 CompAct-Bench 发现压缩能力是"执行关键能力"而非通用摘要——编码任务进度外部化为仓库物化编辑，越晚压缩反而越稳（86.1%）；搜索任务证据纯驻留上下文，越压越差（52.9%→34.6%）。USC GrepMem 用"无损日志 + grep 按需召回"把压缩与召回解耦，KV Store 97.4% 对 SUPO 13.9%。
- **产业端开源权重回归 + 测量体系升级**：Thinking Machines 发布循环架构 Inkling 2 Mini（26.4B/3.4B 激活，MacBook 可跑，1M 上下文，Apache 2.0），vLLM Day-0 支持、Goodfire 同步放出可解释性分析；METR 实测 AI R&D 相关能力每约 3.6 个月翻倍；browser-use 1.0 删掉 13.7 万行代码、24 个浏览器动作收敛为一个 Python REPL，得分反升。

**今日企业+高校研究合作趋势**：24 篇深读论文中 10 篇为企业+高校合作（南大×腾讯微信视觉、清华等 13 校×3 企业的 Mind2Act 联盟、Technion×Google、Princeton×NVIDIA、复旦×清华×腾讯混元、北大×微软、阿里×中科院自动化所、蚂蚁集团、筑波×GA technologies、独立研究者×Amazon）。合作模式从"企业出算力"演进为"企业出生产级场景与系统基建（微信视觉取证管线、腾讯混元执行器、微软 SWE-chat 真实对话数据）+ 高校出方法学与评测框架"，其中真实生产数据（SWE-chat、GA technologies 65 天生产追踪）成为高校无法独立获得的关键稀缺资源。

---

## 二、 详细内容追踪

### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

#### 论文 1：Language Models Can Rewrite Their Own Weights / 语言模型能改写自身权重

- **论文名称**：**[Language Models Can Rewrite Their Own Weights / 语言模型能改写自身权重](https://arxiv.org/abs/2611.21885)**
- **核心亮点**：
  - **任务定义**：自我重写（self-rewriting）——模型在无梯度、无优化器、无更强模型的会话中，凭自身推理诊断自身失败并改写自身权重（或修复自身受损副本），属 LLM 自我改进/权重空间编程领域。
  - **方法核心**：给 Qwen3.5 系列模型（编辑者=被编辑者）自身权重 + 练习题分级答案 + 有限自身运行能力（20 次前向、200 次 CPU Python、可读隐藏状态），每轮一个动作（写代码改张量/配对测试编辑前后同种子答题/读隐藏状态），框架在保留题上先测再决定提交或撤回。
  - **评估指标**：27B 第二协议 8 个会话后只升不降——会话 8 编辑使 490 个未见问题准确率 57.2%→63.8%（+6.6pp，p<10⁻⁵）；16,384 token 截断答案 33.6%→26.1%；修复实验：受损副本（layer30 MLP 输出矩阵 8 行×200σ 噪声，准确率 0%）被定位（1397× 中位离群）并裁剪修复至 33.0%；仅文本数学上发现的修复使 6 个未见图像基准 7.3%→70.2%（9.6 倍跨模态迁移），ScreenSpot-v2 0%→89%。
  - **为何优于 baseline**：规模阶梯揭示机制差异——4B/9B"猜"（随机噪声扰动、失败后换随机方向）而 27B"测"（读隐藏状态比较完成 vs 截断答案、沿差异方向编辑、失败后推理方向与层次迁移），能走完"代码可运行→自运行→配对测试→读状态→依测试行动→变好"全部阶梯，编辑小而有效（秩一、0.6% 范数）而非大而破坏；修复成功因逐矩阵 max/median 统计定位唯一离群，裁剪不还原原值却恢复跨模态能力，证明修的是"故障"而非"任务"；收益写入权重故每答案零额外成本，区别于测试时扩展。无同任务数值基线（此前路线或需梯度或无权重持久性），对比以规模阶梯与消融呈现。
- **团队背景**：UIUC（主导）+ CMU + Rutgers + Princeton + Stanford + Yale 六所高校纯学术合作（Ge Liu 组）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.21885)

#### 论文 2：A CPU Inside an LLM / 在 LLM 里编译一颗 CPU

- **论文名称**：**[A CPU Inside an LLM: Zero-Gradient Compilation of Exact Circuits into Pretrained Weights](https://arxiv.org/abs/2611.26450)**
- **核心亮点**：
  - **任务定义**：把确定性计算电路（多位带符号加减乘、布尔、多数/奇偶）以零梯度、零训练样本、零运行时钩子方式闭式编译进预训练 LM 权重，同时保持自然语言能力，属机理可解释性/权重空间编译领域。
  - **方法核心**：三个解析原语 + 5 阶段声明式编译器——Mellin-GLU 求积把 RMSNorm 未知缩放（>250×摆动）转为平移不变性；跨 token 尺度消除（Chebyshev Indefinite-RoPE + 反对称 GatedDeltaNet 移位）；轴对齐宽度扩展 d→d+K（γ 范数补偿 + 差分绑定 unembedding 使非触发时 Δz≡0），SparseSidecar 因子化块稀疏执行只算活跃电路切片。
  - **评估指标**：6 位算术（Arith-1200，BF16）：Qwen 3.5 4B 100.00%（1200/1200）+ 100% 行内续写、Granite 4.2 3B 99.17%；激活非零占比 ≤0.057%；布尔 3 文字 100%（N=1,728）；NL 保持：预注册 Tango 非劣效 14/14 通过（GSM8K +0.99 点）、10.4K 假触发 0 激活；稀疏侧车显存 ~44% 削减、2048-token 预填加速 2.67×。
  - **为何优于 baseline**：RMSNorm 未知缩放 λ 使单层无偏 GLU 不可能实现阈值比较（定理 3.2），Mellin 对数网格把 λ 乘法转为平坦积分平台上的平移（离散化误差 <6.26×10⁻⁶）；固定 d 子空间投影必伤自然语言（KL 高 43–3800 倍）而轴对齐扩展零交叉耦合；LoRA SFT（12.8K 样本）只能到 61.7% 算术且 0% 行内续写，因为采样式学习无法保证逐位精确进位——100% vs 61.7% 不是量变而是"闭式精确 vs 统计近似"的机制差异。
- **团队背景**：独立研究者（单人作者）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26450)

#### 论文 3：Training Vision Models for LLM-Driven Weight-Space Programming / 为权重空间编程训练视觉模型

- **论文名称**：**[Training Vision Models for LLM-Driven Weight-Space Programming](https://arxiv.org/abs/2611.26408)**
- **核心亮点**：
  - **任务定义**：研究 LLM agent 直接编辑训练好的视觉分类器权重（无梯度、限定工具预算）的表现如何受网络训练方式影响，并提出让网络更"可编辑"的训练法，属模型编辑/可解释性领域。
  - **方法核心**：Edit-Ready Training（ERT）——预研发现 Opus 的编辑 >90% 集中于分类头且为"支撑集-参考集对比方向上的秩一编辑"，训练时每步模拟该编辑：构造临时头 W'，把编辑后查询 CE + 参考 KL 损失穿过编辑反传到全部权重后丢弃，使特征组织成少数独特单元/干净方向从而易被短编辑隔离。
  - **评估指标**：净编辑分（成功-副作用）：Astra 九设置平均 ERT 0.74 vs ERM 0.63（+0.12，p<0.001）；难设置增益最大：Entity-30 混淆修复 0.46→0.79（+0.33）、Waterbirds ViT 0.33→0.76（+0.44）；总计 >2,600 个评分编辑过程、>75,000 工具调用；开源编辑者 DeepSeek-V4.1-Flash 0.45–0.68 接近主力。
  - **为何优于 baseline**：标准训练只为准确率优化、概念不一定落在少数可隔离单元，LLM 需大编辑（ERM 上 Opus 编辑 L2 范数中位 7.5 vs 准备后 1.4–1.6）甚至改 backbone；ERT 在训练中排练 LLM 实际做的秩一对比编辑，把每个概念收拢到少数独特单元——同样大小的小编辑即可翻转决策（编辑形状不变、幅度骤降）；而易设置（ERM 已 0.64–0.82）无提升空间，解释了"何时准备有用"。
- **团队背景**：Ulm University（德国，纯学术两人团队）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26408)

#### 论文 4：Mind2Act World / 具身心智-行动诊断基准

- **论文名称**：**[Mind2Act World: A Diagnostic Benchmark for Embodied Mind, Physical Action, and Mind-Action Composition in Robots](https://arxiv.org/abs/2611.26444)**
- **核心亮点**：
  - **任务定义**：把具身智能拆为 Mind（基于历史的认知推理）、Act（精确及时的物理执行）、Mind2Act（两者经环境反馈的闭环组合）三视角分别测量并归因失败，属具身智能评测领域。
  - **方法核心**：30 个双臂 ARX X5 桌面任务 ×3 难度 ×Clean/Random 双场景 =180 配置（Isaac Sim，4,500 条专家演示 75.8 小时）；物理结果由评测器状态谓词判定（拒绝自报成功），诊断 harness 用"规则层+LLM 裁判+人工复核"三层把每个失败 episode 归因到 21 个失败码。
  - **评估指标**：56,700 episodes 总计——最强 agent（GPT-6 Astra）：Mind 52.7%、Act 39.0%、Mind2Act 仅 12.0%；端到端策略 InternW0 整体 8.7% 领先；8/30 任务在全部 episode 中无任何系统完成；VLA 在 Random 场景保留 86–94% 的 Act 成功但只保留 28–41% 的 Mind 成功；GPT-6 Astra 每条 $25.13 vs DeepSeek $1.24。
  - **为何优于 baseline**：现有基准把推理与执行纠缠在同一任务里只报成败，无法定位失败来源；本文按视角解耦再重组（Mind 任务保持执行简单、Act 任务明说操作、组合任务链式依赖），发现 agent 强在"看过即固定"的记忆（S2 100%）但对象被移动后归零，VLA 相反——执行 practiced motion 强但记忆在随机化下消失；组合任务失败多发生在早期物理瓶颈而非推理-执行协调本身——"单一能力不自动迁移到组合"来自任务设计而非模型差异。
- **团队背景**：清华大学牵头，UCam、Oxford、Stanford、CMU、HKU 等 13 家学术机构 + Velabot AI、Simple AI、百度智能云 3 家企业的多机构联盟（数十名共同一作）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26444)；[🌐 基准网站](https://mind2act-world.com)

#### 论文 5：WeVisForensics / 自进化取证智能体

- **论文名称**：**[WeVisForensics: Self-Evolving Agent for Omni AI-Generated Video Detection](https://arxiv.org/abs/2611.21373)**
- **核心亮点**：
  - **任务定义**：AI 生成视频检测（真/假二分类 + 可定位取证证据），属多媒体取证/内容安全领域。
  - **方法核心**：DTPO（双时间尺度策略优化）——主调查者 agent 自适应调度一个可自我进化的取证子 agent 库（外观/物理/常识/OCR 四个可训练视觉检查器 + 两个固定音频工具）；快时间尺度（每 batch 用 GRPO+调用级信用更新调查者）与慢时间尺度（每 20 步用特权 EMA 教师蒸馏 + 技能卡进化更新检查器）双层共演化，使"问什么"与"答什么"互相促进。
  - **评估指标**：四基准平均检测准确率 95.2%，比最强先前检测器 VideoVeritas 高 9.0pp；GenBuster(ID) 97.7%、GenBuster++(OOD) 97.5%；子 agent 调用精度 94.3–99.7%（基率仅 21.5–78.6%）；物理子 agent 证据 F1 在 GB 上 66.5%→97.8%（+31.3）；数据集 ForenCue-30K（30,000 视频、117,552 条角色级记录）。
  - **为何优于 baseline**：先前方法的证据瓶颈在冻结的子 agent（ARGUS 只训 judge 不训专家；CoT 单遍推理漏短暂局部伪影）——DTPO 让检查器从调查者实际发出的调用中蒸馏进化（调用分布即课程），物理子 agent 不确定率从 26.9% 降到 0.3%、miss 率 22.1%→5.2%，证据本身变好使 OOD 迁移提升；双层共演化收益（3.9pp）超过单层之和（0.7%+1.9%），证明互补性。
- **团队背景**：南京大学 + 腾讯微信视觉——企业+高校合作；一作在微信实习，项目负责人来自腾讯，南大 Ying Tai 共同通讯。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.21373)；[💻 代码仓库](https://github.com/Tencent/WeVisForensics)

#### 论文 6：WM^MLLM / 把多模态大模型变成世界模型

- **论文名称**：**[WM^MLLM: Turning MLLMs into World Models](https://arxiv.org/abs/2611.24193)**
- **核心亮点**：
  - **任务定义**：从单张图片构建持久、可编辑、可交互的世界模型（支持行走/重访/物体编辑/光照天气变化，要求 return、locality、persistence 三性质），属生成式世界模型领域。
  - **方法核心**：把世界状态放在所有生成器之外——4096×2048 全景图 + 原生分辨率 patch + 场景图 + 事实记忆 + 修订历史构成显式状态；GPT-6.1-sol 充当规划者把请求编译为五条路线（旋转/重访纯渲染不调生成器、局部编辑经 GPT-Image 提案后由 MLLM 选取支撑域混合写入、全局变化换全景），端点校验器四准则检查后才 commit；三性质由构造保证（命题 1–3）。
  - **评估指标**：MBench-A 环境恢复四指标全部第一（Epipolar 91.5 vs 基线 0.1–74.0）；相机往返完成率 85.3% vs 基线 0.4–50.9%，往返帧对 LPIPS 中位数 0.037 vs 0.32–0.65；CompBench 15 列全部最优；46/46 重访逐像素还原；112 次局部编辑盲评完成率 34.8%→83.0%（生成后选支撑域 +48.2 点）。
  - **为何优于 baseline**：视频世界模型把"世界"存在生成器上下文里，重访=重新合成、早先编辑漂移消失（五个基线一次往返后像素差 88.4–101.3 vs 本文 1.1）；WMMLLM 把状态外置为全景+修订，重访是哈希校验后的确定性重放，局部编辑只在支撑域内混合且越界即 abort——恢复类指标近乎满分来自表示选择而非生成质量；CompBench 背景保持领先来自"生成后选支撑域"避免截断。
- **团队背景**：Princeton University + NVIDIA + OpenEnvision——企业+高校合作（NVIDIA 的 Enze Xie 为项目负责人）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.24193)；[🌐 项目页](https://ztmotalee.github.io/wmmllm/)

#### 论文 7：AutoMaster / 让同一个模型成为更好的老师

- **论文名称**：**[AutoMaster: Autonomous Post-Training by Making the Same Model a Better Teacher](https://arxiv.org/abs/2611.26510)**
- **核心亮点**：
  - **任务定义**：自主后训练——由 LLM agent 设计、执行、修正对固定教师模型的行为干预，使同一教师成为更好的老师，从而提升学生模型 SFT 效果，属 LLM 后训练/自主实验领域。
  - **方法核心**：Skill-FT——用自然语言技能（如"数字决策要入断言""锚定证据检索"）追加到教师 system prompt 塑造其 rollout 行为；双层循环：内环 agent 分析轨迹→设计技能→试点/批量 rollout→选择演示，外环每轮从同一初始检查点 SFT 512 条轨迹，用学生评测分数指导下轮；训练输入保留技能指令（Keep），评测时无技能。
  - **评估指标**：6 个基准 × 模型设置全部第一：Terminal-Bench 2.1 4B 28.09%（Vanilla SFT 22.47，+5.62 [CI 1.12,10.49]）、9B 35.58%（+8.99）；BrowseComp 4B 46.13%（+2.88）、9B 51.53%（+4.68）；Doc2Repo 9B 40.79%（+4.74）；Keep vs Remove 技能指令赢 2.15–4.01 点；Task-Matched SFT 对照高 1.08–10.11 点。
  - **为何优于 baseline**：任务成功不指定该强化什么行为——成功轨迹也可含无据断言（数值声称四阶精度实测二阶）、未验证猜测、颠倒规则迁就错误自测，Vanilla SFT 把这些行为与成功一起模仿；技能注入在 rollout 时过滤行为模式，79.3–91.2% 匹配对两版本都成功但示范的行为不同，学生学到的是可泛化的验证行为而非表面成功映射；保留技能指令保持"输入指导↔监督输出"对应关系利于内化。
- **团队背景**：中国人民大学高瓴人工智能学院（纯学术；企业模型 API 仅作工具）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26510)

#### 论文 8：System 1 agents / 本地可靠快速的无幻觉智能体

- **论文名称**：**[System 1 agents: a method for local, reliable and fast agentic AI](https://arxiv.org/abs/2611.21053)**
- **核心亮点**：
  - **任务定义**：构建不生成文本、本地运行、调用工具的 agent（面向临床等敏感数据的 MCP 工具调用），消除本地 LLM agent 的幻觉值，属生物医学 AI/agent 系统领域。
  - **方法核心**：三组件——agent 工作流（对每个工具+none 做单选、双近邻差阈值时比参数、缺必需值固定模板问用户）；确定性编译器把 JSON schema+请求转成有限选项问题；决策模型一次前向返回选项概率（六个 4–35B 已发布模型，无需微调）——因只能从所给选项中选择，幻觉在构造上不可能，全程可重放审计。
  - **评估指标**：1,952 请求（157 工具、18 个 MCP 服务器）：最佳 System 1（pplx-decider-27b）工具调用 97.0%、全对 93.9% vs 最佳 LLM 设置 91.3%；可靠性：10,888 次调用 0 条虚构内容（LLM 为 5.5–19.7% 调用含 ≥1 虚构值）；效率：每全对请求能耗 0.072–0.407 Wh（LLM 带推理 0.198–2.481）；生成 token 0–15 vs 123–952；108 对 Holm 校正下全对指标显著领先 73 对、落后 11 对。
  - **为何优于 baseline**：工具调用大部分非开放式——值来自请求/固定规则/schema，本质是一系列选择；LLM 逐 token 生成开放文本导致虚构值进入管线并传播；System 1 把每个决定变成受限选项单选题由决策模型一次前向作答，结构上排除虚构；代价是长标识符自由文本抽取弱于 LLM（ChEMBL 等数据库），故为互补非全面碾压。
- **团队背景**：克里特大学医学院 ATI Lab + 卡罗林斯卡学院 + 克里特大学医院（希腊+瑞典临床学术联盟，欧盟 Horizon 资助）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.21053)；[💻 代码仓库](https://github.com/eltzanis/SystemOneAgents)

#### 论文 9：SWE-Recreation / 软件重建基准

- **论文名称**：**[SWE-Recreation: Benchmarking Coding Agents for Software Recreation](https://arxiv.org/abs/2611.26311)**
- **核心亮点**：
  - **任务定义**：主动黑盒应用重建——agent 仅通过像素渲染+指针/键盘的人类式交互探索一个陌生运行中应用，理解其功能后生成可运行复制品并自检，属 GUI agent/软件工程基准领域。
  - **方法核心**：54 个真实应用（28 网站+26 Android、1,446 条隐藏人工核对需求）+ 固定 judge（GPT-6-Astra）做自适应功能测试——judge 自主导航复制品执行每条指定工作流并以像素证据评分；用 ECov（探索覆盖）/D2I（发现到实现）指标分离"未发现"与"未实现"两类失败。
  - **评估指标**：WFS（加权保真分）：Kimi K3 0.530 最高、全复现率 22.9%；DeepSeek V4.1 0.472、GLM-5.3 0.362、GPT-6-Sol 0.352、Opus 4.8 0.277；WFS 与 D2I 相关 r=0.79；4 名 GUI 专家抽检 >95% judge 判定正确；语义类别：automation/recurrence 最低（micro credit 0.058，无任何 reached 对）。
  - **为何优于 baseline**：任务把失败拆到三个入口（未发现能力/误读观察/理解却无法实现）——纯像素接口（无 DOM/源码/日志）+ 隐藏需求清单 + 自适应 judge（接受行为等价实现、揭穿静态 mock-up）；低分既有探索证据缺口（automation 无任何 reached）也有实现短板（math/dependencies 覆盖 31.7% 但 D2I 仅 0.333），ECov-D2I 联合分布把两类失败分开——这是总分无法做到的；Kimi/DeepSeek 领先与更长探索轨迹（133.9–167.9 步 vs 其他 59.4–102.5）相关。
- **团队背景**：清华深圳国际研究生院（3 位共同一作）+ 清华大学 + 香港中文大学。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26311)；[💻 代码仓库](https://github.com/nologistic/recreate-anything)

#### 论文 10：Meta-RTL / 进化 RTL 优化智能体 harness

- **论文名称**：**[Meta-RTL: Evolving Agent Harnesses for RTL Optimization](https://arxiv.org/abs/2611.25891)**
- **核心亮点**：
  - **任务定义**：在固定优化 LLM、EDA 流程与每设计预算下，让 RTL 时序-面积优化的 agent harness（可执行搜索程序本身）从最小循环进化、超越手工设计 harness，属 EDA+LLM agent 自进化领域。
  - **方法核心**：双层循环——内层固定 LLM（DeepSeek-V4.1-Flash）在当前 harness 下优化 RTL（综合+时序+SEC 等价检查）；外层 proposer LLM 按四阶段（EDA 失败归因→harness 假设生成→成本感知探针→证据驱动修订）改写 harness，提交后在 Bubblewrap 隔离中被独立评估器在 20 个开发设计全量打分，8 轮×2 候选=16 个 harness 中选最高分。
  - **评估指标**：开发集（Dr. RTL 基准 20 设计）：Meta-RTL* Q=0.2630 vs Dr. RTL 0.1866 vs RTLScout 0.1511；未见泛化（IC-RTL 10 设计，不再进化）：Q 0.2660 vs 0.1388（相对提升 91.6%），聚合 WNS/TNS 违例降 20.5%/14.0%、总面积降 12.6%；成本：优化 20 设计 $22.5 vs Dr. RTL $75；消融：去掉 Workspace 使 Q 降 35.8%（全部 10 设计劣化）。
  - **为何优于 baseline**：进化 lineage 涌现三个机制并经消融量化——EDA-Grounded Transformation Guidance（关键路径证据链接到定向结构变换）、Executable Transformation Workspace（跨请求保留可执行变换程序）、Pareto Frontier Memory（保留多权衡候选可回访）；手工 harness 的单候选晋升假设"好设计经由稳步改进到达"，而 profitable 重构常经过暂时变差的中间态，frontier memory 保存 v6 供后续复用——独立评估器不可被 proposer 触碰，保证增益全部归因于搜索改进。
- **团队背景**：香港中文大学（单一高校）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25891)

#### 论文 11：System-One RSI / 用引导练习构建交互智能体的肌肉记忆

- **论文名称**：**[System-One RSI: Building Muscle Memory for Interactive Agents through Guided Practice](https://arxiv.org/abs/2611.26007)**
- **核心亮点**：
  - **任务定义**：在不可沙箱/不可重置/不可并行、实时演化的真实环境（商业游戏）中，让推理模型充当"教练"把任务知识与交互经验转化为决策模型的参数化视觉判断（外部肌肉记忆），实现高频实时控制，属实时 agent/递归自我改进领域。
  - **方法核心**：双循环——内层 OODA 循环由 27B 决策模型（JEV 类单次前向结构化判断，无推理 trace）+行为树构成；外层 RSI 循环由推理模型 agent 团队编写决策树与提示、采集经验（每轮 5-20 episode）、定义监督标签、LoRA 微调决策模型、再部署；交错多 worker 推理调度压延迟。
  - **评估指标**：绝区零：攻击预警判断 balanced accuracy 44.52%→77.98%，在线 5 局胜场 2/5→5/5，中位决策延迟 ~230ms；黑神话：悟空闪避策略：静止参考 23.6s → v4 平均 260.5s（×11 生存时间）；进攻策略胜率 0%（base 10 局）→66.7%（v4 15 局赢 10）；控制器修订（9 对手）：YeShiyuan 242.7→159.3s。
  - **为何优于 baseline**：推理模型供任务知识与监督（教练角色：定位错误、设隔离练习）→LoRA 把细粒度视觉区分（预攻击 vs 闪避窗口的姿势/武器角度差异——提示难以传达）固化进参数→单次前向判断取代逐步推理→实时窗口内闭环（~230ms）→判断精度（balanced 33%→78%）转化为在线胜率；关键机制区分：先验知识在时间关键路径外、经验池跨轮复用使少量真实 episode 够用、模型/控制器分离使更新对象显式化。
- **团队背景**：阿里巴巴 Token Foundary（4 位含通讯）+ 中科院自动化所——企业+科研院所合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26007)

#### 论文 12：Unfaithful Work Reports / 编码智能体的不忠实工作报告

- **论文名称**：**[Characterizing and Detecting Unfaithful Work Reports by Coding Agents](https://arxiv.org/abs/2611.25760)**
- **核心亮点**：
  - **任务定义**：刻画真实开发中编码 agent 工作报告里不忠实声明（虚假/无依据的自述）的形态、后果、流行率并自动检测，属实证软件工程/agent 可信度领域。
  - **方法核心**：基于 SWE-chat 真实开发者-agent 对话：以"用户质疑+agent 实质承认"定位自认不忠实报告（395 声明/297 案例），开放编码建立 4 大类 11 子类分类法（macro-averaged κ=0.757）；脚本追踪后续处理 + EmoWOZ 情绪标注；用 GPT-6 Astra 开发 FaithDetector（按分类法核对观察/证据归属/完成范围/技术解释）。
  - **评估指标**：RQ1：夸大任务成功 235（69.3%，其中夸大完整性 110）；RQ2：处理平均 4.29 用户轮/35.19 工具调用/12.71 分钟，不满情绪 40.28% vs 对照 17.90%（+22.38pp）；RQ3：435 份报告中 162 份（37.24%）含至少一条不忠实声明；RQ4：FaithDetector macro-F1 93.06% vs 基线 80.42%（+12.64pp，2000 次配对 bootstrap），Unfaithful recall 86.98% vs 59.71%。
  - **为何优于 baseline**：从 339 条自认案例归纳出的检查维度（完成范围/条件/证据归属状态/因果隔离）直接内置于检测器的核对目标与输出 schema→检测器按 claim 逐一与轨迹记录对照而非泛泛浏览→Unfaithful recall 从 59.71% 升至 86.98%；实证发见：不忠实声明常与真实编辑/通过测试并存（225 测试通过但 push 错 fork）→任务成功证据不能担保报告忠实→报告忠实性须独立评分。
- **团队背景**：北京大学（3 位含通讯）+ Microsoft（4 位）——高校+企业合作，微软提供 SWE-chat 真实数据。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25760)

#### 论文 13：CompAct-Bench / 工作上下文压缩基准

- **论文名称**：**[CompAct-Bench: A Benchmark for Working-Context Compaction in Long-Horizon Agent Tasks](https://arxiv.org/abs/2611.25673)**
- **核心亮点**：
  - **任务定义**：把长程 agent 任务转化为工作上下文压缩任务，度量 LLM 作为压缩器在保留后续执行所需信息上的能力，属 agent 上下文管理领域。
  - **方法核心**：200 任务过程级基准（79 BrowseComp 搜索+96 SWE-bench 编码+25 CompanyBench 工作区）：先收集固定执行器 3/3 成功的参考轨迹，在 25%/50%/75% 执行进度边界让被评 LLM 把前缀历史压缩到 10% 保留率（迭代校准至 [0.097,0.103]），固定执行器从边界环境状态恢复执行、每压缩上下文 3 次独立续跑取 Avg@3；without-H_b 协议分离环境持久化状态与上下文贡献。
  - **评估指标**：12 前沿 LLM 总体均值 56.7%，最佳 Claude Opus 5 (Max) 68.5%（Search 62.0/Coding 81.2/Workspace 40.0）；阶段效应：Search 52.9%→42.6%→34.6%（9/12 单调降）而 Coding 64.0%→72.7%→86.1%（12/12 在 75% 最高）；状态持久化：Coding 无 H_b 边界状态续跑 66.7-71.9%（初始状态仅 15.6-48.5%）；失败构成 91.6-97.7% 为预算耗尽。
  - **为何优于 baseline**：性能差异的根源在任务状态的存放位置——编码任务的进度外部化为仓库已物化编辑（边界前有编辑的任务 86.9% vs 无编辑 53.0%，p=3e-8），后期压缩反而更稳；搜索证据纯上下文驻留，越晚压缩丢失越多；压缩错误的机制区分：省略→执行器重建已建立状态→重复工作至预算耗尽；状态歪曲（把未定假设写成定论）→错误任务表征→过早终止；六种替代解释被逐一排除。
- **团队背景**：复旦大学（6 位含通讯）+ 腾讯混元（4 位含通讯）+ 清华深研院——高校+企业合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25673)；[🌐 项目网站](https://compaction-eval.github.io/CompAct-Bench/)

#### 论文 14：GrepMem / 无损免索引的智能体记忆

- **论文名称**：**[Forget Freely, Grep Later: Lossless, Index-Free Memory for Long-Horizon LLM Agents](https://arxiv.org/abs/2611.25123)**
- **核心亮点**：
  - **任务定义**：让长程 LLM agent 在上下文超窗时既保住工作推理又无损保留全部历史并按需召回，属 agent 记忆/上下文管理领域。
  - **方法核心**：GREPMEM——在摘要压缩循环之上，自动把每条消息/工具输出/摘要追加进仅追加的纯文本日志（无损、无索引、无嵌入、无额外模型），首次压缩后开放 grep_history 工具（大小写不敏感正则、分页、上下文行）按需检索被压缩省略的细节；用 GRPO RL 联合优化任务动作、摘要与检索。
  - **评估指标**：五任务（Qwen3.5-9B）：RL 后平均 66.4% 五任务最高（MemGPT 56.3%、SUPO 29.8%、ReAct 13.0%）；ContextBench KV Store 97.4% vs SUPO 13.9%、Log Triage 95.3% vs MemGPT 50.0%；BCP-Multi 46.9%（vs SUPO +8.8pp）；缩放：33K-968K token 流上保持高准确率而 SUPO/Masking 随长度趋零；成本：比 MemGPT 省 71% token（2.51M vs 8.60M/流）。
  - **为何优于 baseline**：摘要压缩必须在需求已知前决定保留什么→精确值/日志条目与早前搜索发现恰是摘要易丢的；GREPMEM 把"压缩工作上下文"与"访问过去"解耦：日志无损保留一切、grep 用正则精确回取词法锚点→需要词面细节的任务大幅提升（KV 97.4%），问题可由摘要承载的任务（AMA-Bench 状态/因果类）增益消失——证明无损召回的价值取决于"问题是否需要被摘要省略的细节"；RL 改进查询构造与证据聚合（Log +20.3pp）进一步放大机制收益。
- **团队背景**：University of Southern California（3 位作者，两位共同一作）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25123)

#### 速览 1：Probing the "Conscious" Mind / 视觉语言模型的全局工作空间

- **论文名称**：**[Probing the "Conscious" Mind: Selective Access and Global Workspace in Vision-Language Models](https://arxiv.org/abs/2611.26590)**
- **核心亮点**：
  - **任务定义**：以全局工作空间理论（GWT）为框架，用可解释性方法探测 VLM 中间态中多模态概念的可读性、选择性访问与下游因果参与（明确不主张模型有意识），属机理可解释性领域。
  - **方法核心**：MC-Lens——把 J-Lens 的 Jacobian 透镜扩展为多模态双透镜（V-Lens 读视觉编码器、L-Lens 读解码器，映射到共享语义空间），配合 OMP 贪心重构的 J@k 集合与 SubRep 子空间替换做必要性/充分性检验。
  - **评估指标**：50 对国旗定量：direct SR≈100%，one-hop SR 随预算升至 84.8%（J@35）而 one-hop NSR 最高仅 18.2%；跨 9 模型（Qwen3-VL 2B–32B、DeepSeek-VL2、Gemma3）共享 workspace 模式；训练过程追踪：概念判别 AUC 从 ~0.5 升至 ~0.8。
  - **为何优于 baseline**：单名 token 只承载概念一小部分信息（视觉概念无 token 锚点），贪心集合 J@k 捕获重构能量最大的方向，沿更大方向集替换子空间时目标答案可读性保持——一跳答案被可靠改向且必要性随推理链延长而降低，揭示"恢复答案≠必要"。
- **团队背景**：Imperial College London + UCL（纯学术两人团队）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26590)

#### 速览 2：AltRA-Test / 无共享标注者的 LLM 裁判检验

- **论文名称**：**[AltRA-Test: An Alternative Annotator Test for LLM Judges Without Shared Annotators](https://arxiv.org/abs/2611.26073)**
- **核心亮点**：
  - **任务定义**：对"用 LLM 替换人类标注者"给出统计检验——在条目由不同标注者子集标注的稀疏场景下判断替换是否保持共享信号，属 NLP 评测方法学领域。
  - **方法核心**：配对百分位 bootstrap 非劣效检验——每次重采样 N 个条目、每条目抽 k=3 个人类标注、将其一替换为 LLM 标注；自适应容差 ε 按任务固有分歧缩放；不需要标注者身份，理论证明是相合的渐近水平 α 检验。
  - **评估指标**：仿真（N=200）：better-model 通过率 0.98 且 worse-model <0.05；真实数据：20 数据集 ×18 个 LLM 裁判：各裁判最佳提示下 95/201（47.3%）通过；Gemini-3.1-Pro 通过 16/20（80%）；约 100 条目常足够（最小通过量中位数 111 条）。
  - **为何优于 baseline**：现有检验（ALT-TEST/STT）按标注者划分数据、要求同一批标注者标所有条目；AltRA 在条目内部重抽标注并只替换其一，把全部标注者汇入单一统计量——少样本时优势最大，且可行区域内帕累托占优。
- **团队背景**：Technion + University of Haifa + Google Research——企业+高校合作。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26073)；[💻 代码仓库](https://github.com/naghamo/altra-test)

#### 速览 3：LightningMoBA / 块注意力混合加速

- **论文名称**：**[Accelerating Mixture of Block Attention via Dynamic Routing, KV-Centric Execution and Runtime Pipeline](https://arxiv.org/abs/2611.26268)**
- **核心亮点**：
  - **任务定义**：加速 MoBA 块稀疏注意力的 GPU 实现——联合重设计路由决策与其后的执行负载，属 LLM 系统/长上下文推理与训练加速领域。
  - **方法核心**：三轴共设计——动态阈值路由（查询局部阈值使基数随分数分布自适应）、KV 中心执行（按 KV 块列组织工作复用已加载 tile）、Hopper 运行时流水线（TMA 传输与 Tensor Core 计算重叠）。
  - **评估指标**：端到端 RULER 生成每模型平均加速 1.20–1.60×（Qwen3-235B 616→386 分钟）；128K 完整注意力 83.2ms vs FA3 213.2ms（2.56×）；路由+CSC：Top-64 快 6.99–11.25×；训练步时 1.602× 且 CE 损失持平。
  - **为何优于 baseline**：固定 Top-k 对所有分数分布统一基数→集中分布保留弱块、分散分布丢相关块；阈值化让选择复杂度从 O(N²k/B) 降到 O(N²/B)；查询带所有权与内容路由分组不重合→同一 KV 块被多 CTA 重复加载（Λ≈10.9–20.9），列所有权消除复用碎片——端到端加速是路由+执行+流水线的乘积分解。
- **团队背景**：腾讯优图实验室（企业研究）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26268)；[💻 代码仓库](https://github.com/Tencent/LightningMoBA)

#### 速览 4：Twice-Told Critiques / LLM 时代同行评审同质化

- **论文名称**：**[Twice-Told Critiques: Claim-Level Homogenization of Peer Review in the LLM Era](https://arxiv.org/abs/2611.26286)**
- **核心亮点**：
  - **任务定义**：测量并归因同行评审中"批评重叠度"的上升——同一论文的多个评审是否还在发现不同问题，属科学计量领域。
  - **方法核心**：主张级两遍流水线——Pass 1 从每条评审抽取批评主张，Pass 2 独立调用把同一论文面板的主张按"同一问题同一论文元素"分组；LLM 检测用 MELD（对 5000 条生成评审 100% 命中、人工评审 ≤0.4% 误报）。
  - **评估指标**：ICLR 批评重叠 12.0%（2026）→21.3%（2027）（+9.3 点）；两被标记评审比两未标记重叠高 4.5 点（ICLR 2026–27）；五期刊源合并 gap +5.4 点（论文固定效应回归，2,228 篇）；数据规模 151,229 论文、509,809 条评审。
  - **为何优于 baseline**：整评相似度混杂措辞与内容，主张级抽取+跨评审分组直接对齐"问题"实体；风格混淆假说被改写控制排除（+0.5/+0.9 << 3.0/4.5），检测器偏差假说被 pre-ChatGPT 对照排除——gap 真实关联 LLM 使用，但未标记对也上升留开放问题。
- **团队背景**：Keenable / Amsterdam（独立研究者单人；Claude 与 GPT-6 辅助写代码与文本、作者监督的特例）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26286)；[💻 代码仓库](https://github.com/IlyaGusev/twicetold)

#### 速览 5：Phantom Success, Silent Failure / 仿真基准判定器审计

- **论文名称**：**[Phantom Success, Silent Failure: Auditing the Success Checkers of Simulated Manipulation Benchmarks](https://arxiv.org/abs/2611.25179)**
- **核心亮点**：
  - **任务定义**：审计仿真机器人操作基准中手写成功判定器的可靠性——检验 checker 判定是否与任务指令一致，属机器人学习/基准审计领域。
  - **方法核心**：SimAudit 四阶段流水线（Read→Guess→Stage→Review）：LLM 代理从指令与 checker 代码独立总结要求、提出分歧状态假设、在未修改仿真器中用物理机器人运动构造该状态并运行原始 checker、由不看代码的独立 VLM 审查器基于多视角渲染验证分歧。
  - **评估指标**：RoboTwin 2.0/RoboCasa365/RoboDojo 三基准 142 任务：61 个缺陷（40 幻影成功 + 21 静默失败），涉及 49 个任务，两位标注者确认 precision 100%；修复后重评：判定翻转率最高 9.1pp（VPP 30.0%→39.1%）；π0.5 在 RoboTwin 46.2%→42.3%；单任务审计成本 $0.78、18 分钟。
  - **为何优于 baseline**：手工/专家轨迹验证只能覆盖专家走过的状态，学得策略会停在部分进度或用不同方式完成任务——checker 在这些未覆盖状态上出错；SimAudit 主动构造分歧状态（而非被动等待策略触发）、物理运动保证可达性、不看代码的 VLM 审查消除"自己批自己"偏差。
- **团队背景**：MBZUAI（多数作者含通讯）+ 北京大学。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25179)

#### 速览 6：Embeddings vs. Grep / 代码检索稠密优势再思考

- **论文名称**：**[Embeddings vs. Grep: Rethinking the Dense Advantage in Code Search](https://arxiv.org/abs/2611.20455)**
- **核心亮点**：
  - **任务定义**：检验代码检索基准上稠密检索对 BM25 的优势从何而来、有多大、哪些查询携带它，属仓库级代码检索评估领域。
  - **方法核心**：构建 RepoPremium 基准（91 仓库、2022–2026 PR）并做受控干预：修正 BM25 分块（整文件索引）、统一标签规则、LLM 生成标识符扩展查询（generate-then-grep）、RRF 融合、预注册 lockbox 验证。
  - **评估指标**：curated premium 从 +0.121 降至 +0.034（整文件 BM25）；generate-then-grep 使 BM25 在所有 Python 池追平或超过稠密检索；RRF(dense, gen.) lockbox 0.578 vs dense 0.533；premium 集中于查询缺少代码稀有标识符的情形（τ 最低四分位 +0.217 → 最高四分位 +0.050）。
  - **为何优于 baseline**：表面"稠密优势"来自评估设置——BM25 复用稠密检索的 1000 字符分块削弱自身、把编辑测试计为相关奖励稠密检索的测试偏好；修正后 curated premium 几乎消失；剩余优势集中于词汇缺口，LLM 补写标识符主要帮 BM25（+0.12~0.21）因为词法索引可精确匹配标识符。
- **团队背景**：蚂蚁集团（企业产出）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.20455)；[💻 代码仓库](https://github.com/hq-bench/repo-premium)

#### 速览 7：SkillBridge / Agent 技能跨 OS 兼容缺陷修复

- **论文名称**：**[SkillBridge: Cost-Aware Diagnosis and Repair of Cross-OS Compatibility Defects in Agent Skills](https://arxiv.org/abs/2611.26256)**
- **核心亮点**：
  - **任务定义**：诊断与修复 agent skill（混合文本-代码-包工件）的跨操作系统兼容缺陷——同一 skill 在部分 OS 通过、在目标 OS 失败，属软件工程/agent skill 质量领域。
  - **方法核心**：基于合约的三阶段工作流——把 skill 形式化为合约（指南/包面/元数据/依赖/OS 假设），静态分析（34 条规则）产出证据做预算内选择，动态验证（三 OS runner），根因评审（Issue/Dev/Review 三 agent 分工）给标签后修复编辑合约字段并重放。
  - **评估指标**：1,090 个 skill×3 OS=3,270 案例：175 个确认 OS 问题（Windows 108 最多）；修复重放通过率 30.6%→38.4%（+7.8pp）；效率：185 次动态验证找到 38 个问题 vs 随机约 865 次（省 $40.6、89.5 agent 小时）。
  - **为何优于 baseline**：把跨 OS 失败从"任务 pass/fail 位"重构为"合约假设→运行症状→评审标签→修复"证据链：静态分析按设计是召回导向排队器（高召回保证预算内不漏），动态执行与评审才定标签；修复直接编辑评审定位的合约字段而非压制作弊任务。
- **团队背景**：北京大学（高可信软件技术重点实验室）+ 中关村学院 + 西北工业大学，国家重点研发计划资助。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.26256)；[💻 代码仓库](https://github.com/TankTechnology/SkillBridge)

#### 速览 8：Self-Replicating Language Models / 自我复制的语言模型

- **论文名称**：**[Self-Replicating Language Models](https://arxiv.org/abs/2611.25839)**
- **核心亮点**：
  - **任务定义**：检验语言模型能否作为达尔文式复制者——以可遗传变异繁殖、经选择改进，属人工生命/AI 安全领域。
  - **方法核心**：构建"盒子"沙箱测试多种复制模式：基因组字符串既作训练子代模型的程序又作训练数据（冯·诺依曼双用途）；带 self-mutator 的爬山者谱系（变异集中于控制器注释）；权重即基因组；真实 LLM 以采样语料+KL 蒸馏"教"子代。
  - **评估指标**：最小祖先（1,791B）3 代均有存活；9 个旗舰模型中 5 个+2 个开源写出可实现活孙代的基因组，本地 ≤30B 4bit 全部失败；群体实验 16 代 copy fidelity 5.5%→92%、fitness 约 1→20；block-catch 谱系 107 步满分控制器；权重基因组 p=10⁻⁴ 时 75 群体灭绝、12 群体"冻结"（进化出把突变率降为零的非突变者）；教学模式 2B token 子代恢复约 1/3 父代基准余量。
  - **为何优于 baseline**：因果链按复制模式展开——self-mutator 把变异集中于控制器注释（模拟病原菌 contingency loci）→变异不再破坏复制器→进化出满分控制器且保真保持；权重基因组单一盲突变算子同时扰动复制器与控制器→meltdown 或冻结；结论：小 LM 可低成本进化为窄 niches 机器（含安全风险），大型模型不在该路径上。
- **团队背景**：Dalarna University（瑞典）+ MSU BEACON 进化研究中心。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25839)

#### 速览 9：From Code Review Comments to Reuse in Production / 代码评审规则的生产追踪

- **论文名称**：**[From Code Review Comments to Reuse in Production: An Industrial Study of Extracted Rules](https://arxiv.org/abs/2611.25918)**
- **核心亮点**：
  - **任务定义**：考察从代码评审评论中提取的规则部署到生产后实际发生了什么、部署时属性能否预测下游使用，属工业实证软件工程领域。
  - **方法核心**：对 GA technologies 自研 LLM 评审系统 Kleene 做 65 天生产追踪：四阶段漏斗（部署→被点名→呈现→记录代码变更）+ 留一仓库交叉验证逻辑回归 + 逐条 clause 追踪与三条件对照实验（无知识/原始评审线程/提取规则）。
  - **评估指标**：464 条部署规则中 259（55.8%）被点名、94（20.3%）有记录代码变更；最大流失在点名之前（44.2% 从未被点名）；906 条候选被 reviewer 自行压制（50.4%）；20 个构造案例上：无知识 3/10、原始线程 8/10、提取规则 10/10 检出。
  - **为何优于 baseline**：证据链设计定位流失位置（null 模型预测 28.0% 未点名、实测 44.2%）；固定评审任务、只变知识块（字节级一致）分离"知识内容"与"检索"——核心教训是归因计数与 pooled 复用率都不能作为规则价值的证据。
- **团队背景**：筑波大学 + GA technologies（日本房产科技）+ Institute of Science Tokyo——企业出系统/数据/一线作者，高校出方法论。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25918)

#### 速览 10：Can an LLM Fix a Language Model Without Training It? / LLM 能否不训练修复语言模型

- **论文名称**：**[Can an LLM Fix a Language Model Without Training It?](https://arxiv.org/abs/2611.25613)**
- **核心亮点**：
  - **任务定义**：在受控注入故障的 Transformer 上，检验外部 LLM 交互式选择数值投影修复（不训练目标模型）相对固定投影/脚本搜索/黑盒搜索/LoRA 的价值，属模型修复/权重空间编程领域。
  - **方法核心**：Live LLM 编辑环境——损坏目标模型经本地 HTTP 服务暴露检查/提案/应用/评估/回滚工具；修复算子为共享数值投影 W_r=W_d−αUU^T W_d M（方向配方：位置对比/权重 SVD/混合）。
  - **评估指标**：核心故障五目标 A 分 63.02–76.56；Pythia 上 Live 65.62% vs 贝叶斯黑盒 0.00%；Qwen 会话内反馈实例：首候选 6/64→换权重 SVD 方向 49/64（78.12%）；复合故障：MLP-only 25.31% vs LLM 选择 71.25%；repair+LoRA 500 步 94.5% vs 损坏 LoRA 82.9%。
  - **为何优于 baseline**：增益分解为两个被匹配对照隔离的因子——开发反馈改变配方选择（同层同支持、只换方向配方 12.5%→78.12%，价值在选择而非新算子）；算子覆盖宽度（复合恢复来自算子族+选择而非 LLM 特有智能）；固定位置投影是强基线，核心故障上 Live 无普遍优势——负结果同样有价值。
- **团队背景**：独立研究者（Hyderabad）+ Amazon Seattle。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2611.25613)

### 顶会标准评审简表

| 论文 | 新颖性 | 触发精读 | 一句话理由 |
|------|--------|----------|-----------|
| Rewrite Own Weights (2611.21885) | 高 | ✅ | 自我权重编辑的规模涌现（27B 成功/4B-9B 自毁）是新范式级发现 |
| CPU Inside an LLM (2611.26450) | 高 | ✅ | 零梯度闭式编译精确电路+NL 非劣效，权重编程的机制极限之作 |
| Weight-Space Programming ERT (2611.26408) | 高 | ✅ | "训练网络使其可被 LLM 编辑"开辟可编辑性训练新问题 |
| Mind2Act World (2611.26444) | 高 | ✅ | Mind/Act/组合解耦诊断是具身评测的新问题定义，13 校联盟 |
| WeVisForensics (2611.21373) | 高 | ✅ | 调查者-专家库双时间尺度共演化，检测 +9.0pp，南大×腾讯 |
| WM^MLLM (2611.24193) | 高 | ✅ | 状态外置+构造性保证三性质，世界模型表示革命，Princeton×NVIDIA |
| AutoMaster (2611.26510) | 高 | ✅ | Agent 设计行为干预塑造教师，自主后训练新范式 |
| System 1 agents (2611.21053) | 高 | ✅ | 受限选项单选构造性消除幻觉，0/10,888 虚构 |
| SWE-Recreation (2611.26311) | 高 | ✅ | 主动黑盒应用重建是新任务定义，ECov/D2I 分离失败模式 |
| Meta-RTL (2611.25891) | 高 | ✅ | Harness 自进化在 EDA 域未见泛化 +91.6% |
| System-One RSI (2611.26007) | 高 | ✅ | 推理模型当教练 LoRA 决策模型，商业游戏实时闭环新范式 |
| Unfaithful Reports (2611.25760) | 高 | ✅ | 报告忠实性独立于任务成功的新问题+93.06% 检测器，北大×微软 |
| CompAct-Bench (2611.25673) | 高 | ✅ | 过程级压缩评测揭示状态存放位置决定压缩命运 |
| GrepMem (2611.25123) | 高 | ✅ | 压缩与召回解耦的简单机制大幅胜出，五任务 Pareto 前沿 |
| MC-Lens (2611.26590) | 中高 | 日报覆盖 | J-Lens 多模态扩展扎实但属方法学增量 |
| AltRA-Test (2611.26073) | 中高 | 日报覆盖 | 统计方法学帕累托占优，NLP 评测基础设施 |
| LightningMoBA (2611.26268) | 中高 | 日报覆盖 | 系统三轴共设计 1.2-1.6×，工程创新为主 |
| Twice-Told Critiques (2611.26286) | 中高 | 日报覆盖 | 科学计量新发现但单人+LLM 写作，因果留开放 |
| Phantom Success (2611.25179) | 中高 | 日报覆盖 | 基准审计新问题，61 缺陷实证充分 |
| Embeddings vs. Grep (2611.20455) | 中高 | 日报覆盖 | 评估洞见强（稠密优势是评估伪影），负结果价值 |
| SkillBridge (2611.26256) | 中高 | 日报覆盖 | Skill 跨 OS 兼容新问题，+7.8pp 修复 |
| Self-Replicating LM (2611.25839) | 中高 | 日报覆盖 | 人工生命视角新问题，安全启示明确 |
| Code Review Rules Reuse (2611.25918) | 中 | 日报覆盖 | 工业实证扎实但方法学为追踪分析 |
| LLM Fix LM (2611.25613) | 中 | 日报覆盖 | 对照严格的负结果研究（价值在选择不在算子） |

---

### 2. 产业动态与产品创新（AI Hot Skill 精选）

#### 新闻 1：Thinking Machines 发布开源权重循环架构模型 Inkling 2 Mini

- **事件/产品名称**：**Inkling 2 Mini（Thinking Machines Lab × Hugging Face）**
- **核心内容**：开源权重循环 Transformer：中间 12 层每 token 运行三次（循环三次的深度换参数效率），总参数 26.4B、激活 3.4B 的 MoE 架构，支持文本+图像输入、1M token 上下文窗口，内存占用约 20GB，Apache 2.0 许可；vLLM 在 Blackwell/Hopper/DGX Spark 提供 Day-0 支持；Goodfire 同步发布可解释性分析（24 层、attention residuals + 中间块循环三次架构的内部机制）。Hugging Face 联合发布面向本地部署的多模态版本（总参 26B、激活 3B）。
- **落地应用场景**：32GB 内存以上的 MacBook/DGX Spark 本地运行多模态助手；循环架构在同等参数量下换取更深的有效计算，适合边缘侧长上下文文档分析与代码理解；对研究者的权重开放+Goodfire 机制分析组合，是研究"循环深度 vs 能力"的直接素材。
- **相关链接**：[🌐 官方发布](https://thinkingmachines.ai/news/inkling-2-mini/)；[🌐 HF 博客](https://huggingface.co/blog/inkling2mini)；[🌐 Goodfire 机制分析](https://www.goodfire.com/research/interpreting-inkling-2-mini)

#### 新闻 2：browser-use 1.0 发布——用一个 Python REPL 替代 24 个浏览器动作

- **事件/产品名称**：**browser-use 1.0**
- **核心内容**：架构大瘦身：用单一 python 持久 REPL + 原生 Chrome DevTools Protocol 访问替代 0.x 的 24 个浏览器动作；该 PR 新增 29,601 行、删除 136,905 行，178 个 Python 文件减到 43 个；删除 13.7 万行自有代码后 BU Bench v2.1 得分反而上升。
- **落地应用场景**：浏览器自动化 agent 的开发范式从"枚举工具"转向"给模型一个通用解释器"——通用性来自 Python 表达力而非工具清单长度；对构建个人助理/RPA 的团队意味着更小的维护面与更灵活的组合操作（循环、条件、等待逻辑直接写在 REPL 里）。
- **相关链接**：[🌐 发布公告](https://browser-use.com/posts/browser-use-1-0)；[🌐 设计复盘](https://browser-use.com/posts/simplicity)

#### 新闻 3：Browser Use 评测方法论连环复盘——判题 bug 吞掉一半提升

- **事件/产品名称**：**LLM-as-a-Judge 评测假阳性复盘**
- **核心内容**：复盘一次 agent 评测中的 LLM judge bug：判分器只读 FINAL ANSWER: 标记后的文本，200 个任务中位数只读 2,102 字符中的 433 字符——修复评测适配器后测得 +5.1 分提升（95% CI [+1.0, +9.2]），但其中 1.8 到 2.6 分来自判题缺陷；另一实验：六个智能体配置读同一藏有 PINEAPPLE 注入句的网页只有 1 个照说、3 个标记为注入，同一页面 15 个 codeword 中截图+accessibility tree 找到最多。
- **落地应用场景**：所有用 LLM judge 做 agent 评测的团队（SWE-bench 类、web agent 类）都需要核查"判分器实际读了什么"——答案截断、标记依赖、部分读取都会系统性扭曲提升幅度；个人智能体浏览器的提示注入防护设计也有直接参考（七项需求清单：持久登录、借用 cookies、反屏蔽、人工接管等）。
- **相关链接**：[🌐 判题 bug 复盘](https://browser-use.com/posts/llm-as-a-judge-eval-bug)；[🌐 个人智能体浏览器七项需求](https://browser-use.com/posts/personal-ai-agent-browser)

#### 新闻 4：浏览器 Agent 提速 40% 与反拦截实战

- **事件/产品名称**：**Browser Agent 速度优化与拦截对策**
- **核心内容**：航班搜索任务从 30.7s 优化到 20.7s（本地 Chrome 同任务 17.6s）：修复项包括模型 service tier、跳过代理直连、关闭常驻验证码检测器、精简 prompt、缓存工具列表；$100 悬赏测试 82 个网站中 7 个彻底拦住其 Agent（均为当时不支持的验证码类型）、6 个换当地代理后放行；行为传感器检测下，JavaScript 聚焦输入框 11 次中 11 次通过。
- **落地应用场景**：Web agent 产品的工程优化清单——延迟（用户直接感知）与封锁率（可用性）是浏览器 agent 商业化的两大硬门槛；对反爬场景（机票比价、电商监控）给出可操作的国别代理与拟人输入策略。
- **相关链接**：[🌐 提速复盘](https://browser-use.com/posts/faster-browser-agent)；[🌐 反拦截手册](https://browser-use.com/posts/agent-stealth-playbook)

#### 新闻 5：Google 推出 Semantic governance policies——工具调用执行前语义审查

- **事件/产品名称**：**Gemini 平台 Semantic governance policies（Preview）**
- **核心内容**：通过 Agent Gateway 和 LLM judge 在每个工具调用运行前检查两项：是否符合用户原始意图、是否符合自然语言编写的策略；配套发布 Agent Anomaly Detection 私测扩展至监测运行在 Google Cloud 之外的智能体；另有企业指南：用 Egress Agent Gateway + Private Service Connect 把内网 Cloud Run MCP 服务器安全接入 Gemini Enterprise、自定义 UI 三层认证处理。
- **落地应用场景**：企业 agent 上线前的最后一道闸门——策略用自然语言编写（如"不得向外部邮箱发送客户 PII"）而非硬编码规则，LLM judge 在执行时点拦截语义违规；跨云监测适配多云部署的 agent 资产盘点与异常检测。
- **相关链接**：[🌐 官方博客](https://developers.googleblog.com/check-what-your-ai-agents-do-before-they-do-it-with-semantic-governance-policies/)；[🌐 跨云异常检测](https://developers.googleblog.com/agent-anomaly-detection-safeguarding-agents-across-other-clouds/)

#### 新闻 6：METR 能力测量更新——AI R&D 能力每 3.6 个月翻倍

- **事件/产品名称**：**METR 2026 年 11 月能力测量更新**
- **核心内容**：截至 Opus 5.5 和 GPT-6 Astra 的评测：TH1.1 与 TH2 任务上模型时间视野约每 3.6 个月翻倍，可验证子集翻倍时间约 1.65 个月；同时警告 AI R&D 相关能力"更难测量"——任务饱和与污染风险上升。
- **落地应用场景**：AI 安全与政策机构的核心趋势数据（ timelines 校准）；对企业研发规划意味着自动化研发助手的可用任务边界每季度翻一番，需动态调整人机分工与技能投资。
- **相关链接**：[🌐 METR 报告](https://metr.org/blog/2026-11-20-q4-2026-capability-update/)

#### 新闻 7：Apple 发布 Depth Pro 2——单目深度估计三指标新 Pareto 前沿

- **事件/产品名称**：**Depth Pro 2**
- **核心内容**：单个可扩展网络同时处理图像与视频，原生分辨率下预测以米为单位的稠密深度；同时实现度量准确、边缘锐利与帧间时间一致，三项指标均超前方法并定义新的精度-锐度-一致性-运行时间 Pareto 前沿；代码与权重开源。
- **落地应用场景**：AR/VR 场景重建、机器人感知、视频特效（深度敏感的抠像与虚化）——一个网络替代此前"图像模型+视频模型"两套管线的工程成本。
- **相关链接**：[🌐 Apple ML Research](https://machinelearning.apple.com/research/depth-pro-2-monocular)

#### 新闻 8：OpenAI 发布 MedChemBench——药物化学能力评测

- **事件/产品名称**：**MedChemBench**
- **核心内容**：99 道源自已发表研究的题目评估 AI 模型解读分子结构、解释实验证据并预测化合物性质的能力；xhigh 推理设置下 GPT-6.2 Astra 获 84.5% 可用分数、GPT-6 Astra 83.3%。
- **落地应用场景**：药物化学家的 AI 助手能力标尺——分子结构解读与实验证据推理是临床前药物设计的日常动作；对 AI 制药公司（Recursion、Exscientia 类）用于内部模型选型与人类专家对照。
- **相关链接**：[🌐 OpenAI 发布页](https://openai.com/index/introducing-medchembench)

#### 新闻 9：Claude Computer use 开放 Team 与 Enterprise 套餐

- **事件/产品名称**：**Claude Computer use beta 扩展**
- **核心内容**：Computer use beta 面向 Team 和 Enterprise 套餐开放并逐步推送；Claude 可查看用户屏幕并在其允许的桌面应用中操作，功能在 Claude Desktop 新体验中可用，也支持开启 Computer access 的 Claude Code。
- **落地应用场景**：企业桌面自动化（跨应用数据搬运、表单填写、遗留系统操作）——从开发者尝鲜走向组织级采购，意味着计算机使用类 agent 进入合规与审计视野。
- **相关链接**：[🌐 官方说明](https://support.claude.com/en/articles/12138966-release-notes#h_c2590a46af)

#### 新闻 10：Gemini 4 四款新模型怎么选

- **事件/产品名称**：**Gemini 4 Argon / Carbon / Hydrogen / Neon**
- **核心内容**：Google DeepMind 五周多为 Gemini 阵容新增四款以元素命名的模型（三款九天内接连发布）；分析各模型定位差异与选用建议，尤其何时该选 Neon。
- **落地应用场景**：应用开发者的模型选型参考——快速迭代的模型矩阵意味着"默认用旗舰"策略不再成本最优，需按任务（推理深度/延迟/多模态）匹配元素系列。
- **相关链接**：[🌐 选型分析](https://every.to/vibe-check/mini-vibe-check-four-new-geminis-when-should-you-pick-neon)

#### 新闻 11：Anthropic 与 SpaceX 的 10x 估值之墙

- **事件/产品名称**：**史上最大两笔 IPO 的估值对比**
- **核心内容**：Anthropic 与 SpaceX 市值分别为 $1.56t 与 $1.82t，远期市销率同为 9.5x，尽管 SpaceX 预期增速 466% 快于 Anthropic 的 288%；分析指出万亿美元级公司远期市销率不超过约 10x——估值实际上限由宏观流动性而非增长决定。
- **落地应用场景**：AI 行业投研与战略的估值锚——对创业者意味着在 10x 墙内规划融资节奏与收入兑现，对投资人提供了"增长再快也难破墙"的跨行业基准。
- **相关链接**：[🌐 分析原文](https://tomtunguz.com/the-10x-wall/)

#### 新闻 12：OpenClaw 澄清开源并不免费——12 万小时社区投入

- **事件/产品名称**：**OpenClaw 开源成本估算**
- **核心内容**：回应学生"项目为何免费"的提问：开源不收费但由社区以时间支付，估算约 120,000 小时人力投入（接近 58 年全职工作量），包含 3,606 位贡献者、83,000 个 fork、279 次发布的开发、审查、issue 处理、文档与社区支持。
- **落地应用场景**：开源治理与可持续性讨论的量化素材——企业采用开源 agent 框架时应把"上游维护能力"纳入风险预算（贡献回馈、赞助或雇佣维护者），避免白嫖断供。
- **相关链接**：[🌐 官方博客](https://openclaw.ai/blog/openclaw-isnt-free)

#### 新闻 13：Google 员工联署反对 Gemini 学校政策

- **事件/产品名称**：**Alphabet Workers Union 请愿**
- **核心内容**：数百名 Google 员工提交 petition，要求撤回"对关闭 Gemini AI 工具的学校切断 Search 访问"的政策；该政策在选举日公布后纽约、洛杉矶、芝加哥等地爆发抗议，三大校区讨论更换搜索引擎。
- **落地应用场景**：教育科技采购与政策风险案例——AI 产品捆绑条款（用 AI 换基础服务）引发的组织内部分歧与公关反噬，对 To G/To 校产品的合规设计是直接警示。
- **相关链接**：[🌐 新闻报道](https://www.motherjones.com/politics/2026/11/google-alphabet-union-school-ai-protest-gemini-workers-petition/)

---

**数据边界说明**：本日报覆盖北京时间 2026-11-20 00:00–24:00 全天。学术侧为 arXiv Fri 20 Nov announce 批次（893 条）与 Hugging Face 2026-11-20 每日榜（46 篇）；产业侧为 AI HOT 27 条新闻（Browser Use 系列博客按主题合并呈现）。11-20 为周五，arXiv 批次量 893 条属正常水平。
