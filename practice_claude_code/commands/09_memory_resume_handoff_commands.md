# 09 memory、resume 与 handoff

## 继续会话

```bash
claude --continue
claude --resume <session-id>
claude --resume
```

## 会话命名

```bash
claude --name "oauth2-migration-phase-1"
```

## 分叉会话

```bash
claude --resume <session-id> --fork-session
```

## auto-memory 路径

```text
~/.claude/projects/<project_name>/memory/MEMORY.md
```

已知行为：

- Claude 会自动更新项目级经验
- 新会话启动时会自动读入前 200 行或前 25KB
- 同一 Git 仓库的不同 worktree 共享这份 memory

## `--bare` 与 memory

```bash
claude --bare
```

`--bare` 会跳过 auto-memory。

## handoff 文件建议

对跨天、跨人或跨阶段任务，仍然建议另写 handoff 文件，而不是只靠 memory 或 resume。

## 这章最容易写错的地方

- 不要把 auto-memory 当成完整对话历史。
- 不要把 auto-memory 当成当前任务快照。
- 不要把 resume 当成跨人协作的唯一方案。
