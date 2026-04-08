# 01 会话与恢复命令

## 开新会话

```bash
claude
```

Claude Code 默认启动交互式会话。

## 继续最近会话

```bash
claude --continue
```

适合：同一目录里继续最近一次逻辑未完成的任务。

## 恢复指定会话

```bash
claude --resume <session-id>
claude --resume
```

- 给 ID：恢复指定会话
- 不给 ID：进入选择器

## 从旧会话分叉

```bash
claude --resume <session-id> --fork-session
claude --continue --fork-session
```

适合：保留历史，但在新 session ID 上尝试另一条路径。

## 给会话命名

```bash
claude --name "oauth2-migration-planning"
```

适合：让后续 `--resume` 选择器更好认。

## 会话内对应 slash commands

- `Type A` `/resume [conversationId]`：会话内恢复历史会话，对应 `--resume` / `--continue`。
- `Type C` `/clear`：清空当前历史，作用更像“会话内重开”，不是 `--fork-session` 的等价物。
- `Type A` `/rename <name>`：在交互中重命名当前会话，对应启动时的 `--name`。
- `Type C` `/exit` `/quit`：结束当前交互会话。

## 这章最容易写错的地方

- 不要把 `--fork-session` 当成“干净重试”。
- 不要把所有任务都续在 `--continue` 上。
- 不要忘了 Claude Code 默认保存会话。
