---
title: "Aegis OS: A Transactional Multi-Tenant Operating System Runtime for Eliminating Authorization Fatigue in Autonomous Coding Agents 精读"
date: 2026-11-10
draft: false
tags: ["Agent", "AI安全", "Coding", "Harness", "论文精读"]
categories: ["paper-reading"]
summary: "深度精读 Heurion Research 的单作者系统安全论文 Aegis OS。它直面编码 Agent 的安全-自治两难：逐动作确认提示打断 45–57% 的工具调用造成授权疲劳，--yolo 模式又让主机凭证暴露于提示注入。核心洞察是事务边界安全——授权不应守卫隔离状态空间内的投机性中间动作，只应守卫进入主机仓库或外部网络的不可逆边界跨越。用 APFS clonefile / Linux FICLONE 的写时复制影子工作区 + Seatbelt/Bubblewrap 内核沙箱 + L7 域名/方法/路径作用域的秘密边界注入出口代理 + Diff3 三方合并提交门，把中断提示率从 53.31% 压到 1.12%，80 次端到端攻击全部拦截，克隆 1000 文件仓库只要 6.3ms。"
---

# Aegis OS: A Transactional Multi-Tenant Operating System Runtime for Eliminating Authorization Fatigue in Autonomous Coding Agents —— 精读

