# 03 规划与执行命令

## 规划阶段：直接切到 `plan` agent

```bash
opencode --agent plan

opencode run --agent plan \
  "分析 src/auth/ 的现有实现，给出 OAuth2 迁移计划"
```

`plan` 是 OpenCode 内建 primary agent，官方说明它默认对 file edits 和 bash 都走 `ask`，适合分析和规划阶段。

## 一个重要边界：OpenCode 当前没有内建 `/plan`

和 Claude Code 不同，OpenCode 这次官方 TUI 命令清单里没有一个内建 `/plan`。

所以在交互状态下，更稳的做法是：

- 用 `tab` 在 primary agents 间切到 `plan`
- 或者定义一个 custom `/plan-*` command，把它绑定到 `agent: plan`

## 把计划落盘

```bash
opencode run --agent plan \
  "分析 src/auth/ 的现有实现，给出 OAuth2 迁移计划" \
  > docs/plans/oauth2-plan.md
```

建议先审查这份文件，再进入执行阶段。

## 执行阶段：切回 `build`

```bash
opencode run --agent build \
  "读取 docs/plans/oauth2-plan.md，只实现第一阶段，不扩范围"
```

## 在 TUI 里切 primary agent

官方 TUI 文档当前写明：

- `tab` 可以 cycle through primary agents

也就是说，在长会话里你可以先切到 `plan`，再回 `build`，但多文件任务仍然建议把计划落盘。

## 会话内对应 custom slash commands

OpenCode 的一个真实优势是：

- 你可以把高频规划动作做成自己的 `/plan-auth`、`/review-migration` 一类 custom commands
- 这样“交互里怎么做”就不只是自然语言提示，而是正式的 slash command
- 但要注意：如果 command 不写 `agent`，它默认继承你当前 agent；所以规划型 command 最好显式写 `agent: plan`

## 推荐的计划文件位置

```text
docs/plans/
.opencode/plans/
```

计划至少要写清：

- 受影响文件
- 每个文件的修改方向
- 明确不改范围
- 执行顺序
- 风险和验证方式

## 可以抽成 custom command

例如把高频规划动作做成 `.opencode/commands/plan-migration.md`：

```md
---
description: Generate a migration plan
agent: plan
---
Analyze @src/auth and produce a staged migration plan.
```

如果你想让这类规划动作即使在主会话里触发，也尽量别污染当前主上下文，可以进一步考虑：

```md
---
description: Analyze auth in isolation
agent: plan
subtask: true
---
Analyze @src/auth and return only a staged plan summary.
```

这样在 TUI 里可以直接用对应的 `/...` custom command 触发。

## 这章最容易写错的地方

- 不要让 `build` agent “顺便先规划”。
- 不要让计划只留在会话历史里。
- 不要跳过人工审查就直接执行多文件改动。
