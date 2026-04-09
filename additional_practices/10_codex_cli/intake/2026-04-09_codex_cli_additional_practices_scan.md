# 2026-04-09 Codex CLI Additional Practices Scan

Run ID: `2026-04-09_additional_practices_r1`
Tool: `Codex CLI`
Step: `Step 2B. Tool External Scan`
Pass: `Tier A + Tier B`
Effective Window: `2026-01-01` to `2026-04-09`
Accessed: `2026-04-09`

## 本轮查询与发现路径

主要查询：

- `Codex CLI release notes 2026`
- `Codex CLI changelog 2026`
- `Codex CLI subagents cloud worktrees docs`
- `Codex CLI slash commands docs`

发现路径：

1. 先从 OpenAI 官方 docs / changelog 入口收敛到当前 Codex 文档树
2. 再分别打开 CLI features、CLI reference、CLI slash commands、subagents、feature maturity
3. 补 OpenAI 官方 blog / engineering posts，优先看 `2026-01-01` 之后与 Codex CLI 相关的文章

## Source Ledger

| Date / State | Tier | Source | Why it matters |
| --- | --- | --- | --- |
| 2026-01-14 | A | [API changelog: `gpt-5.2-codex` release](https://developers.openai.com/api/docs/changelog) | 确认 `gpt-5.2-codex` 在当前窗口内进入 Responses API |
| 2026-01-25 | A | [GitHub release `0.91.0`](https://github.com/openai/codex/releases/tag/rust-v0.91.0) | 直接确认 sub-agent guardrail 被收紧到最多 `6` 个 |
| 2026-01-31 | A | [GitHub release `0.93.0`](https://github.com/openai/codex/releases/tag/rust-v0.93.0) | 直接确认 plan mode streaming、`/apps`、smart approvals 默认开启 |
| 2026-02-02 | A | [GitHub release `0.94.0`](https://github.com/openai/codex/releases/tag/rust-v0.94.0) | 直接确认 plan mode default、personality stable、skills from `.agents/skills` |
| 2026-02-05 | A | [GitHub release `0.97.0`](https://github.com/openai/codex/releases/tag/rust-v0.97.0) | 直接确认 allow-and-remember approvals、live skill updates、`/debug-config`、initial memory plumbing |
| 2026-02-05 | A | [GitHub release `0.98.0`](https://github.com/openai/codex/releases/tag/rust-v0.98.0) | 直接确认 steer mode stable by default |
| 2026-01-23 | A | [OpenAI Engineering: Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/) | 直接解释 Codex CLI 的 agent loop、prompt assembly、compaction、AGENTS / skills / MCP 影响 |
| 2026-02-02 | A | [OpenAI Product: Introducing the Codex app](https://openai.com/index/introducing-the-codex-app/) | 提供多代理、worktrees、skills、automations、personality 的官方产品信号，但要区分 app 与 CLI 边界 |
| 2026-02-03 | A | [API changelog: GPT-5.2 and GPT-5.2-Codex now ~40% faster](https://developers.openai.com/api/docs/changelog) | 说明性能变化可能改变实际使用习惯 |
| 2026-02-05 | A | [OpenAI Product: Introducing GPT-5.3-Codex](https://openai.com/index/introducing-gpt-5-3-codex/) | 强化 long-running、interactive steering、Codex 全平台能力信号 |
| 2026-02-10 | A | [API changelog: server-side compaction](https://developers.openai.com/api/docs/changelog) | compaction 从手动习惯走向平台原生能力 |
| 2026-02-10 | A | [API changelog: Skills in Responses API](https://developers.openai.com/api/docs/changelog) | skills 不再只是局部补充，而是进入平台 execution 面 |
| 2026-02-25 | A | [GitHub release `0.105.0`](https://github.com/openai/codex/releases/tag/rust-v0.105.0) | 直接确认 CSV fan-out、`/copy`、`/clear`、灵活 approval controls |
| 2026-03-02 | A | [GitHub release `0.107.0`](https://github.com/openai/codex/releases/tag/rust-v0.107.0) | 直接确认 thread fork into sub-agents、clear memories |
| docs, accessed 2026-04-09 | A | [Codex CLI features](https://developers.openai.com/codex/cli/features) | 当前 CLI 的交互式、审批、subagents、cloud 入口 |
| docs, accessed 2026-04-09 | A | [Codex CLI command line options](https://developers.openai.com/codex/cli/reference) | 当前 CLI 命令面、`codex cloud`、`exec resume` 等参数事实 |
| docs, accessed 2026-04-09 | A | [Codex CLI slash commands](https://developers.openai.com/codex/cli/slash-commands) | 当前 built-in slash commands 列表与行为定义 |
| docs, accessed 2026-04-09 | A | [Codex Subagents](https://developers.openai.com/codex/subagents) | subagents 的默认值、继承行为、custom agents、实验性 fan-out |
| docs, accessed 2026-04-09 | A | [Codex Feature Maturity](https://developers.openai.com/codex/feature-maturity) | 官方对 `under development / experimental / beta / stable` 的解释 |
| docs, accessed 2026-04-09 | A | [Codex web overview](https://developers.openai.com/codex/cloud) | 当前 Codex cloud 产品说明，但其中有 CLI 支持陈旧描述，需和 CLI docs 交叉核对 |
| 2026-01-04 | B | [JP Caparas: Quick Start](https://jpcaparas.medium.com/codex-cli-quick-start-agents-md-better-prompts-safer-runs-36d7060fcf68) | 明确展示 day-to-day 安全工作流、AGENTS.md、clarifying questions、plan-and-verify、resume/review |
| 2026-02-07 | B | [JP Caparas: Definitive Guide](https://jpcaparas.medium.com/the-definitive-guide-to-codex-cli-from-first-install-to-production-workflows-a9f1e7c887ab) | 展示 months of daily use 后的默认 config、approval/sandbox 选择、`/debug-config`、skills 和生产工作流 |
| updated 2026-04-04 | B | [Blake Crosley: Definitive Technical Reference](https://blakecrosley.com/guides/codex) | 提供 daily flow、plan mode、cloud task recipe、AGENTS.md / skills / profiles / workflow recipes |
| 2026-01-28 | B | [Lulla et al.: On the Impact of AGENTS.md Files](https://arxiv.org/abs/2601.20404) | 提供 AGENTS.md 对运行时间和 token 消耗的实证支持 |
| 2026-02-12 | B | [Gloaguen et al.: Evaluating AGENTS.md](https://arxiv.org/abs/2602.11988) | 提供 AGENTS.md 促进更广泛探索但不总能提升成功率的反证与边界 |

## Tier A 关键事实

### 0. 当前窗口内的官方 release 序列，已经足够说明 Codex CLI 的行为面在快速变化

按官方 GitHub release notes，可直接确认：

- `2026-01-25` 的 `0.91.0`
  - 最大 sub-agent 数从更高值收紧为最多 `6`
- `2026-01-31` 的 `0.93.0`
  - plan mode 有了专门 TUI 视图
  - `/apps` 进入 TUI
  - smart approvals 默认开启
- `2026-02-02` 的 `0.94.0`
  - plan mode 默认开启
  - personality 稳定化
  - skills 开始从 `.agents/skills` 加载
- `2026-02-05` 的 `0.97.0`
  - allow-and-remember approvals
  - live skill updates
  - `/debug-config`
  - initial memory plumbing
- `2026-02-05` 的 `0.98.0`
  - steer mode 默认稳定开启
- `2026-02-25` 的 `0.105.0`
  - `spawn_agents_on_csv`
  - `/copy`
  - `/clear`
  - 更灵活的 approval controls
- `2026-03-02` 的 `0.107.0`
  - thread 可直接 fork into sub-agents
  - `codex debug clear-memories`

这说明：

- `Step 2B` 不应该只看“今天 docs 里有什么”
- 还必须看这几个变化是在当前窗口内怎样逐步出现的

Implication:

- `session_kickoff_discipline` 现在至少要重新审计 plan mode default 之后的开局方式
- `command_surface_fluency` 不能只看旧版 slash surface
- `delegation_subagent_orchestration` 明显在当前窗口内发生了多次强化
- `memory / compaction / steer` 已经开始改变会话连续性的默认节奏

### 1. `codex cloud` 已经是 Codex CLI 的正式终端入口

当前 CLI features 文档明确写道：

- `codex cloud` 可以在终端里浏览活跃或已完成任务，并把变更应用到本地项目
- `codex cloud exec --env ENV_ID` 可以直接从终端提交任务
- `--attempts` 支持 `1-4` 的 best-of-N 运行

对应命令参考页也明确给出：

- `codex apply` 可把最近的 Codex cloud diff 应用回本地仓库
- `codex cloud` 是正式命令组，而不是“即将支持”

这和 `Codex web` 概览页里那句“CLI support for cloud delegation is coming soon”存在冲突。对 CLI 事实判断，应以更具体、更新的 CLI docs 为准，而不是用概览页的旧描述。

Implication:

- `long_horizon_control` 在 Codex CLI 上的官方支撑明显增强
- 这更像 `09` / `13` 章的 merge 强化，而不是全新章节

### 2. current slash surface 比现有基线稿更宽

当前官方 slash commands 页面明确列出并定义了：

- `/fast`
- `/personality`
- `/debug-config`
- `/statusline`
- `/permissions`
- `/agent`
- `/status`
- `/ps`

其中几个点对当前基线特别重要：

- `/fast` 支持线程级快速模式开关
- `/personality` 支持 `friendly`、`pragmatic`、`none`
- `/debug-config` 直接暴露 config layer 和 policy diagnostics
- `/approvals` 仍可作为 alias 使用，但不再出现在 slash popup 里
- `/sandbox-add-read-dir` 当前被官方限定为 Windows only

Implication:

- `command_surface_fluency` 的官方事实层更强了
- 当前窗口内已知时间点包括：
  - `2026-01-31`: `/apps`
  - `2026-02-05`: `/debug-config`
  - `2026-02-25`: `/copy` / `/clear` 的更明确工作流角色
- 当前 `practice_codex_cli/practices/12_interactive_only.md` 和对应 commands/reference 可能需要补入这些新的高价值交互层入口
- 但这些点本身更像 command-surface merge，不一定各自值得升格为独立 practice

### 3. Subagents 的继承规则和默认边界更清晰了

当前官方 subagents 文档明确给出：

- built-in agents 仍是 `default`、`worker`、`explorer`
- custom agent 文件放在 `~/.codex/agents/` 或 `.codex/agents/`
- 可选字段如 `model`、`sandbox_mode`、`mcp_servers`、`skills.config` 默认继承 parent session
- subagents 继承当前 sandbox policy
- 非交互流程里如果无法获取新 approval，需要新批准的动作会失败并回传到 parent workflow
- `agents.max_threads` 默认 `6`
- `agents.max_depth` 默认 `1`
- `spawn_agents_on_csv` 是 experimental

Implication:

- `delegation_subagent_orchestration` 的能力事实层更强
- 当前窗口内的关键时间点包括：
  - `2026-01-25`: 最大 sub-agent 数收紧到 `6`
  - `2026-02-25`: `spawn_agents_on_csv`
  - `2026-03-02`: thread fork into sub-agents
- 这更支持“可控拆分”而不是激进递归 fan-out
- 默认 `max_depth = 1` 是一个行为级信号，说明官方仍然在鼓励浅层 delegation

### 4. compaction 和 context management 已经进一步平台化

`Unrolling the Codex agent loop` 官方工程文明确认：

- Codex 会在 prompt 中聚合多层指令来源，而不仅是单一文件
- `AGENTS.override.md`、`AGENTS.md`、skills metadata 都会进入 assembled context
- Codex 会自动在 token 超过阈值时触发 compaction
- 早期需要用户手动触发 `/compact`
- 现在已经会自动使用 `/responses/compact` endpoint

Implication:

- `06` 稳定前缀 / 上下文压缩 不是空白，但需要补充“自动 compaction 时代的使用习惯”
- 这更像 merge 到既有 `04` / `05` / `06`，不明显支持单独新 practice

### 4.1 plan mode 和 steer mode 在当前窗口里都从“可用”走向“默认”

官方 release notes 直接确认：

- `2026-02-02` 的 `0.94.0`
  - plan mode default
- `2026-02-05` 的 `0.98.0`
  - steer mode stable by default

Implication:

- `session_kickoff_discipline` 和 `recovery_replay_resume_hygiene` 的行为面都被抬高了
- 这两个变化更像强化既有 `01` / `03` / `09`，而不是强行新建章节

### 5. OpenAI 官方在 2026 窗口内持续强化“多代理 + 长任务 + 可交互监督”

`Introducing the Codex app` 和 `Introducing GPT-5.3-Codex` 都在强化同一方向：

- 多个 agent 并行
- 长时间运行
- 中途持续互动和监督
- skills / automations / worktrees 让 agent 协作更像完整工作流

但要注意边界：

- built-in worktrees、automations 目前首先是 app 级能力
- 不能直接写回成 Codex CLI 自带 worktree 子命令
- 对 Codex CLI 的意义更多是“产品哲学变了，CLI 用户也更值得学习这些行为模式”

Implication:

- `workspace_worktree_isolation` 和 `long_horizon_control` 的跨产品信号更强
- 但 CLI 内是否应升格为新 practice，仍需看 CLI 自身 adoption 和 workflow 证据

### 6. `gpt-5.2-codex` 与 `gpt-5.3-codex` 的窗口内变化会影响实际使用节奏

当前窗口内的模型侧变化包括：

- `2026-01-14`: `gpt-5.2-codex` 进入 Responses API
- `2026-02-03`: `gpt-5.2-codex` 运行约快 `40%`
- `2026-02-24`: `gpt-5.3-codex` 进入 Responses API
- `2026-02-05` 官方产品文还强调：GPT-5.3-Codex 更适合 long-running tasks，且可在运行中持续 steer

Implication:

- `model_provider_routing_upgrade` 可能不只是“换个模型名字”，而是影响什么时候值得长跑、什么时候值得交互监督
- 这更像 `07` / `11` / `13` 的 merge 强化

## 当前最强候选方向

基于当前 Tier A 官方扫描，Codex CLI 里最值得继续核实的方向是：

1. `command_surface_fluency`
2. `long_horizon_control`
3. `delegation_subagent_orchestration`
4. `workspace_worktree_isolation`
5. `session_kickoff_discipline`
6. `recovery_replay_resume_hygiene`

## Tier B 采用层信号

### 1. 日常工作流已经明显收敛到“安全默认 + 明确开局 + review 回路”

JP Caparas 在 `2026-01-04` 的 quick-start 里，把可复用的日常工作流压成了：

- 不靠危险全开模式起步
- 用 `AGENTS.md` 保持稳定约束
- 先澄清再执行
- 长任务用 plan-and-verify loop
- 用 resume / review 接续工作

而他在 `2026-02-07` 的长文里又给出一个 months-of-daily-use 的默认配置：

- `approval_policy = on-request`
- `sandbox_mode = workspace-write`
- `model_personality = concise`
- skills、shell snapshot、request_rule 等默认开启

这两篇独立写作共同指向一件事：

- 对 Codex CLI 用户来说，可靠工作方式已经不是“直接扔个 prompt 看看”
- 而是先把 session 的执行姿态钉住

Implication:

- `session_kickoff_discipline` 有明确 adoption signal
- `approval_profiles_and_remembered_grants` 也有行为层信号

### 2. 高频用户已经把 slash surface 当成控制平面，而不是零散快捷键

JP 的 definitive guide 把 `/debug-config`、`/compact`、`/review`、`/skills` 放在“第一天就该知道”的入口层。

Blake Crosley 的 workflow recipes 则把命令面进一步编进具体工作流：

- 新项目用 `/init`
- 日常开发用 `/diff`
- 复杂重构先用 `/plan`
- cloud debugging 用 `codex cloud status` / `diff` / `apply`

这说明：

- 高信号用户并不是把 slash commands 当“补充功能”
- 而是在用它们切换控制态、检查配置、复盘 diff、管理长任务

Implication:

- `command_surface_fluency` 已经有重复 adoption signal
- 但更像 merge 到第 `12` 章，而不是拆出单独新 practice

### 3. long-running 工作流开始出现明确的“本地回路 / cloud 委派”分界

Blake 的 guide 里已经把：

- 本地 CLI
- cloud tasks
- desktop app

明确分成不同 surface，并给出 cloud debugging recipe：

- `codex cloud exec`
- `codex cloud status`
- `codex cloud diff`
- `codex apply`

这和官方 CLI docs 的 cloud 命令面形成闭环，说明 long-running 已不只是功能存在，而是已有可重复 recipe。

Implication:

- `long_horizon_control` 有 adoption signal
- 但当前更像强化 `09` / `13` 的阈值判断，而不是独立新章

### 4. AGENTS / skills 的使用已经形成“写少而准”的反向约束

Blake 的 guide 把 `AGENTS.md` 和 skills 都放进 daily user 的核心阅读路径，同时强调：

- `AGENTS.md` 是项目级 operating contract
- skills 是按需加载的可复用 expertise
- profiles 可以替代手工切 flag

两篇研究给这件事加了一个重要边界：

- `2601.20404` 指出 AGENTS.md 可降低运行时间和 token 消耗
- `2602.11988` 指出 AGENTS.md 会诱导更广泛探索，但不一定提高成功率，且上下文文件过重会带来成本

这组信号合在一起说明：

- Codex CLI 的行为升级不是“更多 AGENTS / 更多技能越好”
- 而是“稳定合同写少而准，重流程知识尽量放到可按需加载的技能或外部文件”

Implication:

- `skills_over_agents_bloat` 是真实候选
- 但 adoption signal 仍弱于 `session_kickoff_discipline` 和 `command_surface_fluency`

### 5. delegation 的采用层信号存在，但仍明显落后于 capability 层

官方在当前窗口内连续强化了 subagents。

但在高信号外部材料里，真正稳定、清晰的“多 subagent 默认工作流”还不如：

- 计划先行
- 明确 review 回路
- local vs cloud surface 分工

Blake 甚至明确把很多 delegation pattern 指向 cloud tasks 或 SDK orchestration，而不是鼓励所有用户直接堆复杂 subagent 树。

Implication:

- `delegation_subagent_orchestration` 的 capability 层很强
- 但 adoption 层目前仍更像“浅层、可控、职责清晰”的使用，而不是激进 fan-out

## Tier B 后的收敛

当前 Codex CLI 的 intake 已经足以进入候选提炼，原因是：

- `Tier A` 已覆盖官方 docs、官方 blog、官方 changelog、官方 releases
- `Tier B` 已补到至少两位独立高信号作者和两篇原始研究
- 新增信号已经主要在加密现有判断，而不是持续生成全新候选

因此按预算规则，当前工具应停止继续外扫，进入 `Step 2C. Tool Candidate Extraction`。

## 当前更像 merge 的方向

- `06` 稳定前缀 / compaction
- `07` 模型路由
- `08` 权限策略
- `09` resume / handoff / cloud
- `10` 多智能体分流
- `12` interactive-only / slash command layer
- `13` 高级 agentic 协调

也就是说，当前尚未看到足够强的 Tier A 证据，证明 Codex CLI 一定需要一个全新的正式 practice 章节；更大的可能是：

- 某几个新行为会显著强化现有第 `12` / `13` 章
- 或者把已有行为从“命令层补充”推进到“更明确的 practice 叙述”

## 文档漂移与谨慎点

### `codex cloud` 文档漂移

- `Codex web` 概览页仍写着 CLI support coming soon
- 但 CLI features / CLI reference 已明确把 `codex cloud` 写成正式终端入口

当前判断：

- 对 Codex CLI 的事实核验，应优先采用 CLI 专页
- 把概览页视为旧描述

### `/status` 的壳层命令漂移

- slash docs 说 `/status` 会展示“像 `codex status` 一样”的摘要
- 但 CLI reference 当前没有 `codex status` 顶层命令

当前判断：

- 这更像文档文案漂移
- 不能据此新增不存在的 shell 子命令

## 下一步

- 补 `Tier B pass`
  - 找可信工程团队 / KOL / 高信号公开实战，验证这些能力是否真的改变了使用习惯
- 完成后进入 `Step 2C`
  - 把官方扫描与采用信号收敛为受控候选
