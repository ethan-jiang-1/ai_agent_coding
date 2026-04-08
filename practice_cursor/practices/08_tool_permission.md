# 08 工具权限克制：用 mode 和 guardrails 管边界

## 核心习惯

Cursor 的权限控制不是一组 CLI 沙箱参数，而是通过 `Ask`、`Agent`、`Background Agents`、terminal guardrails、审批和 MCP 开关来实现；如果你的当前 Cursor 构建里仍然有 `Manual` 或等价的窄编辑模式，再把它当补充入口。

## 在 Cursor 里怎么做

- 规划或读代码阶段，优先用 `Ask`。
- 只想让 AI 在更窄的边界里改文件时，优先收紧 mode 和工具范围；如果当前构建里仍有 `Manual`，再按需使用它。
- 需要搜索、读写、跑命令时，再进 `Agent`。
- 后台长跑任务才用 `Background Agents`，并先收紧边界。
- MCP 只接你当前任务真需要的服务器。
- 如果团队里 review、安全审计、测试修复这类动作真的重复很多次，再考虑用 `.cursor/commands` 做固定入口。

最实用的理解：

- `Ask`：先只读分析
- `Agent`：前台执行
- `Background Agents`：远程自动执行，权限边界要更严

## 不要这样做

- 不要只靠提示词说“先别动文件”。
- 不要在不确定阶段默认给最强执行权。
- 不要把前台 Agent 和 Background Agents 的风险级别写成一样。

## 验收标准

- 当前 mode 和任务阶段匹配。
- 工具集没有超过实际需要。
- 背景 agent 的权限、仓库和目标边界是清楚的。

## 对应支撑

见 `../commands/08_tools_and_guardrails_commands.md`。
