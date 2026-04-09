# Claude Code Advanced Agentic Features Reference

核验日期：2026-04-09  
本机版本：`2.1.96 (Claude Code)`

## 来源

- Claude Code Docs: `Create custom subagents`
- Claude Code Docs: `Run agent teams`
- Claude Code Docs: `Common workflows`
- Claude Code Docs: `Run prompts on a schedule`
- Claude Code Docs: `GitHub Actions`
- 本机 `claude --help`
- 本机 `claude agents --help`

## 1. 本次只确认三条能力线

- `subagents / task delegation`
- `worktree / workspace isolation`
- `long horizon / long-running / scheduled automation`

原则：

- 官方正式写出来的，按正式能力整理
- 文档有但本机 help 不可见的，标成“文档面已出现，但本机不可发现”
- 没有证据的，不硬补

## 2. Subagents：Claude Code 当前是正式一等能力

官方 `Create custom subagents` 页面当前明确写了：

- Claude Code 自带 built-in subagents：`Explore`、`Plan`、`General-purpose` 以及其他 helper agents
- 每个 subagent 有自己的 context window、tool access、independent permissions
- 多个并行且需要互相通信的 worker，官方明确建议看 `agent teams`

自定义 subagent 当前支持两条正式路径：

- 文件方式：Markdown + YAML frontmatter，放在 `.claude/agents/` 或 `~/.claude/agents/`
- 启动参数：`--agents <json>`，字段与 file-based frontmatter 对齐

官方列出的关键 frontmatter 字段包括：

- `tools`
- `disallowedTools`
- `model`
- `permissionMode`
- `maxTurns`
- `memory`
- `background`
- `isolation`

这意味着 Claude Code 的 subagent 能力不只是“换一个 prompt”，而是正式支持：

- 权限模式
- agentic turns 上限
- persistent memory
- 后台运行
- worktree 隔离

## 3. Worktree：Claude Code 当前是正式 CLI 能力

和 Codex CLI 不同，Claude Code 当前有明确的一等 CLI worktree 面。

本机 `claude --help` 当前明确列出：

- `-w, --worktree [name]`
- `--tmux` requires `--worktree`

官方 `Common workflows` 页面进一步确认：

- `claude --worktree feature-auth` 会创建 `.claude/worktrees/feature-auth/`
- worktree branch 命名为 `worktree-<name>`
- worktree 默认从 `origin/HEAD` 对应的默认远端分支切出
- 可通过 `WorktreeCreate` hook 替换默认创建逻辑
- `.worktreeinclude` 可复制 gitignored 文件到新 worktree

官方还明确写了：

- subagents 也可以使用 `isolation: worktree`
- 无改动的 subagent worktree 会自动清理

因此对 Claude Code 来说，worktree 不是外部补丁，而是正式内建能力。

## 4. Agent teams：比 subagents 更重的一层，且仍属实验能力

官方 `Run agent teams` 页面当前明确写了：

- agent teams 是 experimental
- 默认关闭
- 需要 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`
- 一个 lead 协调多个 teammates
- teammates 各自独立 session、独立 context、可彼此直接通信
- teams 与 tasks 存在本地：`~/.claude/teams/` 与 `~/.claude/tasks/`

官方同时明确区分了 subagents 和 agent teams：

- subagents：在单一 session 内并行，结果回到主线程
- agent teams：跨独立 sessions 协作，有共享 task list 和 mailbox

权限与成本边界也写得很清楚：

- teammates 启动时继承 lead 的 permission settings
- 若 lead 用 `--dangerously-skip-permissions`，teammates 也会继承
- agent teams 的 token 使用显著高于单一 session

本机 `claude --help` 没有列出 `--teammate-mode`，但 agent teams 官方文档已经写出：

- `teammateMode` 可放在 `~/.claude.json`
- 文档面存在 `claude --teammate-mode in-process`

因此更稳的结论是：

- agent teams 是官方正式文档里的能力
- 但仍是 experimental
- 某些控制面已经在文档里出现，但本机 help 还不可发现

## 5. Long horizon：Claude Code 当前有三层

### A. 会话内短期续跑

本机 help 当前明确支持：

- `--continue`
- `--resume`
- `--fork-session`

这解决的是会话连续性，不是 autonomous scheduling。

### B. 会话内调度：`/loop`

官方 `Run prompts on a schedule` 页面当前明确写了：

- `/loop` 是 bundled skill
- session-scoped
- session 最多持有 50 个 scheduled tasks
- recurring tasks 7 天后自动过期
- `CLAUDE_CODE_DISABLE_CRON=1` 可禁用

官方还明确写了限制：

- Claude Code 只有在运行且 idle 时才会触发这些任务
- 关闭终端或会话结束，任务全部取消
- 重启后不会恢复

### C. Durable scheduling

同一官方页面当前把 durable scheduling 明确分成三类：

- Cloud scheduled tasks：Anthropic-managed infrastructure，持久，配置经 `/schedule`
- Desktop scheduled tasks：跑在本机，持久
- GitHub Actions：跑在 CI

也就是说，Claude Code 的 long horizon 不应只写成 `/loop`。

## 6. GitHub Actions：官方明确给出 `--max-turns`，但要限定在该表面

官方 `GitHub Actions` 页面当前明确写了：

- `claude_args` 可透传 Claude Code CLI arguments
- 官方示例直接使用 `--max-turns`
- 文档还建议用 `--max-turns`、workflow timeout、concurrency controls 控制 runaway jobs

但本机 `claude --help` 当前没有列出 `--max-turns`。

因此更稳的写法是：

- 可以把 `--max-turns` 写进 GitHub Actions / automation surface
- 不要直接把它写成本机常规 CLI help 已稳定暴露的参数

## 7. 对 practice 写作的直接影响

### 对多智能体

- `subagents` 写进第 `10` 章
- `agent teams` 写进新增第 `13` 章
- 两者不能混成同一种并行能力

### 对 worktree

- Claude Code 可以正式写 `--worktree`
- 也可以正式写 subagent `isolation: worktree`
- 这比“手工 git worktree + 切目录”更靠近第一方能力

### 对 long horizon

- `--continue / --resume / --fork-session` 写在会话连续性
- `/loop` 写在 session-scoped scheduling
- `/schedule`、Desktop、Cloud、GitHub Actions 写在 durable automation

## 8. 当前最容易写错的地方

- 不要把 subagents 和 agent teams 写成一回事
- 不要把 `/loop` 当成 durable scheduling
- 不要忽略 agent teams 的 experimental 状态和更高 token 成本
- 不要把 GitHub Actions 文档里的 `--max-turns`，直接写成“本机 help 已稳定暴露”的常规参数
