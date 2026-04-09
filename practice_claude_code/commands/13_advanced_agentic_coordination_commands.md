# 13 高级 Agentic 协调命令

Claude Code 到这一步要分开四件事：

- `subagents`
- `worktree`
- `agent teams`
- `scheduled automation`

这些都和“多 agent”有关，但不是同一种能力。

更稳的升级顺序通常是：

- 先 subagents
- 再 worktree
- 再 teams
- 最后 durable scheduling

## 1. 更轻的并行：subagents

会话内入口：

```text
/agents
```

启动前入口：

```bash
claude --agents '{...}'
claude --agent reviewer
claude agents
```

适合：

- 探索、实现、审查这类角色拆分
- 结果最终仍回到主会话
- 不需要 worker 彼此直接通信

## 2. 文件隔离：`--worktree`

```bash
claude --worktree feature-auth
claude --tmux --worktree feature-auth
```

适合：

- 多个实现任务并行跑
- 需要避免文件互相踩踏
- 希望 Claude 自带 worktree 创建与清理

subagent 也可通过 frontmatter 开：

```yaml
isolation: worktree
```

## 3. 更重的并行：agent teams

先启用：

```bash
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

或者在 `settings.json` 里放：

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

然后直接用自然语言要求：

```text
Create an agent team to review this PR: one teammate on security, one on performance, one on test coverage. Wait for teammates to finish before you synthesize.
```

当前官方明确写了：

- lead 负责建队、派任务、综合结果
- teammates 是独立 Claude Code sessions
- teammates 可彼此直接通信
- teams / tasks 存在 `~/.claude/teams/` 和 `~/.claude/tasks/`

重要边界：

- 这是 experimental
- token 成本显著高于单一 session
- teammates 启动时继承 lead 的 permission settings
- 如果 subagents 或 worktree 已够用，不要为了“任务更大”就直接升级到 teams

## 4. 会话内长程：`/loop`

```text
/loop 5m check if the deployment finished and tell me what happened
/loop 20m /review-pr 1234
```

当前官方明确写了：

- `/loop` 是 session-scoped
- 最多 50 个 scheduled tasks
- recurring tasks 7 天后自动过期
- 关闭终端、会话退出、重启后都不会保留

如果要彻底禁用：

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

## 5. Durable scheduling：Cloud / Desktop / GitHub Actions

当前官方把 durable scheduling 分成三类：

- Cloud scheduled tasks：Anthropic 云端跑，CLI 里经 `/schedule` 配
- Desktop scheduled tasks：本机桌面端跑，持久
- GitHub Actions：CI 里跑，适合 repo event 和 cron

GitHub Actions 示例：

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "Generate a daily summary of open issues and PRs"
    claude_args: "--max-turns 5 --model sonnet"
```

同时要配：

- workflow timeout
- concurrency controls

## 6. 长任务提醒

如果你只是把 Claude 放后台跑一阵子，而不是做完整调度，更稳的辅助面是 `Notification` hook。

它适合：

- Claude 等你批权限时提醒
- Claude 空闲完成时提醒
- 长任务跑完提醒

## 7. 一张快速决策表

- 只需要角色拆分：subagents
- 需要真正文件隔离：`--worktree`
- workers 需要彼此通信和共享任务：agent teams
- 只在当前 session 里轮询一阵：`/loop`
- 要跨重启、跨离线、跨 CI：Cloud / Desktop / GitHub Actions

## 这章最容易写错的地方

- 不要把 subagents 和 agent teams 写成同一种东西。
- 不要把 `/loop` 当成 durable scheduling。
- 不要忽略 agent teams 的 experimental 状态和更高 token 成本。
- 不要把 GitHub Actions 文档里的 `--max-turns` 直接写成“本机 help 已稳定暴露”的常规参数。