> **论文链接**：[https://arxiv.org/abs/2611.08516](https://arxiv.org/abs/2611.08516)
>
> **发表时间**：2026 年 10 月 22 日（arXiv 2611.08516v1 [cs.OS]）
>
> **发表机构**：Heurion Research（独立研究机构）
>
> **作者**：Hui Zhao（单作者）
>
> **备注**：纯企业/独立实验室性质，无高校合作。开源实现与可复现基准测试套件位于 [github.com/heurion-org/aegis-os](https://github.com/heurion-org/aegis-os)，含 9 个 Rust crate。分类为 cs.OS 主分类，兼跨 cs.CR / cs.MA / cs.SE。

## 一、论文背景

先讲清楚这篇文章要解决的痛点，需要从四个前置概念说起。

**间接提示注入（indirect prompt injection）是什么**：攻击者把恶意指令埋进 Agent 会读取的内容里——仓库文件、commit log、issue 描述、网页——诱使 LLM 在正常任务执行中顺手执行恶意工具调用。与直接注入（用户自己在 prompt 里攻击）不同，间接注入的载荷藏在"数据"里，Agent 与用户都可能毫无察觉。它是本文威胁模型 T1 的源头，也是后文 80 次攻击实验的假设前提：**驱动 Agent 的 LLM 被视为不可信**。

**授权疲劳（authorization fatigue）是什么**：当系统频繁弹出安全确认对话框，用户会逐渐从"阅读-判断-批准"退化为"条件反射式批准"。这是可用安全（usable security）领域被反复验证的经典失效模式——Android 权限对话框研究（Felt et al., SOUPS 2012）和浏览器警告研究（Alice in Warningland, USENIX Security 2013）都表明提示频率越高、用户越不看内容。放到编码 Agent 场景：一个 30 步的修 bug 轨迹通常包含 13–18 个改写状态的操作，论文实测 **45%–57% 的工具调用会触发确认提示**，把自主工作流切成 4–7 步的碎片，并训练出"盲批"肌肉记忆。Anthropic 自己的遥测数据也印证了这一点：Claude Code 用户批准了约 93% 的权限提示。

**安全-自治两难（Security–Autonomy Dilemma）是什么**：Claude Code、Codex CLI、Google Antigravity 这类本地编码 Agent 直接在开发者的活动工作树上改文件，每次 `fs_write`、`proc_exec` 都对主机产生即时副作用。于是只有两个坏选项：要么每个动作都要人确认（授权疲劳），要么开 `--yolo` / `--dangerously-skip-permissions` 旁路标志（一次间接提示注入就能读走 `~/.ssh/id_rsa`、`~/.aws/credentials`，或把 `$GITHUB_TOKEN` 发到攻击者域名）。

**多 Agent 工作区冲突与上下文膨胀**：开发者越来越多地并行跑异构 Agent（一个管架构、一个写后端、一个生成测试），共享一个活动目录时互相覆盖改动；大规格说明内联经过 LLM 上下文窗口又造成 token 膨胀。这两个问题（H1 并发丢失更新与上下文成本）和前述安全问题是耦合的——没有隔离就没有安全的并发，没有引用化就没有便宜的协作。

**多租户（multi-tenant）是什么**：这里的"租户"是组织（organization）层的隔离单位——同一台工作站上可以有多个组织（如 `org-alpha`、`org-enterprise-0..9`），每个组织下又有多个 Agent 主体（principal，如 `agy-agent`、`claude-code`、`codex-cli`）。跨组织的 CAS 工件读取、IPC 消息注入属于越权（威胁 T5），由策略引擎的 R1 规则把守。这沿用了云安全里"租户隔离"的语义，只是租户从"客户"变成了"组织"。

**原生 CLI 沙箱（Claude Code / Codex 的 Seatbelt/Landlock 模式）看似折中**，但论文的对照实验显示：开启网络的 workspace-write 沙箱在 80 次攻击中仍然失守 68 次（85% ASR）——因为它原地改写共享树、明文秘密仍在环境变量里、也没有事务语义。逐项看它的失守点：仓内破坏性命令（`rm -rf src`）直写活动主机树；黑名单挡住了 `.ssh/.aws/.gnupg` 却挡不住 `.npmrc`、`.pypirc`、`.netrc`、`.git-credentials` 等 dotfile 凭证；`$GITHUB_TOKEN` 明文环境变量可被任意外发；树上的 `build.rs`、`Makefile`、`package.json` 仍可投毒。

论文要同时打三场仗：减提示（可用性）、挡攻击（安全性）、保并发（多 Agent 正确性），而且不许在任何一个维度上向另一个维度赎买——这就是"事务边界安全"要一次性解决的联合约束问题。

## 二、论文定位和关联工作

论文自我定位是"环境运行时"（Environment Runtime Only）：不内嵌 LLM 规划器、不含 prompt 模板，只提供文件系统事务、OS 进程隔离、网络边界代理、A2A 进程间协作与策略执行，经 MCP（垂直工具中介）+ Google A2A（水平协作）暴露 35 个 MCP 工具。

| 相关研究线 | 代表工作 | 与 Aegis OS 的关系 |
|---|---|---|
| OS 事务与投机执行 | TxOS（SOSP'09 内核级系统事务）、Speculator、Mbox、UnionFS | 思想源头：把"事务边界"从内核改造移植到 Agent 执行基座；Aegis 用现代文件系统原语（clonefile/FICLONE）而非改内核实现 |
| 可用安全的授权疲劳 | Felt et al.（移动权限）、Alice in Warningland（浏览器警告） | 动机依据：提示频率与警告效力负相关；Aegis 让沙箱内动作无副作用、天然可逆，从而可安全免提示 |
| LLM Agent 沙箱与注入防御 | ToolEmu、AgentDojo、InjecAgent（攻击面）；IsolateGPT、CaMeL、FIDES（信息流控制） | 互补关系：模型级 IFC 防御 + Aegis 的系统级事务边界；CaMeL 管信息流，Aegis 管 OS 资源的事务语义 |
| 生产级 CLI 沙箱 | Claude Code sandbox-runtime、Codex CLI（Seatbelt/Bubblewrap/Landlock） | 最直接的对照基线：同样是内核 MAC，但原地写共享树、无边界秘密注入、无事务合并（论文 Table 6 量化了差距） |
| Agent 操作系统与多 Agent 框架 | AIOS（LLM 调度内核）、AutoGen、MetaGPT | AIOS 调度 LLM 查询但无 OS MAC；AutoGen/MetaGPT 是 Python 对话协调；Aegis 是语言无关的 OS 运行时 |
| 能力系统与 microVM | Capsicum、Firecracker | Tier 2 microVM 控制面（libkrun/Firecracker）借鉴来源 |

一个值得注意的行业背景：Anthropic 于 2025 年 10 月开源了 sandbox-runtime（Claude Code 沙箱化 bash 背后的 OS 级隔离层），官方报告内部使用中权限提示减少约 84%。Aegis OS 与它共享同样的原语（Seatbelt/Bubblewrap + 代理网络），差异在于 Aegis 额外叠加了事务性 CoW 工作区、三方合并提交门与秘密边界注入——本文第六部分会展开这个差异是否解释指标差距。

为了更直观地理解 Aegis 在执行基座谱系里的位置，把论文 Table 1 的对比浓缩如下：

| 执行基座 | 工作区供给 | N-Agent 磁盘成本 | IPR | 内核 MAC | 边界秘密注入 | 事务性三方合并 | 原生 IPC+CAS |
|---|---|---|---|---|---|---|---|
| Host-Interactive（无约束 CLI） | 原地（0ms） | 共享树 | 高（45–57%） | ×（仅提示） | ×（原始环境变量） | ×（原地写） | × |
| Host-YOLO（跳过权限） | 原地（0ms） | 共享树 | 0% | ×（无约束） | ×（原始环境变量） | ×（静默覆盖） | × |
| 原生 CLI 沙箱 | 原地（约 6ms） | 共享树 | 低–中 | ✓（Seatbelt/Landlock） | ×（环境变量进沙箱） | ×（共享工作树） | × |
| Docker 每 Agent 容器 | 容器初始化 | 每容器层 | 低 | ✓（NS/cgroups） | ×（环境变量进容器） | ×（隔离卷） | × |
| AIOS（LLM 调度内核） | 原地 | 共享树 | 中 | ×（无 OS MAC） | ×（无出口代理） | ×（无 CoW 合并） | 部分 |
| **Aegis OS** | 0.71–6.28ms（APFS CoW） | 仅改动的文件 | 0.07%–3.33% | ✓（sandbox-exec/bwrap） | ✓（$AOS_SECRET_*） | ✓（两阶段 LCS Diff3） | ✓（aos-ipc） |

一个容易混淆的辨析：AIOS 也自称"Agent 操作系统"，但它调度的是 LLM 查询与上下文快照（运行在应用层之上）；Aegis OS 是经典 OS 意义上的运行时（运行在 Agent 进程之下、内核之上），管文件系统事务、进程隔离、网络边界。两者名字像，层次完全不同。

## 三、问题定义

论文把问题拆成 **5 类对抗性威胁向量 + 2 类非对抗性可靠性风险**，并给出信任边界：

**威胁模型（LLM 被视为不可信）**：

| 编号 | 威胁 | 典型场景 |
|---|---|---|
| T1 | 间接提示注入 | 仓库文件、commit log、issue、网页里埋恶意指令 |
| T2 | 主机路径逃逸与篡改 | 写 `../../etc/hosts`、`~/.zshrc`，或预埋 `.git/hooks/pre-commit`、`build.rs` |
| T3 | 凭证收割 | 读 `~/.ssh/id_rsa`、`~/.aws/credentials`、`~/.kube/config`，乃至自定义 `~/custom_vault` |
| T4 | 网络秘密外泄 | 把 token 发到未授权域名、`curl --noproxy '*'` 绕过代理、在白名单域名的 issue 正文里夹带私钥 |
| T5 | 跨租户越权 | 读别的租户组织的 CAS 工件、注入 IPC 消息 |

**非对抗性风险**：H1 并发丢失更新（并行 Agent 互相覆盖编辑）；H2 多文件幻觉损坏（投机重构时几十个模块语法崩坏、无法回滚）。

**信任计算基（TCB）**：主机 OS 内核（XNU/Seatbelt 或 Linux namespaces）+ 运行在沙箱外的 Aegis OS 二进制。关键假设：宿主 Agent 客户端把文件变更与 shell 执行路由经过 aegis-os（内置的无约束 Bash/Write 要么在客户端权限里禁用、要么被 PreToolUse 钩子拦截）。这个假设是整个系统成立的前提——MCP 是加性（additive）工具协议，Agent 客户端若仍保留可用的原生无约束工具，Aegis 的一切不变量都无从谈起。

**四个评估指标**，这里逐个解释：

- **IPR（Interruption Prompt Rate，中断提示率）**：需要人工确认的工具调用占比。这是授权疲劳的直接代理指标。
- **T_human（建模人工验证等待时间）**：按每次提示 5 秒的保守上下文切换成本累计。
- **MAS（Maximum Autonomous Streak，最大自主连击）**：不间断执行的最长工具调用序列，衡量工作流连续性。
- **ASR（Attack Success Rate，攻击成功率）**：端到端攻击成功修改主机文件/读取受限凭证/外泄秘密/突破租户隔离的比例。

**问题的本质可以一句话概括**：**授权提示守错了地方**——它守卫的是"隔离状态空间内的投机性中间动作"（天然可逆、无副作用），而不是"进入主机仓库或外部网络的不可逆边界跨越"（真正需要人把关的地方）。这个错位让安全与可用性互为人质：收紧提示伤效率，放松提示伤安全。Aegis 的全部设计可以理解为把这个错位矫正回来。

顺带一提，问题定义里还藏着一个容易被忽略的子问题：上下文经济学。多 Agent 系统里大工件（规格、搜索结果、日志）内联经过 LLM 上下文窗口的成本是真实token 成本——论文用 CAS 句柄把它压到近零，这既是协作原语也是成本优化。

## 四、问题解法

Aegis OS 的解法是一个五组件联动的运行时，核心是"三步工作区事务"。

### 4.1 三步工作区事务

**Step 1——即时 CoW 影子分叉（0.7–6.3ms）**：Agent 会话启动时，把主机仓库克隆为隔离的写时复制影子工作区，目录结构是 `<runtime_dir>/orgs/<org>/agents/<agent>/cow_workspace/upper_workspace/`，旁边伴生 `baseline_snapshot/`（记录会话初始化或上次提交时的主机状态）与 `snapshots/<snap_id>/`（时间旅行检查点）。

**CoW（Copy-on-Write，写时复制）是什么**：克隆时不复制数据，只复制元数据并让新旧文件共享同一批磁盘块；任何一方写入时才真正分配新块。类比：两个人共用一本参考书的"目录副本"，谁都不动笔时就只有一本书的实体；谁要在某页写字，复印那一页给TA。`clonefile` 是 APFS 对整个目录树做这件事的系统调用，`FICLONE` 是 Linux 对单个文件做 reflink 的 ioctl。

macOS 上用 APFS `libc::clonefile`（整目录一次系统调用，只复制元数据、共享磁盘块，自动跳过 `.git`/`target`/`node_modules`）；Linux 上逐文件 `ioctl(FICLONE)` reflink（btrfs/XFS），ext4 无 reflink 时回退 `copy_file_range`，另有可选的 `BTRFS_IOC_SNAP_CREATE_V2` 子卷快照快速路径（对已落盘的万文件 monorepo 是 O(1) 的 2.1ms，但对脏页缓存的新写树反而要 27.9ms——内核强制同步 flush 延迟分配，所以默认仍走逐文件 FICLONE）。与 `git worktree` 的关键区别：worktree 需要有效的 git 提交历史、共享对象存储、无法亚毫秒快照脏的未跟踪树；Aegis 对任意目录透明工作、支持即时嵌套检查点。两个工程细节值得注意：

- **单打开优化**：初始实现每文件发 12–16 个系统调用（检查存在性、stat、递归建目录、重复 fstat），优化后的 `cow_clone_file_inner` 每文件只要 7 个系统调用，把 1000 文件的供给从 37.4ms（btrfs）压到 15.6ms。
- **元数据指纹**：commit/diff 需要判断 Agent 改了哪些文件，三份全读要 542ms；克隆保留源文件的 mtime，未编辑文件 size+mtime 与基线一致即可跳过。遵循 git 的"racily clean"两秒规则（mtime 晚于克隆时刻 2 秒内的文件仍按内容比较），status 查询降到 7.0ms。残余情况——Agent 故意等尺寸改写再恢复旧 mtime——会导致该编辑不被提交，但失败方向是安全的（未审内容不会上主机）。

**Step 2——绿区零提示投机**：影子树内自由编辑/编译/测试。**"绿区/红区/琥珀区"是论文给操作空间的三色分区**：绿区=沙箱内 CoW 变更、受约束进程执行、组织内 IPC/CAS（自动放行、零提示）；红区=主机敏感路径（无条件拒绝）；琥珀区=主机提交与外部网络（人工批准或租约）。

macOS 用 Seatbelt SBPL 强制：全局 `deny file-write*` + 白名单写影子树、regex 拒绝凭证读取（`~/.ssh`、`~/.aws`、`~/.gnupg`、Keychain 等）、`deny appleevent-send` 防 AppleScript/GUI 逃逸、出站网络只允许代理的精确 loopback 端口（内核 socket filter 层强制，`curl --noproxy` 直接 Operation not permitted）。Linux 用 Bubblewrap `--unshare-all` + **默认拒绝式 $HOME 遮蔽**：先对所有候选宿主主目录（`$HOME`、`/root`、`/home/*`、操作员声明的额外根）挂空 tmpfs，再选择性只读重绑编译工具链（`.rustup`、`.cargo/{bin,registry,git}`，故意排除 `.cargo/credentials.toml`）。妙处在于**连未知的自定义凭证路径也被遮蔽**——`~/custom_vault/secret_key.pem` 在挂载命名空间里根本不存在。网络命名空间隔离了代理本身，因此通过"宿主中继 Unix socket + 沙箱内 shim + 每沙箱随机 token"的出口桥接访问代理，沙箱无法冒用别的 Agent 的代理身份。三主沙箱内操作 100% 自动放行、0 提示。

Tier 2（microVM）作为云端控制面接口：提供 virtio-fs 挂载标签、vCPU/内存限制的配置生成器与 Scale-to-Zero 的 pause()/resume() 状态机（libkrun/Firecracker 后端，论文中为规格接口、未绑定真实进程）。

一个把三步串起来的心智模型：**Step 1 造一个"平行宇宙"（毫秒级、块共享、零拷贝），Step 2 让 Agent 在平行宇宙里拥有完全的自由（内核保证它够不到真实宇宙），Step 3 开一扇唯一受审的门把成果搬回真实宇宙（搬的过程自带冲突检测与编译验证）**。授权疲劳的解法不是"更聪明的提示"，而是"让绝大多数动作不再需要提示"。

**Step 3——边界提交门（1 提示或 1 租约）**：只有 `workspace_commit` 才触发两阶段 LCS Diff3 三方合并。**事务边界（transaction boundary）的类比**：整个影子工作区像一张草稿纸，Agent 在上面随意涂改（写坏也无所谓，回滚即可）；只有"誊抄回正式作业本"（提交到主机）这一个动点需要把关。**租约（lease）是什么**：一次人工批准可以换一张"票据"——one-shot（ttl=0，用一次作废）或 TTL 会话租约（如 ttl=3600，一小时内边界提交免提示）。租约把"每任务一次打扰"进一步摊销成"每小时一次打扰"。

- **Phase 1（校验与内存暂存）**：对照三方版本（基线快照、活动主机文件、影子文件）。Host 未变则快进（CoW 克隆直接落地）；peer 已改则 LCS 行级对齐拼接不相交 hunk（A 在文件头插 3 行、B 改 40 行之下的函数，行号错位能对齐合并）；同基线行不相容修改则**整体中止、不碰任何主机文件**、返回精确冲突行号。
- **Phase 2（语义验证门与原子提交）**：先 `refresh_from_host()` 合并 peer 变更，再在沙箱内跑可选 `verify_cmd`（如 `rustc --emit=metadata`、`cargo check --offline`）——这能拦住**不相交行的语义冲突**（A 把第 1 行的函数改名、B 在第 40 行调用旧名，纯语法合并会产出编译不过的文件）。验证通过后原子写入主机并同步基线。验证失败的代价是延迟：快速类型检查加几十到几百毫秒，大型 monorepo 的完整集成测试要秒到分钟级——由调用方/策略选择验证强度。

回滚与缓存：`workspace_rollback` 毫秒级恢复快照；`CARGO_TARGET_DIR` 重定向到回滚树之外，回滚不废增量编译缓存（否则每次回滚都触发全量重编，事务的成本优势就没了）。

### 4.2 零信任出口代理（aos-egress）

沙箱内只有 `$AOS_SECRET_*` 不透明占位符，明文凭证从不进入进程环境/命令行/日志。身份钉扎的 TCP 代理在**域 + RFC 3986 规范化后的 HTTP 方法 + URL 路径前缀**（SecretScopeRule）都匹配时，才在网络边界注入真 token。这防住了**同域混淆代理攻击**（confused deputy）：拿绑定 `api.github.com/repos/acme/` 的 token去 `POST /gists` 公开代码、路径穿越 `/repos/acme/../../gists`、百分号编码穿越 `%2e%2e`、`DELETE /repos/acme/issues/1`——全部 403。再加出站 DLP 扫描（≤64KiB 的 query/body）查 PEM 私钥、`AKIA*`、`ghp_*`、`sk-*`、`xoxb-*` 等指纹，在白名单端点的 issue 正文里夹带 OpenSSH 私钥也会被拦。

### 4.3 A2A IPC 总线 + CAS（aos-ipc）

**MCP 与 A2A 的分工**：MCP（Model Context Protocol，Anthropic）标准化"Agent 垂直调用工具"的接口；A2A（Agent2Agent，Google 发起、现由 Linux 基金会托管）标准化"Agent 水平发现彼此能力并委托任务"。aos-ipc 把两者统一到一条磁盘同步、支持广播的总线上：

- 全局对等发现（内核 PID 存活剪枝防僵尸——macOS 用 `PROC_PIDTBSDINFO`、Linux 读 `/proc/<pid>/stat`，自动驱逐死亡或 PID 复用的对等体），`<org_id>/<agent_id>` 寻址。
- AgentCard 合成（`/.well-known/agent-card.json`），AgentSkill 标签自动反映活跃的 `coord_claim` glob。
- 有状态 A2A 任务生命周期：submitted→working（ipc_ack）→input-required（向请求方邮箱推唤醒消息）→completed/failed/canceled，输出支持 TextPart、DataPart 与 CAS FilePart。
- P2P 邮箱 + 主题 Pub/Sub、`coord_claim` 文件声明锁（glob 租约，在 pre-tool-use 与 workspace_commit 时强制，防止并行 Agent 重叠编辑）。
- 内容寻址存储：`org_artifact_publish` 把 blob 写入 CAS 并返回 `artifact://<org>/sha256:<64-hex>` 约 85–98 字节的 URI，替代 64KB 内联规格；跨组织 CAS 取件受策略 R1 门控。

### 4.4 Cedar 启发的五级策略（aos-policy）

| 级别 | 规则 | 行为 |
|---|---|---|
| R0a | 主机红线（`~/.ssh`、`/etc/shadow`、`/System` 等） | 无条件拒绝 |
| R0b | 提交时投毒守卫（`.git/hooks/*`、`.envrc`、CI workflow、`build.rs`、`Makefile`、`package.json`、`.cargo/config*` 等） | **即使持有有效租约也强制人工批准** |
| R1 | 跨租户隔离 | 拒绝/询问 |
| R2–R3 | 会话租约（one-shot ttl=0 或 TTL=3600）+ 绿区自动放行 | 零提示放行 |
| R4 | 无租约边界跨越 | 询问（一次性批准或授予租约） |

R0b 是对"租约+提示注入"组合拳的针对性防御：攻击者拿到一小时租约后试图夹带自动执行钩子，敏感路径守卫会绕过租约强制弹人审。

三条**运行时不变量**贯穿全部设计：(1) 提交前主机零干扰；(2) 沙箱零原始秘密暴露；(3) 全有或全无的冲突安全提交。

### 4.5 多客户端接入

`aegis-os install` 自动配置三家 CLI 的 MCP 条目。最有工程含量的是 agy 的 PreToolUse 钩子做的 **MCP-First 工具路由**：原生 shell/文件调用被分类——Ring-0（sudo、凭证路径）直接拒绝；主机边界命令（git push、装包）升级询问；仓库内命令则拒绝并附带一个现成的 `mcp_aegis-os_proc_exec` 载荷，把 Agent 引导到零提示沙箱路径上。

## 五、评估指标与实验证据

环境：Apple Silicon 工作站（macOS/APFS/Rust 1.99 release）+ Ubuntu 24.04 VM（btrfs/XFS/ext4，bwrap 0.9），全部真实 OS 进程与内核沙箱。

### 5.1 授权摩擦（RQ1）

**受控轨迹**：50 条确定性轨迹（覆盖 5 类 SWE 任务模板：认证 bug 修复+回归测试、多模块重构、特性实现+配额套件、安全审计+加固、多 Agent 合同同步）× 30 调用 = 1,500 次真实 `AegisRuntimeKernel::call_tool`，四种模式都执行全部 50 次 `workspace_commit`：

| 执行模式 | 提示数 | IPR | 提示减少 | 最大自主连击 | 建模等待 |
|---|---|---|---|---|---|
| Host-Interactive（逐变更提示） | 860 | 57.33% | 基线 | 4 步 | 71.7 分钟 |
| Host-Static-Allowlist（读/状态白名单） | 680 | 45.33% | 20.93% | 7 步 | 56.7 分钟 |
| Aegis（逐任务提交门 ttl=0） | 50 | 3.33% | **94.19%** | 29 步 | 4.2 分钟 |
| Aegis（1h 会话租约） | 1 | 0.07% | **99.88%** | **1,470 步**（367.5 倍） | 5 秒 |

**真实痕迹 Corpus A**（52 会话 / 57,923 步 / 28,087 工具调用：13,098 读/协调、11,070 条 shell（其中 4,318 条即 39.0% 为复合管道 `&&/|` 或内联脚本 `python3 -c`）、3,836 次文件编辑）：Ask-Every-Mutation 53.31%（14,974 提示）；Path+Prefix 白名单 28.54%；Vanilla CoW+Seatbelt 4.97%；Aegis 完整 **1.12%**（315 提示，97.9% 减少，平均 MAS 76.7 步）。Corpus B（5 个 live dogfooding 会话 / 3,962 步 / 2,040 调用，含 68 次真实 workspace_commit 与 15 次真实 policy_approve_escalation）：1.72% IPR、平均 MAS 100 步。真实痕迹里复合 shell 链（`&&`、`python3 -c`）是白名单的天敌——无法安全自动放行（每条复合命令都是潜在的沙箱逃逸通道），而内核沙箱内静默执行没有这个问题。这也解释了为什么"静态白名单 + 原地写"路线在真实负载下有 28.54% 的天花板。

### 5.2 对抗安全（RQ2）

8 类 × 10 = 80 次端到端攻击（路径逃逸/凭证收割/出口外泄/跨租户/提交投毒/L7 混淆代理+DLP/部分提交/静默覆盖）：Host-YOLO ASR 100%（按构造），**Aegis OS ASR 0.0%（0/80）**，macOS 与 Linux 完全一致。对照的无状态原生 CLI 沙箱：WS-Write+Net 85.0%（68/80——仓内破坏性命令直写活动树、dotfile 凭证可读、环境变量 token 外泄、树上构建入口可投毒）；WS-Write 无网络 60.0%。作者还诚实披露了 Linux 测试暴露的两处早期缺陷（后挂的私有 /tmp 遮蔽了工作区使 A/B 类"空过"、把"没连上代理"误计为被攻陷）并已修复——"只在沙箱配置错误时成立的安全结果不是有效结果"。

### 5.3 系统开销（RQ3）

- **供给延迟**：APFS 100 文件 0.71ms / 1,000 文件（78.1MB）6.28±0.65ms，比用户态复制循环快 13.2–18.0 倍；比真实工具 `cp -cR`（99ms）快约 16 倍、比 `git worktree add`（125ms）快约 20 倍。Linux btrfs 15.6ms（GNU `cp -R` 默认 reflink 9.9ms 略快）、XFS 32.1ms、ext4 回退 35.1ms，全 Linux 文件系统上比 rsync/git worktree/git clone --local 快 2–5 倍。10 并发 Agent 磁盘节省 94.0–97.5%。
- **运行时延迟**：策略评估 0.31µs（296 万 ops/s）、IPC 收发 1.06µs、HTTP POST /mcp 177µs、三方合并提交 424µs、Seatbelt 进程孵化 6.68ms；MCP 中介对 LLM 回合延迟的额外开销 <0.04%。跨 OS 远程 daemon（macOS 客户端→Linux daemon）fs_read 往返 0.80ms。

### 5.4 多 Agent 并发（RQ4）

3 个 Agent（agy+claude+codex）并发编辑同一 `lib.rs`：Host-Shared 保留 1/3 特性；Aegis LCS Diff3 保留 **3/3 且 cargo test 通过**。同行冲突检测 10/10（Host 0/10）；不相交行语义冲突经 `verify_cmd` 拦截 10/10（Host 0/10，主机保持 100% 可编译）。CAS 句柄把 3×64KB 规格传递从 196,608 字节（约 49,152 token）降到 294 字节（约 73 token），**IPC 载荷 token 减少 99.85%**。

### 5.5 故障恢复与消融（RQ5/RQ6）

200 个损坏模块（12.8MB）回滚 8.68±1.02ms（比逐文件恢复快 2.4 倍），全规模 `cargo check` 通过。9 行单组件关闭消融矩阵呈干净对角：关 kernel_sandbox 则 A/B/C 类 10/10 失守、关 cred_mask 则 B 10/10、关 net_isolation 则 C 10/10、关 commit_guard 则 E 10/10、关 tenant_gate 则 D 5/10（CAS 存储独立拒绝另一半——纵深防御的实证）、关 secret_binding 则 F 10/10、关 merge 则 H 10/10、关 prevalidate 则 G 10/10。作者自己承认对角阵"部分由设计保证"（每类攻击就是为锻炼对应组件写的）。

### 5.6 局限自述

论文讨论章节自己划定的边界（评审视角看相当坦诚）：

- 真实痕迹语料来自单一工程组织，更大规模跨人群的田野研究留待未来。
- 80 次攻击是为八类威胁定制的，ASR 0% 的结论只在已建模类别内成立，不排除未建模的内核或工具链绕过。
- macOS 延迟来自单台 Apple Silicon 工作站，Linux 数字来自同主机 VM（XFS 还是 btrfs 上的 loop-backed 镜像，噪声大）。
- 残余开放向量：被投毒的应用源文件随后在 Aegis OS 之外的主机上执行（未经人审 diff）；合法写权限端点上的非结构化外泄（专有代码、base64 混淆串）；HTTPS CONNECT 隧道拦截需配置会话级临时 CA。
- Seatbelt 是 Apple 标记的 legacy 接口；mach-lookup（LaunchServices job 生成）加固是未完成的未来工作；Linux 侧 Landlock、seccomp-bpf 亦未实现。

## 六、效果优势的根源解释

### 6.1 因果链：方法差异 → 机制变化 → 指标提升

**方法差异**：现有基座把授权边界放在"单个工具调用"粒度，且原地改写活动工作树——(a) 中间投机动作全部要确认 → 授权疲劳（45–57% IPR，自主连击仅 4–7 步，用户习惯性盲批）；(b) 原生 CLI 沙箱仍原地写共享树、秘密以明文进环境变量 → 85% ASR；(c) 无事务语义 → 并发覆盖（H1）与幻觉损坏不可回滚（H2）。

**机制变化**：Aegis 把授权边界移到"事务性工作区边界"。CoW 影子树使沙箱内一切变更天然可逆且不触主机（不变量 1），因此可以安全地自动放行 100% 中间动作而不牺牲安全——**可逆性换来了零提示**。明文凭证从未进入沙箱、只在 L7 域+方法+路径全匹配时于网络边界注入（不变量 2），同时封死收割（tmpfs 遮蔽 $HOME 含未知路径）与外泄（代理 403 + DLP）——**秘密不可见性换来了零外泄**。提交点收敛为单次 Diff3 三方合并 + 可选沙箱内语义验证（不变量 3），既保并发不覆盖又拦提交投毒（R0b 敏感路径绕过租约强制人审）——**单点收敛换来了冲突安全**。

**指标提升链条**：

| 机制 | 物理原因 | 指标效果 |
|---|---|---|
| 中间动作零提示 + 边界单提示 | 提示从"每变更一次"变为"每任务一次"（或每租约一次） | IPR 57.33%→3.33%/0.07%（受控）、53.31%→1.12%（真实）；连击 4→1,470 步 |
| clonefile 元数据级克隆 | 只复制 inode 元数据、共享数据块，延迟随文件数而非字节数 | 6.3ms 供给使"每任务一工作区"开销可忽略（vs git worktree 125ms 且需干净 git 历史） |
| 凭证不可见 + 出口唯一通道 | tmpfs 遮蔽 + 内核 socket filter 钉扎单端口 + L7 作用域 | 80/80 攻击全拦（对照 CLI 沙箱 68/80） |
| 三方合并 + 语义验证门 | 冲突在沙箱内暴露而非主机上 | 3/3 特性保留、10/10 冲突检测、主机 100% 可编译 |
| CAS 引用传递 | SHA-256 URI 替代内联载荷 | IPC token −99.85% |

消融对角阵从反面闭环：每个机制对其威胁类不可冗余。三处例外同样有信息量：tenant_gate 行只有 5/10——策略层放行的 5 次跨租户 IPC 被 CAS 存储的独立拒绝拦下（纵深防御），并由此催生了 broker 层租户守卫的加固；merge 消融后 4A 实验仍是 3/3——因为 `refresh_from_host` 在 commit 前已合并不相交 peer 编辑，commit 阶段的 merge 专门保护 refresh 与 commit 之间的并发窗口（H 类攻击正是打这个窗口）。

另一个容易忽略的推论：IPR 的下降不是线性的策略放宽，而是结构性的——从 Table 4 可以读出，Vanilla CoW+Seatbelt（4.97%）到 Aegis 完整版（1.12%）之间的差距全部来自"作用域动作租约"这一项策略机制，说明**隔离基础（CoW+沙箱）贡献了大部分提示消除，而租约机制又把残余提示再摊销 77%**。两层贡献可分离，意味着部署者可以按自己的风险偏好选择档位：逐任务提交门（3.33%，每次提交人审）或 1h 租约（0.07%，每小时人审一次）。

### 6.2 外部检索交叉验证

用 WebSearch 检索相似尝试与相近结论做交叉核验：

**方向一致的证据**：

- **Anthropic 官方工程博客《Making Claude Code safer with sandboxing》**（[链接](https://www.anthropic.com/engineering/claude-code-sandboxing)）报告：基于 OS 级文件系统+网络双隔离的沙箱化在内部使用中将 Claude Code 权限提示减少约 84%，且明确指出"有效沙箱必须同时有文件系统与网络隔离"。这与 Aegis 的"隔离边界内自由工作、越界才询问"同构，为"环境约束能换提示减少"提供了独立佐证。但注意量级差异：84%（原地沙箱）vs Aegis 的 90.7–97.9%（CoW+租约），差值恰好对应"Aegis 还消除了原地写共享树带来的边界提示与并发合并问题"。
- **Anthropic《How we contain Claude》**（[链接](https://www.anthropic.com/engineering/how-we-contain-claude)）披露遥测：用户批准约 93% 的权限提示、批准越多关注越少，并因此构建 auto mode 自动化更安全的批准。这直接印证了论文引用的授权疲劳失效模式，也印证" containment 优于逐动作监督"的产业判断。
- **TxOS（SOSP'09，[原文 PDF](https://www.sigops.org/sosp/sosp09/papers/porter-sosp09.pdf)）：内核级系统事务用私有副本+惰性版本管理提供 ACID 隔离**，其"投机执行于私有副本、提交时才全局可见"的思想与 Aegis 的 CoW 影子树+提交门一脉相承——Aegis 可视为把 TxOS 的内核改造路线替换为"现代文件系统 CoW 原语 + 用户态运行时"的实现选型。
- **行业沙箱实践综述**（[architectviewmaster 博客](https://www.architectviewmaster.com/blog/sandboxed-agents-giving-your-code-monkeys-their-own-sandbox)）记录了多个独立团队的相近选择：E2B/Modal/Daytona 默认 copy-in/copy-out 语义防误写源树；OverlayFS 分层挂载做"读-only 基座+可写覆盖"；Turso AgentFS 用文件级 CoW 缓解 OverlayFS 昂贵的 rename 语义；Dagger 用 git worktree+临时容器做并行可回滚。这些独立尝试分别命中了 Aegis 组件树的子集，间接支持"事务性工作区是正确方向"，也说明 Aegis 的贡献更多在**系统集成完备度**而非单项发明。

**相异或相反结论**：

- **QUASAR 等批量授权研究**（见 [Lacuna 综述](https://lacuna.tiptreesystems.com/direction/mitigating-permission-fatigue-in-autonomous-agent-security/rdr_5dd91cf09d45447082c14f391ac372db/v/1)）显示批量审批可减少 50% 以上的中断，说明"减少提示"存在多条路线（批量 vs 边界转移）。但批量授权保留了"人对聚合动作负责"的模式，未消除盲批风险；Aegis 的路线是把可逆动作彻底移出人的注意力预算。两者可互补。
- **产业界对隔离级别的判断与 Aegis 的 Tier 1 选择存在张力**：同一篇综述指出"标准 Docker 容器被认为不足以运行执行任意代码的 Agent，产业正向 microVM 收敛"，而 Aegis 的主力评测在 Seatbelt/bwrap（Tier 1），microVM 仅是控制面接口（libkrun/Firecracker 未绑定真实进程，属未来工作）。macOS 上 `/usr/bin/sandbox-exec` 已被 Apple 标记为 legacy 接口、mach-lookup 加固未做——依赖内核 MAC 强度的部分存在被绕过的长期风险，论文自己也承认 ASR 0% 只对"已建模的八类攻击"成立。
- **残余攻击面的外部印证**：论文坦承若 Agent 被合法授予某端点写权限，提示注入仍可外泄不匹配结构化秘密签名的专有源码或 base64 混淆串；HTTPS CONNECT 隧道内拦截需要配置会话级临时 CA。这与业界对 L7 DLP 的普遍局限认知一致——DLP 本质上是指纹匹配，对语义级外泄（把算法逻辑改写进 issue 描述）无能为力。

**本次检索范围内未发现**：与 Aegis 完全同构的"CoW 事务边界 + L7 秘密边界注入 + Diff3 提交门"整体系统的公开复现或独立第三方评测；也未发现反驳"提示频率与授权疲劳负相关"的实证研究。

### 6.3 顶会评审视角预判

按 USENIX Security / CCS / OSDI 的口味掂量这篇论文：

**强项**：新颖框架清晰（事务边界安全是把 TxOS/Mbox/Unionfs 的 OS 事务思想首次完整移植到 Agent 执行基座）；系统集成度高（CoW VFS + 内核沙箱 + L7 出口代理 + A2A IPC + CAS + 策略引擎，9 crate 全开源）；评测维度全面（受控轨迹 + 真实痕迹 + 红队 + 消融 + 跨文件系统 + 跨 OS 远程部署）；工程细节诚实（Linux 测试暴露的两处缺陷主动披露并修复）。

**会被攻击的点**：(1) 各单项机制均为已知技术的组合（Seatbelt/bwrap、Diff3、CoW、出口代理），novelty 在组合与事务化包装；(2) 单作者 + 单一组织痕迹语料，外部效度存疑；(3) 消融对角"部分由设计保证"是作者自陈；(4) ASR 0% 的攻击集是自建的，缺独立红队验证；(5) macOS Seatbelt legacy 接口与 mach-lookup 未加固是已知逃逸面。总体判断：新颖性中高，系统类会议有竞争力，安全顶会可能要求更大规模外部部署验证。

## 七、必要知识反推

要读懂这篇论文，读者需要预先掌握（或从论文中快速补齐）的知识清单：

1. **OS 事务与 CoW 文件系统原语**：APFS `clonefile` / Linux `FICLONE` reflink / btrfs 子卷快照的语义（元数据级克隆、块共享、写时复制触发）；ext4 无 reflink 的回退路径；git "racily clean" 两秒规则。
2. **内核强制访问控制**：macOS Seatbelt SBPL 规则语法（deny/allow subpath/regex、socket filter）；Linux Bubblewrap 命名空间（`--unshare-all`、`--ro-bind`、`--tmpfs` 挂载顺序语义）；为什么"默认拒绝 $HOME 遮蔽"优于"黑名单枚举凭证路径"；Ubuntu 24.04 AppArmor 限制非特权用户命名空间的部署现实。
3. **经典三方合并**：Diff3 / LCS 的行级对齐原理，语法冲突 vs 语义冲突的区分（为什么不相交行也能产生编译失败）。
4. **网络边界安全**：L7 代理、身份钉扎、RFC 3986 路径规范化（为什么 `%2e%2e` 编码穿越需要规范化防御）、混淆代理攻击、DLP 指纹匹配。
5. **Agent 协议栈**：MCP（Anthropic 的垂直工具协议，JSON-RPC over stdio/HTTP）与 Google A2A（水平发现/任务委托）的分工；AgentCard、内容寻址存储。
6. **可用安全**：授权疲劳/习惯化的实证文献（Felt et al. 的 Android 权限研究、Akhawe & Felt 的 Alice in Warningland）。
7. **策略引擎设计**：AWS Cedar 授权语言的分层思想；租约（TTL lease）作为"时间换提示"的授权原语。
8. **评测方法论**：IPR/MAS/ASR 的定义；消融实验的对角矩阵解读；"按构造 100%"基线的含义（Host-YOLO 列的信息量在对照组而非自身）。

一个"最小知识包"判断：如果读者只有一小时，优先补 CoW 文件系统原语（第 1 条）与 Seatbelt/bwrap 沙箱语义（第 2 条）——论文 80% 的机制细节建立在这两块地基上；Diff3 与授权疲劳文献可以用"是什么"级别的了解带过。

## 八、通用性灵感

超出编码 Agent 场景本身的启发：

1. **把授权边界对齐到不可逆性，而不是对齐到动作**。任何"AI 代理执行+人监督"的系统（浏览器操作 Agent、运维 Agent、数据分析 Agent）都可以问：哪些操作在隔离副本内天然可逆？哪些是真正的边界跨越？提示预算应该只花在后者。这是"注意力预算守恒"的架构化表达。
2. **可逆性是可以用基础设施批量制造的**。CoW 让"给每次尝试一个一次性世界"的成本降到 6.3ms，于是"试错自由"不再是安全让步而是默认态。任何高误伤成本的领域（数据管道、基础设施变更、批量邮件）都值得寻找自己的"clonefile"。
3. **秘密的可见性最小化要在边界注入**。占位符进沙箱、真值在最后一条边界上按"域+方法+路径"作用域注入——这个模式适用于一切需要给不可信执行环境配凭证的场景（CI job、笔记本环境、插件系统）。比起"把凭证藏起来"，"让凭证从未存在过、直到最后一刻在正确的目的地成形"是更强的安全属性。
4. **默认拒绝优于黑名单枚举**。Linux tmpfs 遮蔽整个 $HOME 再放行工具链的做法，对"未知未知"凭证路径免疫；反观黑名单式枚举（原生 CLI 沙箱挡 `.ssh/.aws/.gnupg` 却漏了 `.npmrc`）总是可被枚举差击败。
5. **长期有效的自动化授权要有"敏感路径旁路"**。TTL 租约省提示，但 R0b 证明租约本身会被投毒攻击利用——任何"预先授权"机制都需要一个不可租约化的红线子集。审计自动化系统的读者可以拿 `.git/hooks`、CI 配置、构建脚本清单直接自查。
6. **提交前在沙箱内做语义验证，是比语法合并更强的完整性门**。Diff3 只能保证"行不重叠"，`verify_cmd` 才保证"合并结果可编译"。这个"语法层+语义层双门"的模式可迁移到任何合并场景：配置合并后跑 dry-run、SQL 变更合并后跑 EXPLAIN、策略合并后跑一致性检查。
7. **单作者+开源全量基准的工程范式**。9 个 crate、全部实验脚本可复现（`cargo run --release -- bench`）、诚实披露 Linux 测试暴露的缺陷——这种"把红队做成 CI"的做法值得任何安全基建项目效仿。
8. **大载荷引用化是上下文经济学的通用解**。CAS 句柄 −99.85% token 的思路，对任何多 Agent 系统的中间产物传递（搜索结果、日志、数据集切片）都适用：传哈希不传内容，取件时再施加访问控制。
9. **"每任务一个一次性环境"正在成为 Agent 基础设施的设计共识**。Aegis 的 CoW、业界 E2B 的 copy-in/copy-out、OverlayFS 分层、git worktree 配对都在逼近同一个抽象：Agent 的草稿与世界的正式状态之间必须有显式的事务边界。读到这里的工程师可以自查自己的 Agent 栈：你的 Agent 写的每一行，是先落在草稿上，还是直接写在正本上？
10. **对评测数字保持架构级怀疑**。"Host-YOLO 100% 按构造"与"消融对角部分由设计保证"两处自陈提醒我们：安全评测的结论边界比结论本身更重要——0/80 只在八类已建模攻击内成立。
