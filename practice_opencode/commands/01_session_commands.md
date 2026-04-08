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

更准确的映射关系是：

- `/sessions` 不是单独等于某一个外部命令
- 它更接近 `opencode session list` + `--continue` / `--session` 的组合

所以它是典型的 Type B combined mapping。

## 这组 slash commands 对习惯层的更深影响

- `/new` 让“任务边界变了就新开”不再是一个高摩擦动作。你不需要退出 TUI 才能重置任务。
- `/clear` 只是 `/new` 的别名，不是“把当前线程清空后继续”。
- `/sessions` 让你在会话内部就能切历史会话，所以“继续哪条链”变成运行中持续决策，而不是启动前一次性决策。

## 这章最容易写错的地方

- 不要把 `--continue` 当默认入口。
- 不要把 fork 理解成“自动清理上下文污染”。
- 不要在任务边界已经变化时还继续原会话。
