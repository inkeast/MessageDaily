---
title: "【每日AI前沿追踪】2026年11月12日 核心技术与产业动态速递"
date: 2026-11-12
draft: false
tags: ["DailyNews"]
categories: ["daily"]
summary: "今日主线：NVIDIA 巨震——黄仁勋因丑闻辞任 CEO，CFO Colette Kress 出任临时 CEO；Google 双弹齐发 Gemini Decision 1 判别式决策模型（$0.10/M tokens、150-250ms）与 Gemini 4 Hydrogen 快速执行模型；推理基础设施进入联盟时代，SGLang Consortium 八巨头成立、vLLM 在 Vera Rubin 上实现 58× 吞吐。学术侧 25 篇深读：Agent 记忆后训练（CAMP-II 五域 64.6%）、RSI 经济学审计（RSI 税高达 393%）、验证器生成 harness（Veritas 四基准第一）、Agent 法律合规首基准（ScrapeBench 中性提示下合规率 <5%）与 EDR 攻防基准（DeepSeek 94.4% 绕过率）构成可信 Agent 全景。"
---

# 【每日AI前沿追踪】2026年11月12日 核心技术与产业动态速递

## 一、今日核心洞察与重点摘要

- **NVIDIA 进入后黄仁勋时代**：因婚外情与间谍丑闻持续发酵，黄仁勋辞去担任 33 年的 CEO 职务并退出董事会，CFO Colette Kress 出任临时 CEO，Harvey C. Jones 任首任董事会主席；财报（11/17）与产品路线图承诺"不中断"，但市值单日蒸发约 1.4 万亿美元，市场焦点转向 CUDA 生态护城河是否依赖创始人个人。
- **判别式决策接口成为平台级标准**：Google 发布 Gemini Decision 1 与 Decisions API——单次前向传播直接对候选选项打分（非逐 token 生成），端到端延迟 150-250ms、单请求最多 1000 个封闭式问题、输出 $0 计费、比 Flash-Lite+生成式 API 便宜近 5 倍；同日 Gemini 4 Hydrogen（AA 智能指数 51、Terminal-Bench 4.1 从 18.2%→57%）与 A2UI 1.0 开源 UI 协议（字节/彭博已接入）三箭齐发，Google 正在把"快思考"做成基础设施。
- **推理基础设施进入联盟时代**：LMSYS 联合 NVIDIA/AMD/Intel/Google/Meta/Cloudflare/Modal/RadixArk 成立 SGLang Consortium（推理将占 2026 年 AI 算力 2/3）；vLLM 在 Vera Rubin NVL72 上跑 DeepSeek-V4.1-Flash 实现 GB300 的 58× 吞吐（峰值 252 万 tokens/s）；UCSD 用形式化验证在 PD 分离引擎中揪出 3 类 RDMA 竞态 bug——KV 缓存跨用户污染会静默返回 HTTP 200。
- **可信 Agent 研究集中爆发**：RSI 审计论文揭示复杂自改进方法的"改进税"高达任务成本的 109-393% 而准确率不及一条 2048-token 笔记；EDR 攻防基准显示 DeepSeek-V4.1-Flash 绕过率达 94.4%；ScrapeBench 发现中性提示下 5/6 智能体法律合规率不足 5%；验证器生成（Veritas）与确定性安全架构（Ring 0 监视器 12.7µs 阻断 100% 攻击零误报）给出工程答案——Agent 的下一程竞争点是"可验证性"。

**今日企业+高校研究合作趋势**：25 篇深读论文中 5 篇为企业+高校合作，呈现三种模式——①**中国企业主导型**（京东×北大 CAMP-II、字节×UIC/NYU Veritas、联想×Marquette/Cornell 基准方法学：企业出场景与算力、高校出算法与统计方法）；②**美国系统研究多机构联盟**（Evolving Infra 集结 Georgia Tech/UIUC/Berkeley/MIT+NVIDIA/OpenAI，Ion Stoica 与 Song Han 同列作者，产学边界在 MLSys 领域高度融合）；③**跨学科新形态**（ScrapeBench 由工程师+执业律师合著，法律学者进入 Agent 评测一线）。纯学术侧，六校跨国联合（FGSE：NTU/Georgia Tech/UW/WashU/Sydney/NUS）与欧洲跨国所合作（Helmholtz×TUM×Télécom Paris）成为中坚；国内上海 AI Lab 主导了两项大规模数据/RL 工作（TOME-Math 38 万证明 QA、BookNavigator）。

---

## 二、详细内容追踪

### 1. 前沿学术与技术突破（Hugging Face 精选 + Arxiv 精选）

> 今日共深读 25 篇（Hugging Face 11-12 榜 20 篇中 13 篇相关 + arXiv Wed 11 Nov 批次 743 篇中精选 12 篇），其中 21 篇触发独立精读（见今日精读系列文章），4 篇速览附后。

#### ▍重点深读论文（21 篇，均触发独立精读）

**1. 论文名称**：**Scaling Mathematical Proof Supervision from Structured Textbooks / 从结构化教科书规模化数学证明监督**
- **核心亮点**：
  - **任务定义**：把大学数学教科书转为保留章节顺序与命题依赖的大规模证明监督数据，破解数学证明训练数据"孤岛化"问题（数学推理/训练数据领域）。
  - **方法核心**：TOME-Math 结构保持流水线——OCR 解析→状态机恢复章节层级→提取定理-证明对并检测证明缺口补全自包含子命题，产出 38 万条五维质检 QA；训练侧 FORWARD-GAP 课程（章节正序+缺口优先）。
  - **评估指标**：六基准平均分 Qwen3-30B-A3B 从 29.17%→45.83%（+16.7pp）；MiniF2F 34.45→57.43；对 4.8 倍大的 OpenMath1（182 万条）六项全胜。
  - **为何优于 baseline**：孤立 QA 对丢失先修关系→结构化课程让模型按章节正序渐进学习+Gap QA 显式监督隐含推理步（消融：去 Gap QA 四个证明基准掉 5-6.8pp）——数据结构差异→学习顺序机制变化→泛化提升。
