# 02 重试与分叉命令

## 回滚文件后重跑

```bash
git restore --source=HEAD --worktree src/auth/login.ts

codex exec --ephemeral \
  "重新实现登录函数，必须处理 null、空字符串和数据库异常"
```

适合：非交互式任务走偏，想用更好的提示词从干净状态重来。

## 交互式分叉另一种方向

```bash
git restore --source=HEAD --worktree src/auth/login.ts

codex fork --last \
  "按另一种方案实现登录函数，保留现有接口，不要引入新依赖"
```

适合：你想保留探索历史，但要在新会话里走另一条路线。

## 非交互式任务的正确重试方式

```bash
codex exec --ephemeral "第一次尝试"
# 方向错了
git restore --source=HEAD --worktree <target-files>
codex exec --ephemeral "第二次尝试，带上更完整约束"
```

当前 CLI 没有 `codex exec fork` 这种工作流。`exec` 的“Edit & Rerun”本质上就是：

- 回滚文件
- 改好提示词
- 重新跑一条新命令

## 这章最容易写错的地方

- 不要在污染过的会话里无限追加“还有一个条件”。
- 不要把 `fork` 当文件回滚工具。
- 不要用激进的 `git reset --hard` 代替精确回滚目标文件。
