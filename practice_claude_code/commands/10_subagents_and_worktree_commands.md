# 10 Subagents 与 worktree

## Claude Code 自带的内置 subagents

官方当前明确写了这些 built-in subagents：

- `Explore`
- `Plan`
- `General-purpose`

适合：

- `Explore`：快速、只读的代码搜索和摸底
- `Plan`：plan mode 里的只读研究
- `General-purpose`：需要探索加修改的复杂任务

## 临时定义 subagents

```bash
claude --agents '{"reviewer":{"description":"Reviews code","prompt":"You are a code reviewer","tools":"Read,Glob,Grep","permissionMode":"plan"}}'
```

适合：先在当前 run 里塞一个临时角色，不急着落文件。

## 选择当前 agent

```bash
claude --agent reviewer "审查当前 diff"
```

`--agent` 是把当前主会话本身切成某个 agent 类型，不只是让 Claude 临时委派一个子任务。

## 查看与管理 agents

```bash
claude agents
```

会话内对应：

- `Type B` `/agents`：在交互里查看、创建、编辑或删除 subagents

## 持久化 agent 文件位置

```text
.claude/agents/
~/.claude/agents/
```

当前官方格式是 Markdown + YAML frontmatter，例如：

```md
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Glob, Grep
model: sonnet
permissionMode: plan
---
You are a code reviewer. Focus on correctness, risks, and missing tests.
```

## worktree

```bash
claude --worktree
claude --worktree feature-auth
claude --tmux --worktree feature-auth
```

当前官方明确写了：

- worktree 创建在 `<repo>/.claude/worktrees/<name>`
- branch 命名为 `worktree-<name>`
- 默认从 `origin/HEAD` 指向的默认远端分支切出

适合：

- 为一个独立特性开干净工作区
- 在 worktree 里跑实现任务
- 用另一个 worktree 或会话做独立审查

## subagent worktrees

官方还支持在 subagent 定义里写：

```yaml
isolation: worktree
```

这适合：

- 让 subagent 平行工作但不碰主工作区
- 自动清理无改动的临时 worktree

## 一个典型组合

```bash
claude --worktree feature-auth
```

或者保守手工做法：

```bash
git worktree add ../project-auth -b feature-auth
cd ../project-auth
claude
```

## 这章最容易写错的地方

- 不要不给职责边界就开多个 subagent。
- 不要让多个代理修改同一块代码。
- 不要忘了 worktree 之间共享 auto-memory。
- 不要把更重的 `agent teams` 混进这章；它属于第 `13` 章。
