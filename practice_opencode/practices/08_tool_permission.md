# 08 工具权限克制：默认全开不代表默认合理

## 核心习惯

OpenCode 官方 docs 当前明确写到，默认允许所有操作而不要求显式审批。正因为如此，你更应该主动收权限，而不是靠提示词说“先别改代码”。

## 在 OpenCode 里怎么做

- 不确定方向时，优先用 `plan` agent。
- 对 `edit` 和 `bash` 先收成 `ask`，必要时再放开。
- 对高风险命令做 pattern 级规则，不要只写一个宽泛的 allow。
- 对只读 agent 或 reviewer agent，明确把写和 bash 能力关掉。

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

## 验收标准

- 当前 permission 和任务阶段匹配。
- 高风险命令有明确的 ask / deny 规则。
- reviewer / planner 一类 agent 的能力边界是清楚的。

## 对应支撑

见 `../commands/08_permissions_and_tools_commands.md`。
