# 13 高级 Agentic 协调：先分 subtask、共享 backend、handoff 层

## 核心习惯

OpenCode 的高级 agentic 协调，不该被笼统地写成“多开几个 agent”。

更稳的理解是先分四层：

- primary agent / subagent
- subtask 级独立上下文
- `serve` / `web` / `attach` 的共享 backend
- `share` / `export` / `handoff` 的长程协作层

先选最薄的一层，不要把这些能力混成一种东西。

## 在 OpenCode 里怎么做

- 只是角色拆分，用 `build`、`plan`、`@explore`、`@general`。
- 需要独立上下文研究并压缩结果返回主线程，用 `subtask: true`。
- 需要 terminal、web 或别的客户端继续同一状态，用 `serve` / `web` / `attach`。
- 需要公开分享、跨机器搬运或人类交接，用 `share` / `export` / `handoff`。
- 需要真正文件系统隔离时，明确转到外部 Git worktree，不要把它写成 OpenCode 官方内建能力。

一个简单判断顺序：

1. 这是不是只需要独立研究上下文
2. 这是不是要多个客户端接同一 backend
3. 这是不是要给别人看、给别人接，还是要导入导出状态
4. 这是不是已经超出 OpenCode 原生能力，要借外部 Git 或别的调度系统

## OpenCode 的特殊边界

- subagent / subtask 是正式能力
- `serve` / `web` / `attach` 是共享 backend，不是 scheduler
- `share` / `export` / `import` 是长程协作补充层，不是后台 worker
- 这次 reviewed docs 范围里，没有稳定证据显示 OpenCode 当前有第一方 worktree 或 durable background jobs

## 不要这样做

- 不要把 `@explore` 或 `@general` 写成 detached worker。
- 不要把 `serve` / `attach` 写成 scheduler、cron 或后台 agent。
- 不要把 share/export/import 混成同一种状态传递。
- 不要把 absence of worktree / background jobs 硬补成存在的功能。

## 验收标准

- 你能明确说出当前依赖的是 subtask、shared backend，还是 share/export/handoff。
- 如果是多客户端接续，backend 和客户端边界是清楚的。
- 如果是人类交接，下一步、风险和状态不是只藏在会话历史里。
- 如果需要真正的 worktree 或后台任务，你明确说明那是外部能力，不是 OpenCode 原生命令面。

## 对应支撑

见 `../commands/13_advanced_agentic_coordination_commands.md`。
