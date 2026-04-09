# 08 工具、guardrails 与 MCP

## 前台 Agent 的主要工具面

官方文档能确认 Cursor Agent 至少覆盖这类工具：

- Search / Grep / Codebase
- Read File / List Directory
- Edit / Delete File
- Terminal
- Web
- MCP

## 前台最重要的边界

前台 Agent 适合：

- 先探索，再小范围执行
- 保留人工审批
- 保留 terminal guardrails

## 条件性的窄编辑通道

如果你的当前 Cursor 构建里仍然有 `Manual`，它适合明确只改你选中的文件。

如果没有，更稳的替代做法是：

- 用 `Agent` 但收紧工具范围
- 或者用 `Custom Mode` 把工具组合限制住

## Auto-run with guardrails

Cursor 官方强调 terminal 可以自动运行，但要配 guardrails。

实践上最重要的是：

- allowlist 尽量窄
- 未确认方向前，不要放开高风险命令
- 把“能跑命令”和“该不该跑命令”分开看

## MCP

MCP 适合：

- 第三方上下文拉取
- 工具接入
- 结构化外部数据

但默认原则仍然是：只接当前任务真需要的 MCP server。

## 适合沉淀成 custom command 的工具动作

这次补查官方 `Commands` 页后，能确认几类和这一章高度相关的示例命令：

- `security-audit`
- `code-review-checklist`
- `run-all-tests-and-fix`
- `light-review-existing-diffs`

这类命令的价值是：

- 把 review / audit / test-fix 流程做成稳定入口
- 让团队重复执行的 guardrail 动作不再完全依赖临时提示词
- 把“要不要跑工具、怎么审结果”先结构化

更稳的理解是：

- mode / guardrails 决定能不能做
- custom command 只在真的高频复用时，决定这次怎么做

## 背景 agent 的不同风险面

Background Agents 的 terminal 自动化程度明显更高，所以权限边界必须和前台 Agent 分开理解。

官方当前还能确认这些额外边界：

- Background Agent 运行在隔离远程 Ubuntu 机器
- agent 会 auto-run terminal commands
- 有 internet access
- 如果启动时关闭 Privacy Mode，中途再打开，当前 run 也不会立刻切回 Privacy Mode

所以这章真正要防的是：

- 前台 Agent 的“人工审批心态”被错误带到后台
- remote terminal 自动化带来的 prompt injection / exfiltration 风险

## 这章最容易写错的地方

- 不要把前台 Agent 和 Background Agents 的权限模型写成一样。
- 不要只靠提示词说“别动文件、别跑命令”。
- 不要把 `Manual` 写成当前所有 Cursor 文档都稳定列出的模式。
- 不要默认把所有 MCP 都开着。
- 不要忘了 Background Agent 的 Privacy Mode 切换不是即时回溯生效。