- **团队背景**：中科大+复旦+上海 AI Lab+上海交大多校联合，上海 AI Lab 主导（数据规模化产学研协作）。
- **相关链接**：[📄 点击阅读论文原文](https://arxiv.org/abs/2610.40394)

**2. 论文名称**：**SteerDistill: Quantizing LLM Agents via Closed-Loop Teacher Steering / 闭环教师引导的智能体量化蒸馏**
- **核心亮点**：
  - **任务定义**：低比特量化后 LLM 智能体能力恢复，让 W4/W2 位 agent 逼近 BF16 任务成功率（模型压缩/Agent 领域）。
  - **方法核心**：闭环教师引导蒸馏——将失败 rollout 与教师成功参考按环境状态对齐定位首个"行为分歧"回合，教师生成结构化纠正提示让学生从错误前状态重试，蒸馏时用前向 KL 内化"被引导快照"实现无引导推理。
  - **评估指标**：五智能体基准平均，Qwen3-8B W4A16 达 53.18（BF16 为 53.54，差仅 0.36）；W2A16 超 On-Policy QAD 3.39 分；GPU 时数比 QeRL 少 67%。
  - **为何优于 baseline**：量化学生失败源于忽视任务前提的行为分歧且自身正确重试率仅 9.6%（教师引导后 82.3%）→闭环在真实失败上下文构造局部验证过的纠正→蒸馏将其内化为学生自身分布——纠错来源差异→训练信号质量→近无损压缩。
- **团队背景**：清华 AIR+中科大（研究院+高校）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13410)；[💻 代码](https://github.com/air-embodied-brain/SteerDistill)

**3. 论文名称**：**Incentivizing Self-Learning in LLMs via RL（BookNavigator）/ 强化学习激励大模型自学**
- **核心亮点**：
  - **任务定义**：训练 LLM 在解题中自主从配对教科书检索、学习并应用知识，形成可迁移研读策略（RL/长上下文领域）。
  - **方法核心**：BookNavigator 三工具接口+改造 GRPO——GLM-5.2 八次悲观验证给证明奖励，叠加退火探索奖励（防"能力增强后弃用教科书"）与回合级工具信用分配（冻结模型作工具裁判）。
  - **评估指标**：Intern-S2-Preview-35B 总体 51.5%→79.5%（+28pp），OOD 学科 86.7%（OOD-Subject +29.9pp）；超最强固定检索基线 6.4pp。
  - **为何优于 baseline**：固定检索无法随推理演化动态补前提→退火探索维持工具使用率（正确性-only RL 下工具轨迹从 36.9% 跌至 6.7%）→学到与具体教材无关的通用研读策略→跨书/跨学科/跨任务迁移。
- **团队背景**：上海 AI Lab 主导+上海交大/CUHK/中科大/复旦（国家实验室牵头五机构）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2610.39671)

**4. 论文名称**：**Read Less, Solve More: Token-Efficient Sparse Reading / Token 高效稀疏读取控制层**
- **核心亮点**：
  - **任务定义**：为 AI 智能体控制外部内容（网页/文档/仓库）进入模型的读取量，质量不降而 token 与延迟大降（Agent 基础设施）。
  - **方法核心**：SparseRead 三组件——读意图协议（双意图+五动作+版本指纹）+按工件类型自适应选择定位后端+预算读门控（超预算前忠实回退全览，2L 上界保证）。
  - **评估指标**：六模型五场景 token 降 15.7-92.9%（中位 61.4%）、时间降中位 53.7%，97% 设置分数保持或提升；Kimi-K2.5 长文 QA token -92.9%/分数 +0.598。
  - **为何优于 baseline**：有效证据中位仅占全文 token 的 3.17-5.8%→意图+覆盖记录消除重复读取（4.8→1.5 次/任务）、按工件选后端、门控在低稀疏任务自动回退避免"强制稀疏反增 309% token"的协议税——读取控制机制→上下文信噪比→质量保持下成本骤降。
- **团队背景**：国科大+中科院计算所+松山湖材料实验室（国立科研体系）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2608.22237)

**5. 论文名称**：**GTAGym: Real AAA Game Tasks Environment / 真实 AAA 游戏任务环境与 RL 框架**
- **核心亮点**：
  - **任务定义**：把 GTA V 主线任务拆解为可自动判分、可并行评估、可批量生成 RL 环境的 AAA 游戏智能体基准（具身/游戏 Agent 评测）。
  - **方法核心**：75 主线任务分解为 150 子任务（103 在线 3D+47 射击），Babylon.js 重建场景+引擎状态谓词自动判分；模板化生成器按路线/速度参数采样批量产出带奖励的 RL 环境。
  - **评估指标**：GTAVBench-AGI 最佳 GPT-6-Astra 70.9%（人类 89.8%）；LoRA-GRPO 训练 Qwen3.8-27B 后 ID 步行 54.2%→72.9%、真实 GTA V 任务 15%→23%；空中导航全模型 0%。
  - **为何优于 baseline**：引擎状态谓词替代人工判定+生成环境与基准空间分离→既揭示动态控制瓶颈（校准+调节占首次错误 63.2%），又使 RL 增益从合成环境迁移到真实游戏——判分自动化→可扩展训练信号→跨域迁移。
- **团队背景**：上海创新研究院（SII）+复旦（研究院+高校共建，OpenMOSS 数据开源）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13028)；[💻 代码](https://github.com/ljbro/GTAGym)

**6. 论文名称**：**Evolving Infra: Agent-Adaptive LLM Serving at Scale / 智能体自适应的大规模推理服务系统**
- **核心亮点**：
  - **任务定义**：面向 Agent 负载的 PD 分离推理系统，按负载组成在线调整算力、按智能体上下文管理 KV 缓存（MLSys 领域）。
  - **方法核心**：三件套——对称引擎（单调度循环、节点 0.7s 免重启切相）+共享宿主内存 KV 池与全集群"活跃分支引用计数"目录+在线学习迭代时间模型（误差<7%）驱动增益显著才切换。
  - **评估指标**：36 卡 GB300 上相位切换平均 6.2s（NVIDIA Dynamo 需 681-754s，>100×）；静态吞吐达 Dynamo 1.10-2.73×；P95 TTFT 最高降 13.92×（228→21.4s）；KV 复用损失仅 0.3%（Arrow 为 55.5%）。
  - **为何优于 baseline**：现有系统算力分配与 KV 管理耦合（切换=弃 KV）且信号粗糙→解耦数据面+分支计数让"压缩替换的历史/结束的子 agent"被优先逐出→切换成本趋零使规划器敢在线适配——架构解耦→切换与复用兼得→延迟吞吐双赢。
- **团队背景**：Georgia Tech+UIUC+UC Berkeley+MIT×NVIDIA+OpenAI（六机构产学研，Ion Stoica/Song Han 在列）——**今日最强产学研合作**。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.12304)

