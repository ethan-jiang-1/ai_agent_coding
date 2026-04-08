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
