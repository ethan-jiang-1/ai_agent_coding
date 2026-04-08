# 10 Subagents 与 worktree

## 临时定义 subagents

```bash
claude --agents '{"reviewer":{"description":"Reviews code","prompt":"You are a code reviewer"}}'
```

## 选择当前 agent

```bash
claude --agent reviewer "审查当前 diff"
```

## 查看已配置 agents

```bash
claude agents
```

## 会话内对应 slash commands

- `Type B` `/agents`：在交互里查看或管理 agents。
- 外部入口更适合启动前准备；会话内入口更适合任务中途切角色或检查 agent 配置。

## 持久化 agent 文件位置

```text
.claude/agents/
~/.claude/agents/
```

## worktree

```bash
claude -w
claude -w feature-x
claude --tmux -w feature-x
```

适合：

- 为一个独立特性开干净工作区
- 在 worktree 里跑实现任务
- 用另一个 worktree 或会话做独立审查

## 一个典型组合

```bash
git worktree add .worktrees/oauth2 -b feature/oauth2
cd .worktrees/oauth2
claude
```

## 这章最容易写错的地方

- 不要不给职责边界就开多个 subagent。
- 不要让多个代理修改同一块代码。
- 不要忘了 worktree 之间共享 auto-memory。
