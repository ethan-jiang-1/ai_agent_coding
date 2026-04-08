# 02 重试与分叉命令

## 回滚文件后重试

```bash
git restore --source=HEAD --worktree src/auth/login.ts

claude "重新实现登录函数，必须处理 null、空字符串和数据库异常"
```

适合：方向错了，要从干净文件状态重新来。

## 继续原任务只补条件

```bash
claude --continue "只修改中间件，不动测试，保留现有接口"
```

适合：目标没变，只是补充漏掉的约束。

## 保留历史但试另一条路径

```bash
claude --resume <session-id> --fork-session \
  "按方案 B 实现登录流程，保持现有测试结构"
```

适合：你想保留旧历史，但不想污染原会话。

## 真正的干净重试

```bash
git restore --source=HEAD --worktree <target-files>
claude "重新实现，按这版提示词来"
```

如果你要的是“干净上下文重来”，通常应优先新开，而不是 fork。

## 这章最容易写错的地方

- 不要把 `--fork-session` 当作去除负向 few-shot 的万能工具。
- 不要在一个已经污染的会话里无限追加补丁。
- 不要用粗暴回滚代替精确回滚目标文件。