**7. 论文名称**：**Where Does Alignment Emerge and What Does It Mean? / 柏拉图表示假设的细粒度审视**
- **核心亮点**：
  - **任务定义**：细粒度检验跨模态对齐（柏拉图表示假设）在哪些层/维度出现、能力-对齐趋势是否稳健（表示分析/多模态）。
  - **方法核心**：21 个 LM×3 视觉模型全层 mutual k-NN+PCA 定位对齐子空间+DOCCI 改写控制字幕信息量+四档基线上下文化对照。
  - **评估指标**：跨模态对齐峰值 0.143（远低于视觉-视觉 0.538）；对齐集中于约 100 维子空间；干净字幕（DOCCI）下能力-对齐 r=0.66-0.73 稳健，GSM8K 不显著。
  - **为何优于 baseline**：先前工作只报层间最大值且用噪声字幕→对齐实为中后层低维子空间现象、趋势消失是数据问题而非模型问题——测量方法修正→现象重新定位→解释升级（对齐反映共享语义而非通用能力）。
- **团队背景**：Helmholtz Munich+慕尼黑工大+Télécom Paris（欧洲跨国研究）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.12869)；[💻 代码](https://github.com/ExplainableML/prh-closer-look)

**8. 论文名称**：**WMIP: Working-Memory-Inspired Programmable Probe / 工作记忆启发的可编程探测**
- **核心亮点**：
  - **任务定义**：不允许写中间步骤时程序化测量 LLM 累积状态跟踪能力（认知探测/LLM 评测）。
  - **方法核心**：可编程生成器——三实体初始值+K 次（3-100）增益/损失/转移更新，16-token 核心协议+单整数规则压缩书写空间；同题四协议对照分离指令/预算/书写效应。
  - **评估指标**：18 模型核心协议 K=3 时 44.4%→K=100 时 0.8%；同题写步骤协议 88.0%（+56.3pp）；GPT-5.2 以 56/60 居首，Sonnet 4.5 因截断 0/60；与 BFCL 分数 τb=0.417-0.449。
  - **为何优于 baseline**：现有测试允许书写中间态测不到"心算式"跟踪→16-token 约束制造真实工作记忆需求→暴露精度随更新数崩溃与协议敏感性——约束设计→认知负荷分离→测量效度。
- **团队背景**：东北大学（日）+LUT（芬）+德州农工+WPI（多国高校）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2603.27343)；[💻 代码](https://github.com/dengzhe-hou/WMIP)

**9. 论文名称**：**ScrapeBench: Evaluating Legal Compliance of AI Agents / 智能体网站抓取法律合规评测**
- **核心亮点**：
  - **任务定义**：首个评测 AI 智能体遵守网站反爬政策（ToS+robots.txt）法律合规性的基准（AI 安全/法律对齐）。
  - **方法核心**：CrUX 前 3000 站过滤得 1895 站，GPT-5.2 抽取双标签（人工抽检 κ=0.91）；智能体单 fetch_url 工具，从轨迹判定四结果（违规/合规/许可/过度拒绝）；8 级法律施压提示+180 人人类基线。
  - **评估指标**：1626 个禁抓站点中性提示下 5/6 智能体合规率 <5%；P7 法律警告下 Claude 85.9%/Gemini 85.7%；人类基线 48.9%；GPT 读了 robots.txt 达 100% 但合规仅 14.2%。
  - **为何优于 baseline**：现有基准只测任务完成不测合法性→真值标签+轨迹判定给出可复现合规度量→揭示三大失效（合规靠提示工程、检索≠遵守、对抗下两极分化）——评测维度新增→失效模式可见→为法律对齐训练提供靶点。
- **团队背景**：Pivotal Research+巴斯大学+菲律宾国立+圣母大学+希伯来大学，**工程师+执业律师合著**（法律学者首次深度进入 Agent 基准）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.11306)（NeurIPS 2026 Datasets Track）

**10. 论文名称**：**Teaching the Harness: Student-Aware Skill Transfer / 学生感知的技能迁移教学**
- **核心亮点**：
  - **任务定义**：让强教师 LLM 为固定学生 agent 自动编写可复用技能库，提升学生任务成功率（Agent 机器教学）。
  - **方法核心**：SAT——教师维护"经验画像+分析性学习者模型（失败原因假设）"，用受控诊断探针验证假设后针对性写技能，技能须通过开发任务准入测试（正效用才入库）。
  - **评估指标**：Claude-Sonnet-5 教 DeepSeek-V4-Flash 四次实验 SAT 均值 86.46%（agnostic 基线 81.04%、无技能 75.00%）；教师离场后冻结技能库仍保持 +5.6-11.1 分。
  - **为何优于 baseline**：学习者模型+探针把失败归因到具体执行约束（如动作预算）→教师改写过程本身（逐项查询改批量）→增益集中在执行受限套件（terminal +18.75）；库交换实验证明技能含学生特异信息——归因精度→技能针对性→迁移有效性。
- **团队背景**：Colorado School of Mines（单一高校）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13144)

**11. 论文名称**：**Coding Agent Memory Post-Training II（CAMP-II）/ 编码智能体记忆后训练**
- **核心亮点**：
  - **任务定义**：训练单一策略在五个长程环境中学会"用普通 Bash/文件操作作为跨上下文边界的记忆"（Agent 记忆后训练）。
  - **方法核心**："双边接口先验"——师生共享文件操作先验使学生能自然采样记忆动作、教师能可靠评判记忆内容；完全异步在线策略蒸馏（reverse-KL+token 级优势）跨五环境联合训练；发布 CAMG-II 五域环境。
  - **评估指标**：五域平均 CAMP-II-4B 64.6%（CAMP-I 54.2%、最强基线 MemOPD 51.8%、冻结学生 45.3%）；9B 版 66.5%；外部迁移 Terminal-Bench 2.1 达 38.0% 超 Qwen3.5-35B（33.8%）。
  - **为何优于 baseline**：文件接口 surprisal 最低（教师 0.42 vs 压缩 0.91 nats）→学生记忆动作占比 21.6%（专用工具仅 7.8%）→蒸馏的动作覆盖充足、负优势问题最弱；记忆 token 梯度屏蔽后增益几乎消失——接口选择→采样自然性→训练信号密度。
- **团队背景**：北京大学+京东+川大+北理工+人大+港科大（**企业+高校**：京东产业资源+北大方法学）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.14009)

