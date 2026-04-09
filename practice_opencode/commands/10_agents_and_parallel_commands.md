# 10 agents、subtasks 与并行分流

## 当前内建 agents

官方 docs 当前确认：

- primary agents：
  - `build`
  - `plan`
- subagents：
  - `general`
  - `explore`

## 显式指定 agent

```bash
opencode --agent plan

opencode run --agent build \
  "按 docs/plans/oauth2-plan.md 的第一阶段实施"
```

## 直接创建自定义 agent

```bash
opencode agent create
```

官方 docs 当前说明，这个交互命令会：

- 让你选择全局还是项目级保存
- 生成 identifier 和 system prompt
- 选择可访问工具
- 最后写出一个 markdown agent 文件

## agent 文件位置

```text
.opencode/agents/
~/.config/opencode/agents/
```

## 在会话里调用 subagent

```text
@explore 先梳理 src/auth 的模块边界，只返回摘要
@general 调研现有 OAuth2 迁移路径并对比两种方案
```

适合：让探索任务在独立上下文里完成，再把结果压缩返回主线程。

更稳的默认法是：

- 主线程负责收口
- `explore` 优先做轻量只读探索
- `general` 承接更重的研究子任务
- `subtask: true` 用来隔离噪音，不是把任务变成 detached worker

## 当前 local CLI/TUI 有 built-in `/agents`

这次本地 CLI/TUI 源码注册点里，已经能确认一条 built-in `/agents`：

- `/agents`：打开 agent list / switch agent dialog
- `<leader>a`：默认 keybind 是 agent list
- `tab` / `shift+tab`：默认 cycle agent

但要分清：

- `/agents` 更接近“看和切当前 agent”
- `@explore`、`@general` 更接近“直接在 prompt 里调用 subagent”
- custom `/review-*`、`/plan-*` commands 更接近“固定一个多步工作流”

另外，本机 `opencode-cli agent list` 已确认至少会列出：

- `build (primary)`
- `plan (primary)`
- `explore (subagent)`
- `general (subagent)`

以及一些内部系统 agents：

- `compaction`
- `summary`
- `title`

## 当前没有第一方 worktree 或 detached worker 叙事

这次 reviewed docs 范围里，没有看到：

- 类似 `--worktree` 的本地隔离工作区
- 类似 Background Agent 的独立 branch worker
- 类似 agent teams 的多 session 编排

所以 OpenCode 在这章里更稳的重点是：

- subtask / subagent 的上下文隔离
- 而不是 filesystem 隔离或后台 worker 隔离

## 用 custom command 固定一个 agent 工作流

例如 `.opencode/commands/review-diff.md`：

```md
---
description: Review recent changes
agent: plan
subtask: true
---
Review the current diff and report correctness, risk, and missing tests.

Recent git status:
!`git status --short`
```

这类命令适合把“探索、审查、总结”这类重复动作固定下来。

官方 commands docs 当前还明确说明：

- 如果 command 不指定 `agent`，默认继承你当前 agent
- 如果 command 绑定的是 subagent，默认就会触发 subagent invocation
- 如果你想强制某个 command 以 subtask 方式运行，即使它绑定的是 primary agent，也可以设 `subtask: true`

这三点会直接影响“它到底是在主线程里做，还是在独立上下文里做”。

## 限制某个 agent 只能调哪些 subagents

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "orchestrator": {
      "mode": "primary",
      "permission": {
        "task": {
          "*": "deny",
          "explore": "allow",
          "security-*": "ask"
        }
      }
    }
  }
}
```

官方 docs 当前说明：

- `permission.task` 用来控制 agent 通过 Task tool 调起哪些 subagents
- 仍然需要记住：用户始终可以通过 `@` 直接调用 subagent

## 这章最容易写错的地方

- 不要让多个 agent 同时改同一块代码。
- 不要在没有职责边界时就拆多 agent。
- 不要忘了 `explore` 更适合轻量只读探索，`general` 更适合重研究子任务。
- 不要把 absence of worktree / background worker 硬补成存在的能力。
- 不要把 subtask 当成后台常驻 worker 的替代品。
