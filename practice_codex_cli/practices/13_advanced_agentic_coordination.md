# 13 高级 Agentic 协调：先分角色隔离、文件隔离、运行时隔离

## 核心习惯

任务一旦进入并行、多阶段、长程执行，先不要急着“多开几个 agent”。先判断你需要的到底是哪一种隔离：

- 角色隔离
- 文件系统隔离
- 运行时隔离

把这三种隔离混在一起，是高级 agentic 工作里最常见的低质量动作。

## 在 Codex CLI 里怎么做

- 需要探索者、实现者、审查者分工时，用 subagents 或多条 `codex exec` 链。
- 需要真正避免写入互相踩踏时，用外部 Git worktree，再让 Codex 进不同目录工作。
- 需要人不守着、任务异步跑、结果稍后取回时，用 `codex cloud`。
- 只是继续当前任务或分叉方案时，用 `resume` / `fork`，不要过度升级到 detached task。
- 如果后面要切到 `codex cloud`，先把计划和 handoff 写清，不要直接把模糊任务丢进 detached run。

一个简单判断顺序：

1. 这是角色冲突，还是文件冲突
2. 这是继续当前会话，还是 detached 长程执行
3. 这个任务真的值得多代理，还是只需要更清楚的 handoff

## Codex CLI 的特殊边界

- subagents 是正式能力
- `multi_agent` 当前是稳定开启的
- 但更激进的 fan-out 还不该写成默认稳定做法
- 官方 worktree 文档当前属于 `Codex App`，不是 `Codex CLI` 顶层命令面
- `codex cloud` 是 detached 长程任务面，不等于本地 `resume`
- 正确问题是“何时从交互式续跑切到 detached cloud”，不是“长任务默认都该上 cloud”

## 不要这样做

- 不要让两个会写文件的 agent 共用同一个工作树改同一块代码。
- 不要把 subagent 误当成文件系统隔离。
- 不要把 `resume` 硬写成“长程任务能力”。
- 不要把 `Codex App worktrees` 误说成 `Codex CLI` 的 slash command 或子命令。
- 不要在没有 branch、env、计划文件或取回策略的情况下直接上 detached cloud task。

## 验收标准

- 你能明确说出这次用的是角色隔离、文件系统隔离，还是运行时隔离。
- 如果有并行写入，写入范围是隔离的。
- 如果是长程任务，你有明确的 task id、branch、env 或 handoff。
- 如果用了 subagents，每个角色职责不重叠。

## 对应支撑

见 `commands/13_advanced_agentic_coordination_commands.md`。
