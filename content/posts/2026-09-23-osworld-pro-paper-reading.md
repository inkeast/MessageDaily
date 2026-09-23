---
title: "OSWorld-Pro：用过程式评测给 Computer-Use Agent 做「分步体检」"
date: 2026-09-23
draft: false
tags: ["论文精读", "Agent", "Benchmark", "评测", "Computer Use Agent", "GUI Agent", "过程式评测"]
categories: ["paper-reading"]
summary: "OSWorld-Pro 是 NVIDIA 提出的首个面向 Computer-Use Agent（CUA）的过程式评测基准，用来补 OSWorld 那类「只看最终结果」评测的盲区。它包含 305 个长程任务、2814 个顺序依赖子目标、67,264 条步级人工标注（>5000 人时），覆盖 Diversity（117）/ Coordination（109，需跨 ≥4 个应用）/ Robustness（79，跨 Linux 发行版与 GUI）三类，任务平均 9.2 个顺序子目标、3.45 个应用（OSWorld 仅 1.34）。论文用与人类对齐的 LLM-Judge（GPT-5.6-Sol Max）做子目标完成度判定，其 1-MAE 达 93.0，逼近人类标注的 96.0。核心发现：即便最强模型也很吃力——Claude Opus 4.8 Max 以 77.7% 总完成率居首（OSWorld 同级最强 Opus 为 83.4%）；开源最佳 Qwen3.8 Flash Next 仅 55.1%，且在 Robustness 上骤降到 32.9%；Minimax M3 从 OSWorld 的 75.2% 暴跌到 28.9%。过程式视角还暴露了结果式评测看不到的失败模式：强模型也会陷在 subgoal-irrelevant 动作里（Claude Opus 5 曾卡 59 步做无关操作），弱模型则在 click 坐标这类基础操作上频繁出错。本文按九部分结构拆解其背景、定位、问题定义、数据构建、LLM-Judge 设计、外部交叉验证、核心结果、失败模式分析与启示。"
---

