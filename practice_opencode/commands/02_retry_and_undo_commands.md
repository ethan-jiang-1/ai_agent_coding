# 02 回退、重试与分叉

## 会话内回退

```text
/undo
/redo
```

OpenCode 官方文档当前说明：

- `/undo` 会撤销最近一次用户消息、后续响应以及相关文件改动
- `/redo` 只在 `/undo` 之后可用
- 这套文件恢复依赖 Git

## 用 `/undo` 后重试

典型节奏：

1. `/undo`
2. 重写提示词，把遗漏条件补清楚
3. 再让当前会话继续

适合：只是刚刚那一步实现走偏了，但任务边界没变。

## 方向错了就分叉或新开

```bash
opencode --continue --fork
```

适合：原历史值得保留，但这次明显是新路线。

## 更精确的文件回滚

```bash
git restore --source=HEAD --worktree <file>
```

适合：

- 你只想恢复少量文件
- 不想把整个最近回合都回退
- 项目里已经有你要保留的其他改动

## snapshots 与这章的关系

OpenCode docs 当前能确认：

- snapshots 默认开启
- `snapshot: false` 会关闭 UI 回滚能力

所以如果你依赖 `/undo` 做高频试错，就不要把 snapshots 关掉。

## 这章最容易写错的地方

- 不要把 `/undo` 当成长期版本管理。
- 不要在已经明显换方案时还用追加提示词硬救。
- 不要忘了 Git / snapshot 条件不满足时，回退体验会变差。
