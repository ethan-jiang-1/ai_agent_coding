# Codex CLI Advanced Agentic Features Reference

来源：

- OpenAI 官方文档：`Subagents`
- OpenAI 官方文档：`Slash commands`
- OpenAI 官方文档：`CLI features`
- OpenAI 官方文档：`Config reference`
- OpenAI 官方文档：`Codex App worktrees`
- 本机 `codex --help`
- 本机 `codex cloud --help`
- 本机 `codex cloud exec --help`
- 本机 `codex cloud status --help`
- 本机 `codex features list`

核验日期：2026-04-09
本机版本：`codex-cli 0.118.0`

---

## 1. 本次只确认三条能力线

- `sub agent / multi-agent / task delegation`
- `worktree / workspace isolation`
- `long horizon / long-running / detached execution`

原则：

- 有官方依据的，写成正式能力
- 只有近似入口的，写成近似能力
- 当前官方没给 CLI 能力面的，不硬补

---

## 2. Subagents：Codex CLI 当前是正式能力

官方 `Subagents` 页面当前明确写了：

- Codex 有内建 subagents
- built-in agents 是 `default`、`worker`、`explorer`
- custom agents 可放在 `~/.codex/agents` 或项目内 `.codex/agents`
- custom agents 使用 standalone TOML files
- 运行时可用 `/agent` 查看、切换和管理 agent 线程

官方 `Config reference` 当前还明确给出：

```toml
[agents]
enabled = true
max_threads = 6
max_depth = 1
job_max_runtime_seconds = 120
```

这说明：

- multi-agent 不是只存在于营销文案里
- 并行深度和线程数有正式配置面
- child agent 默认运行时长也是被控制的

本机 `codex features list` 还能确认：

- `multi_agent` 当前是 `stable true`
- `enable_fanout` 仍是 `under development false`

因此当前更稳的结论是：

- Codex CLI 的 subagents 已经是正式能力
- 但更激进的 fan-out 不应写成默认稳定能力

官方文档还说明：

- child agent 会继承 parent agent 的 approval policy 和 sandbox
- parent agent 的 runtime overrides 也会传给 child

这直接影响权限实践：多代理不是天然更自由，默认是继承上层护栏。

补充注意：

- 官方 `Subagents` 页面仍提到 `/approvals` 和 `--yolo` 这类旧说法
- 但当前 slash commands 页面与本机帮助更接近 `/permissions` 和更长的危险模式参数名

实践上应以当前 slash surface 与本机帮助为主，同时保留这类版本漂移提示。

---

## 3. Worktree：当前要区分 Codex CLI 和 Codex App

官方确实有正式 worktrees 文档，但它落在：

- `Codex App > Worktrees`

官方对 worktrees 的描述是：

- 为每个任务创建独立分支
- 使用隔离文件系统
- 所有 worktree 都来自同一个 repo

但当前 Codex CLI 官方文档与本机 `codex --help` 里，没有发现：

- 顶层 `worktree` 子命令
- CLI 内建 worktree 管理命令
- 与 `codex cloud` 对等的 CLI worktree 调度面

`Config reference` 里当前只出现了一条与 worktree 相邻的说明：

- `projects.<path>.trust_level` 可以标记一个 project 或 worktree 的信任级别

这不足以把 worktree 写成 Codex CLI 的第一方命令能力。

因此当前更准确的结论是：

- `worktree` 是 Codex 生态里的正式概念
- 但官方正式文档当前把它放在 `Codex App`
- 对 `Codex CLI` 来说，worktree 目前更应当视为外部 Git 隔离手段，而不是内建命令面

---

## 4. Long Horizon / Long-Running：CLI 当前有两层，不要混写

### A. 本地会话连续运行

本机帮助当前明确支持：

- `codex resume`
- `codex fork`
- `codex exec resume`

这解决的是：

- 会话续跑
- 方案分叉
- 在同一本地工作流里继续推进

它们不是 detached long-running job。

### B. Codex Cloud detached tasks

本机帮助当前明确有：

- `codex cloud list`
- `codex cloud exec`
- `codex cloud status`
- `codex cloud diff`
- `codex cloud apply`

其中 `codex cloud exec --help` 还能确认：

- 必须指定 `--env <ENV_ID>`
- 可指定 `--branch <BRANCH>`
- 可指定 `--attempts <ATTEMPTS>`
- `--attempts` 是 best-of-N

这说明 Codex CLI 当前已经有正式的 detached cloud task 面：

- 可提交
- 可轮询状态
- 可查看 diff
- 可本地 apply

对 long horizon 来说，这比单纯 `resume` 更接近真正的异步长程执行。

### C. 交互态里的背景终端观察面

官方 slash commands 当前还列出：

- `/ps`
- `/statusline`

`Config reference` 还给出：

- `background_terminal_keepalive_polls = 8`
- `background_terminal_max_timeout_ms = 300000`

这说明 Codex CLI 还有一层“会话内背景终端 / 运行中状态”的交互面。

但它和 `codex cloud` 不是一回事：

- `/ps` 更像会话内观察与管理
- `codex cloud` 更像 detached 任务提交与取回

---

## 5. 对 practice 写作的直接影响

### 对多智能体

- 可以正式写 subagents
- 可以正式写 built-in roles
- 可以正式写 custom agents
- 不能把更激进的 fan-out 写成默认稳定能力

### 对 worktree

- 要明确写成“CLI 当前没有第一方 worktree 命令面”
- 真要做文件系统隔离，应用外部 Git worktree
- 不要把 `Codex App worktrees` 误写成 `Codex CLI commands`

### 对 long horizon

- `resume / fork` 写在会话连续性
- `codex cloud` 写在 detached long-running
- `/ps` 这类交互命令写在会话内观察层

---

## 6. 当前最容易写错的地方

- 不要把 `/agent` 写回旧名字 `/agents`
- 不要把 `/permissions` 写回旧名字 `/approval-mode`
- 不要把 `Codex App worktrees` 误写成 `Codex CLI` 自带命令
- 不要把 `resume` 和 `cloud task` 混成同一种长程能力
- 不要把 `enable_fanout` 这种本机仍为开发中的特性，写成默认稳定能力
