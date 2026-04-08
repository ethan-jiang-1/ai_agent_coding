# 01 会话与恢复命令

## 开新 TUI 会话

```bash
opencode
```

默认进入 OpenCode 的 TUI。

## 继续最近会话

```bash
opencode --continue
opencode -c

opencode run --continue "继续上一阶段，先总结当前阻塞"
```

适合：同一逻辑任务还没结束，只是接着做。

## 恢复指定会话

```bash
opencode --session <session-id>
opencode run --session <session-id> "读取上文后继续实现"
```

适合：你明确知道要回哪条历史会话。

## 从历史会话分叉

```bash
opencode --continue --fork
opencode --session <session-id> --fork

opencode run --session <session-id> --fork \
  "基于原有分析，改走方案 B"
```

适合：保留历史，但这次明确是另一条路径。

## 列出历史会话

```bash
opencode session list
opencode session list --format json
```

适合：先看会话清单，再决定 resume 还是 fork。

## TUI 内对应 slash commands

- `/help`：先看当前命令面。
- `/new`：开启新会话。别名 `/clear`。
- `/sessions`：列会话并切换。别名 `/resume`、`/continue`。
- `/quit` `/q`：退出当前 TUI。

## 这章最容易写错的地方

- 不要把 `--continue` 当默认入口。
- 不要把 fork 理解成“自动清理上下文污染”。
- 不要在任务边界已经变化时还继续原会话。
