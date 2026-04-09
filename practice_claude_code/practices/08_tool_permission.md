# 08 工具权限克制：工具白名单比口头约束更可靠

## 核心习惯

Claude Code 里，权限控制不只是“开不开工具”，而是可以细化到 permission mode、工具白名单和 MCP 来源。

## 在 Claude Code 里怎么做

- 规划阶段，用 `--permission-mode plan`。
- 只允许少量工具时，用 `--allowedTools` 或 `--tools`。
- 调第三方 MCP 时，需要时用 `--strict-mcp-config` 收窄来源。
- 已经在交互里时，用 `/permissions` 和 `/mcp` 调整当前会话边界；不要靠自然语言含糊约束。
- 如果用了 subagents 或 agent teams，记得它们不会天然形成更窄权限；父会话如果开太大，子层也会跟着变大。

最实用的理解：

- `plan`：先只读分析
- `default` / `auto`：正常执行
- `--allowedTools`：把能力白名单收窄
- `--tools ""`：彻底禁用工具

## 不要这样做

- 不要只靠提示词说“别动文件”。
- 不要在不确定阶段给全套工具。
- 不要把项目里所有 MCP 都默认开给每次任务。

## 验收标准

- 当前 permission mode 和任务阶段匹配。
- 工具集没有超过实际需要。
- 第三方 MCP 的来源边界是明确的。
- 多代理路径没有偷偷扩大权限面。

## 对应支撑

见 `../commands/08_permission_and_tools_commands.md`。
