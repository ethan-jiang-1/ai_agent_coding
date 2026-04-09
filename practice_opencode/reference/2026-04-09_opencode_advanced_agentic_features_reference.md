# OpenCode Advanced Agentic Features Reference

核验日期：2026-04-09

## 来源

- OpenCode Docs: `Agents`
- OpenCode Docs: `CLI`
- OpenCode Docs: `Server`
- OpenCode Docs: `Web`
- OpenCode Docs: `Commands`
- 已有本地 reference：`2026-04-09_opencode_official_reference.md`

## 1. 本次只确认三条能力线

- `subagents / subtasks / agent orchestration`
- `worktree / workspace isolation`
- `long horizon / multi-client / long-running`

原则：

- 有官方 docs 的，写成正式能力
- 当前 docs 没有的，明确写“没有稳定证据”
- 不把别家工具的 worktree / background job 概念硬套到 OpenCode

## 2. Subagents：OpenCode 当前是正式能力

官方 `Agents` docs 当前明确写了：

- 两类 agents：`primary agents` 与 `subagents`
- built-in primary agents：`build`、`plan`
- built-in subagents：`general`、`explore`
- primary agents 可用 `Tab` 或 keybind 切换
- subagents 可自动被 primary agent 调起，也可手动 `@general`、`@explore`

官方还明确写了这些重要边界：

- `plan` 默认把 file edits 和 bash 收成 `ask`
- `general` 可做复杂研究，也可改文件
- `explore` 是快速、只读 subagent
- `permission.task` 可限制某个 primary agent 经 Task tool 能调用哪些 subagents
- 但用户始终可以通过 `@` 直接调用 subagent

因此对 OpenCode 来说：

- 多 agent 协调的正式主轴是 subtask / subagent
- 不应只写成 `/agents` 菜单操作

## 3. Worktree：当前没有稳定官方证据

这次复核的官方 docs 范围里，没有稳定证据显示 OpenCode 当前有：

- 第一方本地 worktree 命令面
- 类似 `claude --worktree` 的原生隔离工作区
- 类似 Cursor Background Agent 那样的独立 branch worker

因此更稳的结论是：

- OpenCode 当前 docs 没有把 worktree 写成正式一等能力
- 如果你真的需要 filesystem 级并行隔离，应视为外部 Git 技术，而不是 OpenCode 官方 feature

这不是缺陷描述，而是事实边界。

## 4. Long horizon：OpenCode 当前更像“多客户端同状态”，不是调度器

官方 `Server` / `Web` / `CLI` docs 当前明确写了：

- 运行 `opencode` 时，会同时启动 TUI 和 server
- server 暴露 OpenAPI 3.1 endpoint
- `opencode serve` 可启动 standalone headless server
- `opencode web` 可启动浏览器界面
- `opencode attach <url>` 可让 TUI 接到已运行 backend
- `opencode run --attach http://localhost:4096 ...` 可附着到已有 server，避免 MCP cold boot

官方还明确说明：

- web 与 terminal 可同时使用
- 它们会共享相同 sessions 和 state
- `/tui` endpoint 可通过 server 驱动 TUI
- 这一架构被 OpenCode IDE plugins 使用

因此 OpenCode 当前最接近 long horizon / advanced coordination 的正式能力不是 scheduler，而是：

- server-backed shared state
- multi-client access
- attach / web / tui 共用同一 backend

## 5. Share / export / import 仍然是长程协作的重要组成部分

官方 docs 当前明确支持：

- `/share` / `/unshare`
- `opencode export`
- `opencode import`
- 从公开 share link 重新导入

这意味着 OpenCode 的长程协作层至少有两类：

- 同一 backend 上的 shared session/state
- 通过 share/export/import 做跨机器、跨客户端、跨人传递

但这仍然不等于：

- 有独立后台 worker
- 有定时调度
- 有 durable autonomous job runner

## 6. 对 practice 写作的直接影响

### 对多智能体

- 正式写 `build/plan/general/explore`
- 正式写 `@subagent`
- 正式写 `subtask: true`
- 可以写 `/agents` 作为当前 agent list / switch 入口
- 但不要把多 agent 主轴缩成 `/agents` 菜单

### 对 worktree

- 明确写“当前 reviewed docs 里没有第一方 worktree 能力”
- 如果需要隔离工作区，把它视为外部 Git 技术

### 对 long horizon

- 不要写成 scheduler / background worker 工具
- 更准确地写成 `serve` / `web` / `attach` 的多客户端共享状态架构
- share/export/import 是长程协作与 handoff 的补充层

## 7. 当前最容易写错的地方

- 不要把 `@explore` / `@general` 写成 detached background workers
- 不要把 `serve` / `web` / `attach` 写成“定时任务”或“后台 agent”
- 不要把 absence of worktree 补成假功能
- 不要把 share/export/import 混成同一种状态传递