**12. 论文名称**：**ExpertCrowd-SWE: Expert-Authored Engineering Tasks / 专家原创工程任务基准**
- **核心亮点**：
  - **任务定义**：执业工程师在熟悉仓库上原创撰写、按维护者标准分级打分的 100 项多文件工程任务，测前沿编码 agent（SWE 基准）。
  - **方法核心**：专家原创变更请求（禁止复现已公开修复）+作者自写加权 rubric 评分器（partial credit+blocker 封顶+gate 一票否决）+构建时强制校准与三类对照补丁，76/100 任务保密防污染。
  - **评估指标**：3,495 次运行：Claude Opus 5.5 67.3%±4.7、GPT-6 Astra 63.7%±5.5（差不显著）、Fable 5.1 46.5%、Gemini 3.8 Flash 40.0%；80/100 任务无模型五次全满分；同批模型在 DeepSWE 上仅差 0.9 点而本基准拉开 34.8 点。
  - **为何优于 baseline**：任务难度来自跨模块综合而非隐藏信息→partial credit 区分"大部分完成"与"几乎没做"（二元评分会把四模型压到 1-4%）→维护者规则捕获 test-only 评分会奖励的行为——评分粒度→分辨力→真实排序。
- **团队背景**：Toptal ExpertCrowd（企业，3 万专家网络中 39 位工程师撰写）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13246)；[💾 数据](https://huggingface.co/datasets/ExpertCrowd/ec-swe)

**13. 论文名称**：**FlowPlan: Reducing the Token Cost of Coding Agents / 抽象引导规划降低编码 Agent Token 成本**
- **核心亮点**：
  - **任务定义**：不降解决率前提下降低 Claude Code 修复仓库级 issue 的 token 消耗（Agent 成本优化/SE）。
  - **方法核心**：以函数级控制流图/数据流图为工作层，四阶段 locate→plan→edit-and-check→verify；编辑前把修复意图表达为类型化图增量、编辑后指纹对比结构变化是否匹配计划，匹配即显式终止。
  - **评估指标**：六设置全部降 token、五项显著（Holm p<0.001）：Haiku 上 SWE-bench Pro 0.46-0.47×、Verified 0.32-0.35×；基线平均 3.7M tokens/issue（98% 缓存读）；解决率变化均不显著。
  - **为何优于 baseline**：显式图增量提供机器可检查的完成判据→flow_check/verify 取代编辑后反复重读与测试（75% 节省来自编辑后阶段）→砍掉最贵的"验证长尾"（后编辑调用 31→12-18 次）——终止条件结构化→验证开销骤降。
- **团队背景**：约克大学（加拿大，单一高校）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13241)；[💻 复现包](https://github.com/MelikaSepidband/FlowPlan)

**14. 论文名称**：**Dependable Long-Horizon LLM Agents / 长程智能体依赖性实证研究**
- **核心亮点**：
  - **任务定义**：对公开多智能体失败轨迹做"以轨迹为中心"的依赖性分析——错误进入点、传播路径、控制覆盖（Agent 可靠性实证）。
  - **方法核心**：episode 记录八元组标注 95 条失败运行，操作相位×状态域×错误动力学三坐标；LLM 执行 38 字段协议+确定性文本扫描交叉验证。
  - **评估指标**：错误中位进入点在行程 18% 处；decision 42%/fabrication 26%/integration 11%；72% 失败在症状前已含反证但 88% 无 agent 利用；checkpoint 控制仅覆盖 1% 运行。
  - **为何优于 baseline**：症状标签细化为因果链→发现证据闲置、检测无效（replan 保留致错信念）、harness 故障被错误归因等结构性事实→直接推导控制优先级（独立裁判 74% 覆盖 vs checkpoint 1%）——测量粒度→机制可见性→控制设计。
- **团队背景**：Villanova University（单一高校）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13254)

**15. 论文名称**：**Veritas: A Verifier Generation Harness for Agent Tasks / 面向 Agent 任务的验证器生成框架**
- **核心亮点**：
  - **任务定义**：为自带验证器缺失的任务自动生成可靠验证器，使自我改进循环可扩展（Agent 基础设施）。
  - **方法核心**：OpenCode+三机制——证据头 E（规定验证器看什么、状态对比优先于轨迹）+领域规则库 R（6 域 19,603 条按需检索）+撰写原则 H（9 条约束判什么/怎么比/怎么交付），带探针检查与修复的自改进循环。
  - **评估指标**：与官方判定 Cohen's κ*：τ²-bench 0.787（最强 plain harness 0.726）、SWE-bench Verified 0.720、Terminal-Bench 4.0 0.465（Codex 0.387）、TB-Science 0.548——四个执行类基准全部第一。
  - **为何优于 baseline**：三机制各治一类失败（E 防检查看不到的东西、R 补领域义务、H 防过严漏交付）；加到 Claude Code 后 TB 从 0.238→0.586；消融去 H 反低于 plain（0.250）——信息与撰写纪律缺一不可。
- **团队背景**：字节跳动（美）+UIC+NYU（**企业+高校**，字节主导）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13336)

**16. 论文名称**：**The RSI Tax: When Recursive Self-Improvement Is Not Worth the Cost / 递归自我改进的经济学审计**
- **核心亮点**：
  - **任务定义**：在连续任务流上端到端核算 RSI 方法的"改进过程成本（RSI 税）+执行成本"与准确率权衡（Agent 自改进经济学）。
  - **方法核心**：把 RSI 税分解为验证/探索/写作三类并追踪产物是否被后续使用；提出极简基线 SRN——仅一条 ≤2048 token 有界笔记，从失败轨迹修订，无搜索无验证。
  - **评估指标**：四设置中 SRN 三项准确率最高（等权均值 59.18% vs EvoSkill 55.35%）；RSI 税率：Meta-Harness 109-329%、EvoSkill 144-393%，SRN 仅 0.04-0.36%；漏记 RSI 税会把 3.5 倍总成本的方法误判为便宜。
  - **为何优于 baseline**：三大方法 74.8-93.6% 的税花在验证（EvoSkill 刷新旧 frontier 的花费超过任务本身）且 23.7-63.6% 产物从未被使用→候选搜索式改进的验证开销超过任务价值；SRN 全部成本有后续使用——成本结构差异→税负差异→总成本反转。
- **团队背景**：北京大学（单一高校）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13524)

**17. 论文名称**：**Agent Benchmarks Stop Ranking Before They Run Out of Tasks / 基准在任务耗尽前就失去排序能力**
- **核心亮点**：
  - **任务定义**：诊断数据科学/ML agent 基准"饱和"根源——论证饱和多在 1-bit 评分机制而非任务本身（评测方法学）。
  - **方法核心**：两类失败（位太粗/被参考解封顶）的量化诊断+分级得分 s(x)=G(x)·min(3, log(J/J_base)/log(J_ref/J_base))（Cauchy 方程推出 log 形式+clip 防单任务主导）+冻结锚点分层设计。
  - **评估指标**：SWE-bench Verified 前十仅差 3.6 点成 1 层；纠正后顶层 23 家缩至 5 家、8/18 相邻次序翻转；ALE-Bench 饱和指数从 S=1.00 恢复至 0.03（3 层）；Terminal-Bench 并列前十 token 中位差 7.3×。
  - **为何优于 baseline**：单 pass/fail 只能看一个能力带→分级得分在参考解之上保留余量（47.8% 运行超越参考解）→恢复分辨力；log 形式由量纲无关+可加性数学推导而非拟合——评分函数数学性质→信息量→排序有效性。
