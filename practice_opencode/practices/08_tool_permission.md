# 08 工具权限克制：默认全开不代表默认合理

## 核心习惯

OpenCode 官方 docs 当前明确写到，默认允许所有操作而不要求显式审批。正因为如此，你更应该主动收权限，而不是靠提示词说“先别改代码”。

## 在 OpenCode 里怎么做

- 不确定方向时，优先用 `plan` agent。
- 对 `edit` 和 `bash` 先收成 `ask`，必要时再放开。
- 对高风险命令做 pattern 级规则，不要只写一个宽泛的 allow。
- 对只读 agent 或 reviewer agent，明确把写和 bash 能力关掉。
- 如果 primary agent 会调 subagents，把 `permission.task` 也一起定义，不要只盯着 `edit` / `bash`。
- OpenCode 当前没有一个稳定的 built-in `/permissions` 可以现场救场，所以最好在 config 和 agent 层先把边界写好。
- 任务一旦进入 subtask 编排，权限设计也要一起升级；这层边界见第 `13` 章。

最实用的理解：

- `allow`：自动执行
- `ask`：先问你
- `deny`：直接挡住

## 为什么这比口头约束更可靠

因为：

- 提示词只是希望它别做
- `permission` 才是工具层真正决定它能不能直接做

## 不要这样做

- 不要只靠自然语言说“别动文件”。
- 不要把所有 bash 都一次性放开。
- 不要忘了 pattern 规则是最后匹配项获胜，顺序写错会让权限失控。
- 不要只限制 Task tool，却忘了你自己仍然可以用 `@subagent` 直接调起子代理。

## 验收标准

- 当前 permission 和任务阶段匹配。
- 高风险命令有明确的 ask / deny 规则。
- reviewer / planner 一类 agent 的能力边界是清楚的。
- 你能说清全局 permission、agent permission 和 `permission.task` 这三层谁在管什么。

## 对应支撑

见 `../commands/08_permissions_and_tools_commands.md`。
