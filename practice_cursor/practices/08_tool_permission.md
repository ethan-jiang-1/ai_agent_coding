# 08 工具权限克制：用 mode 和 guardrails 管边界

## 核心习惯

Cursor 的权限控制不是一组 CLI 沙箱参数，而是通过 `Ask` / `Manual` / `Agent` / `Background Agents` 这些工作模式，加上 terminal guardrails、审批和 MCP 开关来实现。

## 在 Cursor 里怎么做

- 规划或读代码阶段，优先用 `Ask`。
- 只想让 AI 改你明确选中的文件时，用 `Manual`。
- 需要搜索、读写、跑命令时，再进 `Agent`。
- 后台长跑任务才用 `Background Agents`，并先收紧边界。
- MCP 只接你当前任务真需要的服务器。
- 对 review、安全审计、测试修复这类重复动作，可以用 `.cursor/commands` 做固定入口，而不是每次重写 guardrail 提示词。

最实用的理解：

- `Ask`：先只读分析
- `Manual`：窄范围编辑
- `Agent`：前台执行
- `Background Agents`：远程自动执行，权限边界要更严

## 不要这样做

- 不要只靠提示词说“先别动文件”。
- 不要在不确定阶段默认给最强执行权。
- 不要把前台 Agent 和 Background Agents 的风险级别写成一样。

## 验收标准

- 当前 mode 和任务阶段匹配。
- 工具集没有超过实际需要。
- 高频 guardrail 动作已经有可复用 command 承载。
- 背景 agent 的权限、仓库和目标边界是清楚的。

## 对应支撑

见 `../commands/08_tools_and_guardrails_commands.md`。