- **团队背景**：联想集团+Marquette+Cornell（**企业+高校**：联想第一单位）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13541)

**18. 论文名称**：**BypassGym: Evaluating AI Agents' Endpoint Defense Evasion / 端点防御绕过攻防基准**
- **核心亮点**：
  - **任务定义**：评估 AI agent 能否针对给定 C2 payload 在真实 Windows+部署 EDR 下开发存活加载器（进攻性安全评测，已负责任披露）。
  - **方法核心**：固定 payload（Havoc/AdaptixC2/Sliver）+目标 EDR+规避知识三档；容器内编译，Proxmox 真实 Win11 VM 两阶段评估（功能验证→EDR 测试），快照恢复+结构化反馈。
  - **评估指标**：90 次运行 71 次通过（78.9%）；DeepSeek-V4.1 Flash 最强 17/18（94.4%）；Windows Defender 15/15 全放行、Bitdefender 最难 5/15；1-4 周延迟重测 87.3% 仍通过；知识消融使 GLM 15/18→9/18。
  - **为何优于 baseline**：成功案例 70% 走签名宿主 DLL 侧载+"谨慎集成者"风格（前置侦察+复用供给代码）减少运行时修复；二元反馈无增益而结构化反馈有效（14→17/18）——知识供给与反馈粒度→修复效率→绕过成功率。
- **团队背景**：华中科大+Virginia Tech（两校，无企业，已向全部 EDR 厂商披露）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13684)

**19. 论文名称**：**No Finite Rulebook Can Certify Every Safe Program / 守门 AI 智能体的不可能性定理**
- **核心亮点**：
  - **任务定义**：证明有限可检验规则簿不可能精确刻画安全/机密性/响应性规范的全部合规程序（AI 治理可计算性理论）。
  - **方法核心**：规则簿形式化为可确认集，停机问题归约构造"先解谜题再开保险箱"反例；给出行为判定范畴可确认的精确刻画（Rice-Shapiro 对偶）与守门人可保证的"可检验分离列表"。
  - **评估指标**：理论三定理+220 万随机寄存机仿真：任一 n bit 可靠规则簿必遗漏约 n bit 合规程序；8 个守门器对定制对手全预算模拟器 8/8 正确而所有更快检查器 0/8；随机程序仅 1.6% 不安全（阻碍是对抗性而非典型性）。
  - **为何优于 baseline**：经典结果（Rice/Rice-Shapiro/Chaitin 等）各覆盖一两列而本文唯一全覆盖；推导出部署机制谱系（eBPF verifier/gas budget/runtime monitor/proof-carrying code 均为放宽某条件的实例）——证明结构→定理→工程含义完整。
- **团队背景**：智利 Universidad San Sebastián（独立作者理论工作）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13367)

**20. 论文名称**：**When Should Agents Remember? Falsification-Gated Self-Evolution（FGSE）/ 证伪门控的智能体记忆自进化**
- **核心亮点**：
  - **任务定义**：回答"经验教训何时应写入 agent 持久状态"——把记忆更新当可证伪假设治理（Agent 记忆/自进化安全）。
  - **方法核心**：候选教训表示为结构化假设（前置条件+行为变换+期望效果+验证器），经目标迁移/证伪/归档回归三组探针按门控决定 commit/refine/reject，提交后运行时违规监控自动下架；附有限样本理论保证。
  - **评估指标**：DeepSeek-V3.1 主干：WebArena 26.4→39.8（+13.4）、AppWorld 68.2→78.8；证伪失败率仅 5.0%、有害提交 3.6%；被拒的 101 个高增益候选 P̂fail 达 22.82%（提交项 1.28%）。
  - **为何优于 baseline**：自由文本教训"近迁移增益伴随边界伤害"→前置条件限定激活范围+三探针提交前测试→门控把高增益高风险候选挡在门外（消融去证伪探针有害更新 3.6%→10.6%）——假设结构化→风险可测→安全与增益兼得。
- **团队背景**：NTU+Georgia Tech+UW+WashU+Sydney+NUS 六校联合（无企业，Airon Tech 赞助算力）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.14326)

**21. 论文名称**：**From Attack Surface to Defense Architecture / 敏捷编码框架的组合式安全架构**
- **核心亮点**：
  - **任务定义**：把编码 harness 安全从"模型行为"重构为"架构与权限组合的结构属性"（Agent 安全体系结构）。
  - **方法核心**：ActiveSecHarness 神经符号参考监视器，原则"智能可提议行动，但绝不能定义自身权限"——四信任环+Macaroon 能力令牌单调衰减+乘积污点格与溯源账本+三重确定性调解边界+自愈验证闭环。
  - **评估指标**：252 fixtures（120 攻击+120 良性+12 kill chain）：Ring 0 监视器 ABR=1.000、FPR=0.000、时延 12.7µs（比 LLM-judge 快 11,200 倍）；LLM-judge FPR 0.983（118/120 良性被拒）；oracle 无状态策略漏 4/12 跨步组合攻击。
  - **为何优于 baseline**：高危失败是 agent 工具链加速的经典 OS 原语（symlink/TOCTOU/路径穿越）→LLM-judge 既慢又查不了物理不变量→权限下沉确定性 Ring 0 使边界强制与模型规模无关——权限与智能分离→时延/覆盖/误报三赢。
