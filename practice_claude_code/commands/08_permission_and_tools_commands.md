# 08 权限模式与工具控制

## 规划模式

```bash
claude --permission-mode plan "分析 src/auth/ 并给出计划"
```

## 常见 permission mode

当前本机帮助列出的值有：

- `acceptEdits`
- `auto`
- `bypassPermissions`
- `default`
- `dontAsk`
- `plan`

## 工具白名单

```bash
claude --allowedTools "Read Edit" "只读和编辑，禁止 Bash"
claude --allowedTools "Bash(git:*) Edit" "只允许特定 Bash 范围"
```

## 工具黑名单

```bash
claude --disallowedTools "Bash" "不要运行 shell"
```

## 完整工具集控制

```bash
claude --tools "" "只用知识回答，不调用工具"
claude --tools "default" "使用默认工具集"
```

## MCP 来源收窄

```bash
claude --mcp-config sentry.json "分析生产错误"
claude --mcp-config sentry.json --strict-mcp-config "只使用这份 MCP 配置"
```

## 危险跳过权限

```bash
claude --dangerously-skip-permissions "..."
```

只应在外部已沙箱环境中使用。

## 这章最容易写错的地方

- 不要只靠提示词说“先别改代码”。
- 不要把所有工具默认全开。
- 不要在不可信环境里用危险跳过权限。