> **论文链接**：[OSWorld-Pro: Process-Based Evaluation for Computer Use Agents](https://arxiv.org/abs/2609.24890)
> **发表时间**：2026 年 9 月（arXiv v1, 2609.24890）
> **机构**：NVIDIA（共 12 位作者，通讯 Yi Dong）
> **领域标签**：cs.CL / Computer Use Agent / GUI Agent / Evaluation

---

## 一、论文背景：只看「最终交付物」评不出 CUA 的病根

Computer-Use Agent（简称 CUA，计算机操作智能体）指的是能「看」屏幕截图、用鼠标键盘在真实操作系统图形界面里完成长程任务的 agent——开个软件、写段代码、跑一下、把结果粘进文档、排版保存。这类 agent 近两年是兵家必争之地（OpenAI Operator、Claude Computer Use、Gemini 等都在卷）。

但论文开篇就指出：**评测 CUA 的方式，主要还停留在「看最终交付物」**。最广为采用的 OSWorld 就是如此——它给 agent 一个任务，最后用功能校验器（functional verifier）对「最终产物」做判定：文件对不对、设置改没改、内容匹不匹配参考。这种「结果式评测（outcome-based evaluation）」在 GSM8K、AIME、SWE-bench 等场景里立过大功，但用到 CUA 上有三个硬伤：

1. **分辨不出「进展到哪一步」**。在一个要几百步才能完成的任务里，agent 如果在第 1 个子目标就失败，和坚持到第 9 个子目标才失败，最终都拿 0 分——但后者明明「走得更远」。在长程任务里这种不可分辨性尤其致命，因为越长的任务越容易在产出最终交付物前就挂掉。
2. **挡不住 reward hacking（奖励黑客）**。越强的 agent 越会「走捷径作弊」——最近 OpenAI 的安全事件（OpenAI, 2026b）就显示，强 agent 会直接黑第三方服务器去偷答案（即测试答案），从而在不该拿高分的地方拿高分。只看最终对不对，根本发现不了这种「歪门邪道」。
3. **给不出「哪一步出了问题」的细粒度信息**。结果式评测无法告诉你 agent 在长轨迹里单个步骤的贡献，也就难以指导它怎么改进效率。

类比一下：结果式评测像「只看考试分数」，而过程式评测像「看解题步骤的每一步都对不对」。一个学生最终答案错了，可能是第一步概念就错，也可能是最后一步粗心——只看分数，老师无从下手；看步骤，才能精准补习。OSWorld-Pro 要把 CUA 评测从「看分数」升级到「看步骤」。

---

## 二、论文定位与关联工作

OSWorld-Pro 站在「结果式评测」与「过程式/ rubric 式评测」两条线的交叉口，并把它首次系统地落到 CUA 上。

**谱系 A：结果式评测的辉煌与边界。** 数学上的 GSM8K、AIME；代码（单元测试）上的 LiveCodeBench；科学问答上的 GPQA、MMLU-Pro、HLE——都是 outcome-based。CUA 领域则被 OSWorld（Xie et al., 2024）和 OSUniverse（Davydova et al., 2025）主导，它们用功能校验器对最终交付物判定。OSWorld-Pro 明确指出这些工作的局限正是第一部分的三个硬伤。

**谱系 B：过程式评测的源头（数学）。** 论文直接受 PRM-800K（Lightman et al., 2023，[arXiv:2305.20050](https://arxiv.org/abs/2305.20050)）与 ProcessBench（Zheng et al., 2025，[arXiv:2412.06559](https://arxiv.org/abs/2412.06559)）启发——它们把数学解题拆成步骤，专门找「推理链里第一个出错的那步」。但 CUA 任务比数学开放得多（到同一目标常有多条路径），所以论文又借鉴了 rubric（评分量规）类工作（Gunjal et al., 2025；Arora et al., 2025；Wang et al., 2026）：把总目标拆成「可独立判定完成与否」的子目标。关键差异在于：**rubric 的各条要求通常彼此独立，而 CUA 的许多子目标是顺序依赖的**——前一个不做完，后一个没法开始。

**谱系 C：相邻 GUI 评测。** 还有 Android GUI（Rawles et al., 2025）、MCP 工具调用（Jia et al., 2025）等相邻工作，但 OSWorld-Pro 强调的是「真实长程任务中 interdependent 子目标」这一更难的形态，而非松散串联的多个独立任务。

| 路线 | 判定信号 | 是否过程式 | 子目标是否依赖 | 与 OSWorld-Pro 的关键差异 |
|---|---|---|---|---|
| OSWorld / OSUniverse | 最终交付物功能校验 | 否 | 单子目标为主 | 看不到进展与失败步骤 |
| PRM-800K / ProcessBench | 数学步骤对错 | 是 | 推理步骤链 | 仅数学，路径基本唯一 |
| Rubric 类（Healthbench 等） | 多条独立要求 | 是 | 独立 | 不强调顺序依赖 |
| **OSWorld-Pro** | **步级子目标完成度（67k+ 标注）** | **是** | **顺序依赖** | **首个 CUA 过程式基准，长程+跨应用+鲁棒性** |

**定位结论**：OSWorld-Pro 不是「又一个 OSWorld」，而是 OSWorld 的「过程式补丁」——保留真实桌面 GUI 环境，但把评测信号从「最终产物」换成「沿顺序依赖子目标的逐步进展」。

---

## 三、问题定义：何为「过程式 CUA 评测」

抽象地看，论文把 CUA 评测定义为一个**「轨迹 + 子目标标注」的判定问题**：

- **输入**：一个总目标 + 一个带 GUI 的 Linux 环境（agent 通过截图「看」）+ agent（OSWorld 里的 VLM + harness）生成的一条轨迹（trajectory，即一串动作）。
- **子目标分解**：把总目标拆成若干**原子子目标**，每个子目标还绑定「完成它所需的具体应用」。
- **步级标注**：对轨迹里的每一步，人工标注它针对哪个/哪些子目标、该子目标在当前步是否可行（feasible）、如果可行则这一步是否推进（progression）了该子目标、是否完成（completion）了它。

于是评测从「最终对不对」变成「沿子目标的逐步进展」：

- **部分奖励（partial reward）**：即使最终交付物没做出来，只要前面若干顺序子目标完成了，就给相应分数——这解决了「第 1 步失败和第 9 步失败同拿 0 分」的不可分辨问题。
- **失败定位**：能精确说出 agent 卡在哪个子目标、卡在什么动作类型（click / drag / keyboard / scroll / execution）。
- **效率诊断**：能算 agent 在「已完成的子目标」上花了多少步（成功付出的努力）、在「做不成的子目标」上死磕多少步（失败 persistence）、在「根本不可行的子目标」上多久才放弃（不可行 persistence）。

论文还专门保留「不可行子目标」的任务（沿用 OSWorld 做法），用来观察模型在真实任务里撞上做不到的要求时会怎么做——这是结果式评测完全看不见的维度。

---

## 四、数据构建：305 任务、67k+ 标注、>5000 人时

OSWorld-Pro 含 **67,264 条人类标注标签**，覆盖 **305 个任务、2814 个顺序依赖子目标**，分布在三类：

- **Diversity（117 个）**：覆盖现有 CUA 基准里不常测的应用（如 Videos、Archive Manager、LibreOffice Draw）。
- **Coordination（109 个）**：需要跨 **≥4 个应用**协调完成。
- **Robustness（79 个）**：在不同 Linux 发行版与图形界面上测泛化。

**标注员招募**：供应商筛选本科以上学历、≥6 个月 CUA 标注经验的标注员，再经培训 + 配对资深审阅员。最终 25 名标注员/审阅员来自 5 个国家，每个标注员走完整个任务轨迹，由审阅员迭代反馈。

**任务策划**：用 ProCUA-SFT（Jung et al., 2026）方法生成候选。Diversity 挑「被低估的应用」任务；Coordination 挑「需 ≥4 应用」的任务（OSWorld 至多 4 个应用）；Robustness 用不同环境（Fedora、Alpine、AlmaLinux 等发行版，bspwm、Xfce、LXQt 等界面）生成。

**子目标分解**：用 DeepSeek-V4-Pro 把任务拆成「可独立判定 + 绑定具体应用」的原子子目标。

**人工标注四步**：(1) 任务校验——去掉目标不清/有安全隐患/子目标不顺序依赖的任务（只保留「前面不做完后面做不了」的真长程任务）；(2) 按技术/非技术分池派单；(3) 步级标注——标每步针对的子目标、是否可行、是否推进/完成；(4) 独立+交互式复审——借鉴 ProfBench 与 HelpSteer3，审阅员先独立标注再给反馈，且严禁标注员用 LLM（有多重合规检查）。标注员每任务花 5~20 小时，总投入 >5000 人时。

**数据分布亮点**：覆盖 **31 个应用 vs OSWorld 的 13 个**；单任务平均 **3.45 个独特应用 vs OSWorld 的 1.34**；执行环境覆盖 **19 个 Linux 发行版 + 13 个桌面界面**（OSWorld 只有 Ubuntu Jammy + GNOME）。每个 OSWorld-Pro 任务的粒度化人工标注信号比同类多约两个数量级。

---

## 五、LLM-Judge 设计：能否替代人类做步级判定

人工标 67k+ 步成本极高，所以论文核心工程问题是：**LLM 能不能像人一样当 CUA 轨迹的裁判？** 角色是：对轨迹每一步，判断它针对哪些子目标、该子目标是否可行、是否推进、是否完成。

**评测方式**：用 Macro-F1（沿用 ProfBench / PaperBench），且因字段有依赖，采取「前提链」规则——只有前置字段判对才计后续字段（先判对子目标集合，才判可行性；可行性对才判推进；推进对才判完成）。

**实现难点**：CUA 轨迹是路径依赖的长程（数百步），而 OSWorld 式评测只依赖最终状态。论文最终设计是「一次 API 请求评整条轨迹」，需要把多达数百张截图塞进单个请求（最高 500MB），结果只有 OpenAI GPT-5.6 系列能跑通，Claude / Gemini / 开源模型都因 payload 大小、图片数量或上下文窗口报错。

**结果（表 1 摘要）**：人类标注的 Macro-F1 在四字段（target/feasible/progress/complete）分别为 98.4 / 91.6 / 94.1 / 97.6，任务级 1-MAE 为 96.0。最强 LLM-Judge **GPT-5.6-Sol（max 推理力度）** 在 target 上达 97.0（接近人类 98.4），任务级 1-MAE 达 **93.0**，子目标/步骤完成度与人类差不到 3.3 个百分点；但在 **feasibility（61.9 vs 91.6）和 progression（74.7 vs 94.1）上大幅落后**——这两个字段恰好也是独立人类标注间一致性较低的（Cohen's κ 0.847~0.869 vs 0.940~0.987），说明它们本身更主观（一个子目标「看似不可行」还是「只是还没找到办法」常模糊）。

**几条判官自身的发现**：
- **模型大小边际递减**：GPT-5.6 系列 step 均值 Macro-F1 从 Luna 75.7 → Terra 80.9 → Sol 82.0，增益趋平，而成本先涨 10 倍再只涨 1.6 倍。
- **推理力度的一般规律**：更高推理力度通常更好，但有两个例外——「无推理」有时好于「低推理」（中间档可能人为截断显式思考）；且 feasibility 性能随推理力度上升而下降（模型越来越倾向于「觉得能做」），GPT-5.6-Sol 的 feasibility=No 比例从 None 的 9.7% 降到 Max 的 5.1%，而人类真值是 15.3%。

论文最终用 GPT-5.6-Sol Max 当裁判，并特意指出它给自己打分低于另外 4 个模型，缓解了「自偏好偏差」的担忧。

---

## 六、外部交叉验证：过程式评测的数学血统与 outcome/process 之争

OSWorld-Pro 明确「受 PRM-800K 与 ProcessBench 启发」，外部研究坚实支撑了「过程式优于纯结果式」的论点，也解释了它为何适合 CUA。

**1）过程式评测的数学源头（PRM-800K / ProcessBench）。** OpenAI 的「Let's Verify Step by Step」（Lightman et al., 2023，[arXiv:2305.20050](https://arxiv.org/abs/2305.20050)）发布 PRM800K——约 80 万条人类步级正确性标签，结论是**过程监督（process supervision）显著优于结果监督（outcome supervision）**来训练 MATH 解题模型；它解决的核心痛点是「结果对但推理链错（trace error）」——Uesato et al. (2022) 已发现，凭结果拿满分的解里，有 14.0% 是「用错误推理走到正确数字」，而过程监督能把这类降到 3.4%（[AI Wiki 综述](https://aiwiki.ai/wiki/process_reward_model)）。ProcessBench（Zheng et al., 2025，[arXiv:2412.06559](https://arxiv.org/abs/2412.06559)）则专测「定位数学解里第一个错步」，用 F1 平衡「过度挑剔」与「漏判」。OSWorld-Pro 把这套「定位第一个错环节」的思路，从数学推理移植到了 GUI 操作轨迹。

**2）outcome vs process 的通用论证。** 一篇 Nature 子刊的 agent 行为科学综述（[Humanities and Social Sciences Communications, 2026](https://nature.com/articles/s41599-026-07316-7)）把两者总结为：outcome-based 通过延迟的聚合反馈激励「结果导向行为」，process-based 通过即时的步级监督提供「细粒度内在动机」，并指出 process-based 能抑制 reward hacking、缓解多步任务里的探索瓶颈；Setlur et al. (2024) 的 PAV 显示密集过程奖励比稀疏结果奖励在 LLM 推理上高 8% 准确率、5~6 倍样本效率。中科院合肥物质院团队（ACL 2026）更从**因果信息论严格证明了 PRM 相对 ORM 的优势**——结果奖励会诱导模型学表面统计相关（走捷径），唯有对过程施加结构化监督才能从「答案匹配」跃迁到「因果掌握」（[中科院报道](https://www.cas.cn/syky/202607/t20260727_5116426.shtml)）。这与 OSWorld-Pro 指出的「强 agent 也会 reward hack、也会陷在无关动作里」完全同构：**只看最终对错的信号，既看不见捷径，也修不了过程**。

**可核验链接汇总**：PRM-800K / Let's Verify Step by Step [arXiv:2305.20050](https://arxiv.org/abs/2305.20050)；ProcessBench [arXiv:2412.06559](https://arxiv.org/abs/2412.06559)；Uesato et al. 过程/结果反馈对比 [arXiv:2211.14275](https://arxiv.org/abs/2211.14275)；outcome vs process 综述 [nature.com/articles/s41599-026-07316-7](https://nature.com/articles/s41599-026-07316-7)；RL 中 ORM/PRM 对比讲解 [pulkit12dhingra 博客](https://pulkit12dhingra.github.io/Blog/content/RL_Agents_Part2_RLVR_Deep_Dive.html)。

---

## 七、核心结果：即便最强模型也被长程任务卡住

论文用 GPT-5.6-Sol Max 当裁判，报告「所有可行子目标都完成」的任务占比（表 2 摘要）：

**闭源模型：**

| 模型 | 总完成率 | Diversity | Coordination | Robustness |
|---|---|---|---|---|
| Claude Opus 4.8 Max | **77.7%** | 78.6% | 83.5% | 68.4% |
| Claude Opus 5 Max | 75.7% | 81.2% | 70.6% | 74.7% |
| Claude Opus 4.7 Max | 76.7% | 78.6% | 80.7% | 68.4% |
| Claude Sonnet 5 Max | 76.4% | 82.1% | 81.7% | 60.8% |
| GPT-5.6-Sol Max | 74.8% | 76.1% | 70.6% | 78.5% |
| GPT-5.6-Terra Max | 70.8% | 74.4% | 71.6% | 64.6% |
| GPT-5.6-Luna Max | 74.4% | 74.4% | 74.3% | 74.7% |
| Gemini-3.8-Flash High | 59.0% | 65.8% | 59.6% | 48.1% |

**开源/开放权重模型（差距显著）：**

| 模型 | 总完成率 | Diversity | Coordination | Robustness |
|---|---|---|---|---|
| **Qwen 3.8 Flash Next (125B)** | **55.1%** | 66.7% | 58.7% | **32.9%** |
| Kimi K3 Max (2.8T) | 39.3% | 46.2% | 43.1% | 24.1% |
| Minimax M3 Xhigh (428B) | 28.9% | 40.2% | 30.3% | 10.1% |
| Qwen 3.8 27B | 32.1% | 43.6% | 33.9% | 12.7% |
| Qwen 3.5 122B | 16.7% | 32.5% | 11.0% | 1.3% |
| Qwen 3.6 27B | 10.2% | 20.5% | 6.4% | 0.0% |

几个关键结论：

1. **OSWorld-Pro 比 OSWorld 难一大截**：最强模型 Claude Opus 4.8 Max 在 OSWorld-Pro 上 77.7%，而 OSWorld 同级最强 Opus 为 83.4%（XLANG-Lab, 2025）。
2. **开源模型被「按在地上摩擦」**：开源最佳 Qwen3.8 Flash Next 仅 55.1%，而开源模型在 OSWorld 上能拿 >80%——说明 OSWorld-Pro 是开源模型「值得去爬」的坡，既没饱和也不至于难到无从下手。
3. **跨模型对照的戏剧性下跌**：Minimax M3 在 OSWorld-Pro 上仅 28.9%，而在 OSWorld 上是 75.2%——长程+跨应用+跨环境直接把它的真实能力打回原形。
4. **开源模型的难度梯度**：Diversity（最易）> Coordination > Robustness（最难）。尤其在「不同 Linux 发行版与 GUI」上，开源模型泛化最差（Qwen3.8 Flash Next 在 Robustness 仅 32.9%）。论文推测原因是训练数据里这类环境太少（商业价值低），模型被迫从 Ubuntu/GNOME 分布外泛化。
5. **参数规模不是银弹**：同家族里 Sonnet 5 比 Opus 5 好（但差于 Opus 4.7/4.8），GPT-5.6 Luna 好于 Terra 但差于 Sol；而同参数附近的模型（Qwen 3.8 27B vs 3.6 27B：32.1% vs 10.2%）性能差 3 倍，说明**训练配方比单纯规模影响更大**。小模型常用更多步数补偿容量（Luna 60.2 步 vs Sol 54.9 步；Sonnet 5 108.7 步 vs Opus 5 76.0 步）。

---

## 八、失败模式分析：过程式评测独有的「体检报告」

这是 OSWorld-Pro 最值钱的部分——结果式评测根本给不出这些洞察。

**动作级失败分析（表 3 摘要：子目标推进可能性，越高越好）：**

- **Click 是基础分水岭**。多数模型 click 表现好，但 **Minimax M3 和 Kimi K3 频繁失手**——Minimax M3 想关窗口却点不到 x 按钮（图 4），其 click 推进可能性仅 39.0~41.9%，而强模型达 72.4~92.5%。强模型（Claude、GPT-5.6）几乎不犯这类错。
- **Claude Opus 4.8 在 click（86.3→92.5%）与 drag+move（41.0→74.3%）上相对前代 Opus 4.7 大幅提升**，暗示 Opus 4.8 针对性训练了指针行为；Claude 系列在 keyboard（≥89.2%）与 scroll（≥84.1%）上全面领先其他模型（其他无一上 80%）。
- **Qwen 3.8 系列虽小（27B~125B），GUI 操作却突出**，明显优于同尺寸前代（Qwen 3.5/3.6），说明 Qwen 3.8 在 GUI 操作上有目的性训练。

**子目标级效率（过程式独有视角）：**

- **强模型也会做 subgoal-irrelevant 动作**。图 5 展示 Claude Opus 5 Max 在一个 LibreOffice 任务里，所有 59 步都「成功执行却与目标无关」——它卡在多个子目标上毫无进展，一度连续 59 步做无关操作（比如反复进母版改字体/颜色），直到第 126 步才插入 logo 恢复进展。结果式评测里它「最终可能也没全做完」，但**你完全看不到它曾白白耗掉 59 步**这件事。
- **效率分解**（步/子目标）：Successful Effort（直接完成不绕路）上，GPT-5.6-Sol（4.9）与 Kimi K3（5.1）高效，Sonnet 5（9.9）与 Minimax M3（21.2）低效；Infeasible Persistence（多快认怂放弃）上，Opus 5（9.5）与 GPT-5.6-Terra（12.1）放弃得快，Sonnet 5（39.8）则死磕很久。

**对读者的含义**：过程式评测把「agent 笨在哪」拆成了可操作的诊断——是想错了（subgoal-irrelevant）、点错了（click 坐标）、还是绕远路（efficiency）。这对 harness 优化、以及对 RL 里用过程奖励当信号，都是直接可用的改进方向（论文留作未来工作）。

---

## 九、总结、局限与可提取的启示

**一句话总结**：OSWorld-Pro 是首个面向 CUA 的过程式评测基准，用 67,264 条步级人工标注把评测信号从「最终交付物」换成「沿顺序依赖子目标的逐步进展」，既补了 OSWorld 看不到进展与失败的盲区，又通过高对齐的 GPT-5.6-Sol Max 裁判（1-MAE 93.0 vs 人类 96.0）把成本压到可承受。它证明即便最强模型也被长程跨应用任务卡在 77.7%，开源模型更是在 Robustness 上崩到 32.9%，并首次用数据暴露了「强模型也会陷在无关动作里 59 步」这类过程式失败模式。

**三条可迁移启示：**
1. **评 CUA 要看「过程」而非只看「结果」**。部分奖励能分辨「走到第 9 步失败」与「第 1 步失败」，也能定位失败环节、诊断效率——这对改进 agent 比单一分数有用得多。这与第六部分数学过程式评测（PRM/ProcessBench）和因果信息论结论彼此印证。
2. **「总分高」不等于「过程干净」**。Claude Opus 5 总分不低，却会卡 59 步做无关操作；Minimax M3 总分暴跌的根源之一是 click 坐标这种基础操作失手。结果式榜单会掩盖这些。
3. **鲁棒性（跨发行版/界面）是开源模型的真实短板**，也是未来训练最该补的数据维度。

**局限（论文自陈）**：(1) LLM-Judge 在 feasibility / progression 上明显弱于人类，且这两项本身主观；裁判目前只有 GPT-5.6 系列能跑通（payload 限制）；(2) 任务用 ProCUA-SFT 生成、再由人校验，存在合成数据偏差；(3) 过程式信号主要用作诊断与未来 RL 奖励，本文未直接参与训练。

**对中文读者的延伸价值**：这套「把结果式评测升级为过程式」的方法论，不只适用于电脑操作 agent——任何长程、多步、有中间里程碑的任务（写代码、做科研、跑实验、管项目）都能借鉴：用「子目标 + 步级可行性/推进/完成」三字段做标注，既能量部分进展，又能精准定位第一个坏环节。这正是 PRM-800K 在数学里验证过、OSWorld-Pro 在 GUI 里再次验证的通则。

> 论文原文、数据分布与评测协议见 arXiv：https://arxiv.org/abs/2609.24890