- **团队背景**：Evercursion+安联集团+南洋理工+澳门大学（**企业+高校**：初创+保险集团主导）。
- **相关链接**：[📄 论文](https://arxiv.org/abs/2611.13960)

#### ▍速览论文（4 篇，基于全文阅读的要点浓缩）

- **PISA 集成路由**（[论文](https://arxiv.org/abs/2611.10193)）：Qwen3 嵌入+软聚类+逐模型正确率预测构造成本效用路由，Poisson 二项"严格多数正确"代理免集成专项训练即可选变基数模型子集；四路由基准 HV 宏平均第一（ID 37.41），同质量成本再降 25-36%。意大利 CNR+比萨大学。
- **Precision on Demand**（[论文](https://arxiv.org/abs/2611.14418)）：按步不确定性（margin 门 AUC 0.959）动态切换 4/8-bit 权重的诚实模拟研究——15% 字节预算捕获 94% 翻转，但同预算下验证式投机解码 KL=0 严格更优；作者明示门控适用边界。Intel 单机构。
- **Bits or Permutations?**（[论文](https://arxiv.org/abs/2611.14413)）：统一计价（权重字节×次数）下对抗"更高精度 vs 多顺序平均"——量化主要放大槽位先验（+46.5pp）而非翻转，循环移位平均抵消槽先验（4-bit +12.6pp）；结论：预算优先换 6-bit（精度每分便宜 1.37× vs 4×），6-bit 后剩余预算才换顺序。Intel 单机构。
- **PolyRoute**（[论文](https://arxiv.org/abs/2611.13843)）：12 语言+混码 typed-decision 部门路由基准，Qwen 自生成+gemma4 审计双过滤；关键发现：微调主杠杆是任务微调而非本土数据量（0 本土行 +16.5pp）；轮换选项名使最佳系统跌至 6.5%——部署陷阱清单。与今日 Gemini Decision 1 判别式决策趋势直接呼应。

#### ▍顶会标准评审简表

| 论文 | 新颖性 | 触发精读 | 一句话理由 |
|------|--------|----------|-----------|
| TOME-Math | 中高 | ✅ | 结构保持数据流水线+课程设计，对 4.8× 数据六项全胜 |
| SteerDistill | 高 | ✅ | 行为分歧定位+闭环引导蒸馏，W4 近无损（差 0.36） |
| BookNavigator | 高 | ✅ | 悲观验证+退火探索+工具信用，OOD 学科 +29.9pp |
| SparseRead | 中高 | ✅ | 读取控制层三组件，中位省 61.4% token 质量不降 |
| PISA 路由 | 中 | ❌ | 方法组合完备但增量性质，速览呈现 |
| GTAGym | 中高 | ✅ | AAA 任务自动判分+RL 生成环境，OpenMOSS 开源 |
| Evolving Infra | 高 | ✅ | 解耦数据面+分支引用计数，切换 6.2s vs 681s |
| Platonic 审视 | 中高 | ✅ | 修正能力-对齐趋势认知：数据问题而非模型问题 |
| Precision on Demand | 中 | ❌ | 诚实负结果+模拟规模小，速览呈现 |
| WMIP | 中高 | ✅ | 16-token 约束暴露 K=100 全模型崩溃至 0.8% |
| Bits or permutations | 中低 | ❌ | 0.6B/1.7B 模拟研究，速览呈现 |
| ScrapeBench | 高 | ✅ | 首个法律合规基准（NeurIPS Datasets），合规率 <5% 警示 |
| PolyRoute | 中 | ❌ | 企业基准+常规方法，速览呈现 |
| Teaching the Harness | 中高 | ✅ | 学习者模型+诊断探针，教师离场增益保持 |
| CAMP-II | 高 | ✅ | 双边接口先验+OPD，五域 64.6%，9B 超大模型 |
| ExpertCrowd-SWE | 中高 | ✅ | 专家原创+rubric 评分，分辨力 34.8 点 vs 0.9 点 |
| FlowPlan | 中高 | ✅ | 图增量终止条件，Verified 0.32× 质量不降 |
| Dependable Agents | 中高 | ✅ | 三坐标标注揭示证据闲置 72%/88% |
| Veritas | 高 | ✅ | 验证器生成三机制，四执行基准全部第一 |
| RSI Tax | 高 | ✅ | RSI 税 109-393% 的经济学审计+极简基线反转结论 |
| Benchmarks Stop Ranking | 中高 | ✅ | 1-bit 评分饱和诊断+log 分级得分（数学推导） |
| BypassGym | 中高 | ✅ | 真实 EDR 攻防，DeepSeek 94.4% 绕过率警示 |
| No finite rulebook | 中高 | ✅ | 守门人不可能性定理全覆盖，工程谱系映射 |
| FGSE | 高 | ✅ | 记忆更新=可证伪假设，三探针+理论保证 |
| ActiveSecHarness | 中高 | ✅ | Ring 0 确定性监视器 12.7µs/零误报/全阻断 |

### 2. 产业动态与产品创新（AI Hot Skill 精选）

**1. NVIDIA 巨震：黄仁勋辞任 CEO，Colette Kress 出任临时 CEO**
- **核心内容**：因婚外情与涉间谍传闻丑闻发酵（WSJ 此前报道董事会接近决定罢免），黄仁勋辞去任职 33 年的 President/CEO 并退出董事会，即时生效；CFO Colette Kress（2013 年加入，此前 Cisco/Microsoft 近 25 年财务生涯）任临时 CEO 并继续兼任 CFO，前 Synopsys CEO Harvey C. Jones 出任史上首任董事会主席；Q3 财报（11/17）照常发布，官方承诺运营、客户承诺与产品路线图"不中断"。市场单日市值蒸发约 1.4 万亿美元，此前营收翻倍背景下股价年内已跌 28.6%。
- **落地应用场景**：AI 算力采购方（云厂商/主权 AI 项目）短期观望采购节奏；竞争对手（AMD/Intel）迎来窗口期；关注 CUDA 生态与关键客户关系（OpenAI/微软/Meta 万亿级订单）的连续性管理。
- **相关链接**：[🌐 NVIDIA 官方公告](https://nvidianews.nvidia.com/news/nvidia-announces-ceo-transition)；[🌐 TechCrunch 报道](https://techcrunch.com/2026/11/12/jensen-huang-is-out-at-nvidia-who-is-colette-kress/)

**2. Google 双弹：Gemini Decision 1 判别式决策模型 + Decisions API**
- **核心内容**：DeepMind 发布专为封闭集决策设计的新端点——单次前向传播直接对候选选项打分（非逐 token 生成），输入最多 256K token（文本/图像/音频/视频/PDF）、单请求最多 1,000 个问题，支持 Yes/No、多选（2-255 项）、有序评分三类决策混用；端到端延迟 150-250ms（16K token 内几乎恒定）、输出 $0 计费、输入 $0.10/M tokens，比 Gemini 3.5 Flash-Lite+生成式 API 便宜近 5 倍；在 agent 路由/推理/排序/多模态基准全面领先且校准误差最低。Google 内部已用于 Notebook 实时路由（日常查询延迟降 40%）、Stitch 线框生成与 Search 的 MCP 安全层。同日 Gemini 4 Hydrogen 发布（AA 智能指数 51 超 Argon 的 50；Terminal-Bench 4.1 从 18.2%→57%；Antigravity 新 Orchestrator 编排 Carbon 深思+Hydrogen 快执行，Terminal-Bench 4.0 配额省 24%/快 19%；$1.50/$7.50 定价并普惠免费用户）；另开源 A2UI 1.0 跨平台协议（智能体直接返回原生 UI，字节/彭博/Atlassian 已接入）。
- **落地应用场景**：Agent 工程的"快思考"标准化——意图分类/工具选择/路由/内容评分从生成式调用迁移到判别式接口（延迟与成本双降一个量级）；多模型工作流中决策类子任务与深度推理解耦。
- **相关链接**：[🌐 Decisions API 官方博客](https://blog.google/innovation-and-ai/technology/developers-tools/gemini-decisions-api/)；[🌐 Gemini 4 Hydrogen](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-hydrogen/)；[🌐 A2UI 1.0](https://www.ithome.com/1/025/862.htm)

**3. 推理基础设施联盟时代：SGLang Consortium 成立 + vLLM Vera Rubin 58× 吞吐**
- **核心内容**：LMSYS 在首届 SGLang Summit 上联合 NVIDIA/AMD/Intel/Google/Meta/Cloudflare/Modal/RadixArk 成立 SGLang Consortium（推理预计占 2026 年 AI 算力约 2/3，SGLang 已运行于数十万 GPU、服务 Meta/微软/谷歌/亚马逊等最大生产部署）；成员派技术代表、每年两次路线图会议，SGLang 维护者保留技术决策权。同日 SGLang-Neuron v0.1.0 开源发布（登陆 AWS Trainium）；vLLM 在 Vera Rubin NVL72 上跑 DeepSeek-V4.1-Flash 实现GB300 NVL72 的 58× 吞吐（峰值 252 万 tokens/s、单用户 P90 1,810 tokens/s），四大优化含 DEP 内核重写（prefill +28%/decode +25%）与机架级 Engram 池（多服务 68% 请求）；另有 Inkling 2 Small Day-0 支持、vLLM-Omni 跑 MiniMax H3-Image（4K 生成中位延迟 0.87s）、IBM 用编程智能体不到一周将 SGLang 集成进 Spyre。
- **落地应用场景**：企业推理选型从单引擎走向"联盟治理"格局——硬件厂商深度介入开源引擎路线图；新一代机架级硬件（Vera Rubin/Trainium）的 day-0 支持成为 vLLM/SGLang 之争的关键筹码。
- **相关链接**：[🌐 SGLang Consortium](https://www.lmsys.org/blog/2026-11-12-sglang-consortium)；[🌐 vLLM Vera Rubin 博客](https://vllm.ai/blog/2026-11-12-deepseek-v41-flash-vera-rubin)；[🌐 SGLang-Neuron](https://www.lmsys.org/blog/2026-11-12-sglang-neuron-v0.1.0)

**4. 百度世界大会 2026：词元所大模型交易所 + ERNIE 5.5 Flash + 昆仑芯**
- **核心内容**：李彦宏发布"词元所"（Token Exchange）大模型交易平台——对模型做指纹比对/能力/性能/可用性全面准入测试，对服务商按服务质量、兑现度、安全合规多维度打分并给信用评级标签（上线一个月供应资源超 100 个、日均调用 Token 突破 7000 亿）；ERNIE 5.5 Flash Preview 以 1668 分登 Code Arena: WebDev 开源模型第二（权重 11/25 前开放）；昆仑芯 M100 投用、M300 与天池超节点提前至 2027；Apollo Go 无人驾驶覆盖 28 城；李彦宏提出"人工智能下一幕是智能组织，组织智能是企业最深的护城河"；文心 5.5 Flash Preview 等三模型上线千帆。
- **落地应用场景**：企业选型痛点（模型来源杂、质量参差、虚标参数）由平台第三方准入+信用档案解决；国产模型交易走向"证券交易所"模式。
- **相关链接**：[🌐 词元所发布](https://www.ithome.com/1/025/903.htm)；[🌐 ERNIE 5.5 登榜](https://x.com/arena/status/2120685092909838405)

**5. Anthropic 生态深化：Claude Tag 创业指南 + effort 参数 + 垂直插件**
- **核心内容**：Claude Tag（Slack 中的 AI 队友，Team/Enterprise 公测）发布创业公司使用指南——8 家初创实践五大用例：Campfire 从 Slack 提及到打开 PR 仅 21 分钟；NavigateAI 开 223 个 PR（134 个已合并，中位 2 小时合并）；Moritz 日报从 1 小时缩至 5 分钟；Anthropic 内部 Claude Tag 一个月回答数据频道 75% 问题。Claude API 新增 effort 调节（对话中途切换投入档位）；Claude Meetings 开启 beta（Claude 以参会者身份加入视频会议）；Claude for Manufacturing 插件内置 17 项制造技能；官方建议 Opus 5.5 作知识工作日常主力。Artificial Analysis 实测：升一档模型比拉满 effort 更省钱高效。
- **落地应用场景**：AI 同事从"个人助手"进化为"团队协作者"——值班响应、线程变 PR、公司知识大脑、自助数据、例行报告五场景可复制。
- **相关链接**：[🌐 Claude Tag 指南](https://claude.com/blog/claude-tag-guide-for-startups)；[🌐 Claude Meetings](https://claude.com/blog/claude-meetings)；[🌐 effort 参数](https://www.ithome.com/1/025/894.htm)

**6. Agent 协作产品浪潮：Grok Bot 1.0 + Cursor Bot 1.0 + Glasses**
- **核心内容**：xAI 发布 Grok Bot 1.0（可加入 Slack/Teams 协作、可被邀入会议的 AI 团队成员，马斯克亲自宣传编排配置）；Cursor 推出 Bot 1.0；Google 与 Samsung/Gentle Monster/Warby Parker 合作推出 Gemini 智能眼镜（$395 起，语音交互+实时翻译，开放预购）；Gemini 登陆苹果 CarPlay（全球上线）；ElevenLabs 发布 Scribe v3 语音识别（利用对话上下文）与 Marketplace 平台；Luma 上线视频 Character Swap；Intangible 2.0 发布 Canvas 3D 工作区。
- **落地应用场景**：AI 协作从聊天窗口扩展到会议、可穿戴、车载全场景；语音优先交互（眼镜+CarPlay）成为多模态落地主战场。
- **相关链接**：[🌐 Grok Bot 1.0](https://x.com/elonmusk/status/2120584535423565889)；[🌐 Gemini 智能眼镜](https://arstechnica.com/google/2026/11/google-pushes-privacy-angle-as-gemini-pow)；[🌐 CarPlay 上线](https://www.ithome.com/1/025/831.htm)

**7. 安全攻防与监管：Fireworks 数据泄露 + 自动化红队 + 立法加速**
- **核心内容**：Fireworks AI 遭网络攻击，黑客窃取部分 Notion 客户的聊天记录（AI 平台供应链风险首案）；OpenAI 用自动化红队提升 GPT-6 Astra/Sol 高风险领域模糊鲁棒性，并出资 200 万美元支持儿童 AI 安全研究签署 Children First 宪章；Perplexity 发布 SPACE 沙箱红队研究；Google 公布 Agent Anomaly Detection 在五个第三方 Agent 安全基准的成绩；立法端：澳大利亚拟强制前沿模型独立测试、日本拟对拒不配合的前沿 AI 企业设处罚、美国商务部新规拟限制中国 AI 公司租用 Blackwell（七国出口许可）、Ramp 数据显示 1% 企业贡献 OpenAI/Anthropic 80% 企业收入；OpenAI 误把俄罗斯恶作剧者当乌克兰情报局长通报军事 AI 支持。
- **落地应用场景**：企业 AI 采购新增"供应商数据驻留与泄露责任"条款；Agent 产品需内建异常检测与红队演练流程。
- **相关链接**：[🌐 Fireworks 遭攻击](https://techcrunch.com/2026/11/12/cyberattack-on-fireworks-ai-allowed-hackers-to)；[🌐 OpenAI 自动化红队](https://alignment.openai.com/fuzzy-robustness/)；[🌐 澳大利亚法案](https://the-decoder.com/australias-proposed-ai-safety-bill-would-force-frontier-)

**8. 开源与资本：MiniMax H3-Image + Runway Praxis-1 + 三笔大额交易**
- **核心内容**：MiniMax H3-Image 开源权重上线（草图生成多场景一致电商/广告图）；Runway 开源机器人模型 Praxis-1（速度比 GPT-6 Astra 机械臂快 15.6 倍）；英国 AI 芯片初创 Fractile 完成 7.2 亿美元 C 轮（估值 70 亿美元）；TSMC 拟向 Anthropic 与 OpenAI 各投资 5 亿美元；OpenRouter 正式并入 Stripe（约 75-80 亿美元，其请求量数据：MAI Transcribe 2 增长 12 倍）；Norwest 新设 40 亿美元 AI 基金；Muse Ember (Max) 登顶 Arena Agent 高效模型榜（成本约竞品 1/4）。
- **落地应用场景**：开源权重+垂直场景（电商图/机器人）成为中小团队入局捷径；推理网关被支付基础设施收编（OpenRouter×Stripe），模型分发进入"结算层"整合期。
- **相关链接**：[🌐 MiniMax H3-Image](https://x.com/MiniMax_AI/status/2120593294933819491)；[🌐 Runway Praxis-1](https://www.ithome.com/1/026/104.htm)；[🌐 Fractile 融资](https://techcrunch.com/2026/11/11/with-720m-round-britains-fractile-gets-a-leg-u)；[🌐 OpenRouter 并入 Stripe](https://www.ithome.com/1/025/833.htm)

**9. 国产多模态与视频：Seedance 2.5 flash + Seedream 5.5 全平台 + SparkAudio**
- **核心内容**：字节火山引擎发布 Seedance 2.5 flash 视频模型（速度翻倍、价格降约 34%）；Seedream 5.5 上线 fal 与 PixVerse 平台，即梦画布两周新增 20+ 功能（生图常用功能全覆盖），实测即梦+Seedream 5.5 电商设计工作流与 GPT Image 2.6 横评；科大讯飞星火语音基座 SparkAudio 1.0 上线 API（99 种语言、202 种方言）；美团 LongCat 团队谈 LongCat-2.5 开源与国产算力训练幕后；昆仑万维 Matrix-Game 4.0 上线 Reactor（浏览器内实时交互生成游戏世界）；深度原理 Hydrogen-1.0 实测科研能力超 GPT-6 Astra。
- **落地应用场景**：电商设计工作流（草图→多场景一致商品图）与短视频生成成本大幅下降；多方言语音接入面向国内下沉市场与企业客服。
- **相关链接**：[🌐 Seedance 2.5 flash](https://www.ithome.com/1/026/069.htm)；[🌐 Seedream 5.5 上线 fal](https://x.com/fal/status/2120879920381325727)；[🌐 SparkAudio 1.0](https://www.ithome.com/1/026/386.htm)

**10. 开发者工具与系统研究：UCSD 形式化验证 + Drex 自纠错解析 + 对齐理论上界**
- **核心内容**：UCSD Hao AI Lab 发布 vstack（编码智能体 5 阶段形式化验证工作流，防 agent"钻空子"弱化 invariant）与 veriserve（可验证 PD 分离服务引擎，PdLedger 机制）——发现生产级服务引擎 3 类真实 RDMA 竞态 bug（GPUDirect RDMA 竞态会静默把一个用户的 KV cache 混入另一用户响应却返回 200 OK），促成 6 项上游修复；22.6 万模块/51.7 万证明义务的安全证明仅带来 +0.18% p50 TPOT 开销。NaceAI 推出自纠错文档解析模型 Drex；Apple 发表对齐理论极限研究（KL 预算下奖励提升上界与 reward hacking 分析）；OpenRouter 发布自托管 vs API 经济学（Qwen3.8 27B 单卡 6-12% 负载即回本）与 Agent 无限循环四层防护教程；AWS 开源 aws-emulator 本地模拟器；Factory 联合 Snyk/Palo Alto 组建 Cyber Defense Network。
- **落地应用场景**：Agent 生成代码的验证从"agent 审 agent"转向形式化验证+经验验证组合；PD 分离部署的 KV 隔离正确性审计成为刚需。
- **相关链接**：[🌐 UCSD vstack/veriserve](https://haoailab.com/blogs/verifiable-llm-infra/)；[🌐 Apple 对齐理论上界](https://machinelearning.apple.com/research/theoretical-limits-alignment)；[🌐 OpenRouter 自托管经济学](https://openrouter.ai/blog/insights/self-hosted-llm-vs-api/)

---

> **边界说明**：本日报覆盖北京时间 2026-11-12 00:00–24:00。arXiv 端 Thu 12 Nov 批次于 11-13 09:00 后 announce，故学术部分以 Hugging Face 11-12 榜 + arXiv Wed 11 Nov 批次增量（昨日运行时该批次尚未全量可见的任务）为准；Anthropic IPO/办公套件、AlphaGenome 2、Gemini 4 Carbon、SGLang-Neuron 预告、Grok Bot 预告、ERNIE 5.5 Flash 首发等 11-11 事件已在昨日日报覆盖，今日仅追踪其进展（如 ERNIE 登榜 Code Arena 第二）。
