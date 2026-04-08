# 03 规划与执行命令

## 规划阶段：直接切到 `plan` agent

```bash
opencode --agent plan

opencode run --agent plan \
  "分析 src/auth/ 的现有实现，给出 OAuth2 迁移计划"
```

`plan` 是 OpenCode 内建 primary agent，官方说明它默认对 file edits 和 bash 都走 `ask`，适合分析和规划阶段。

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

这样在 TUI 里可以直接用对应的 `/...` custom command 触发。

## 这章最容易写错的地方

- 不要让 `build` agent “顺便先规划”。
- 不要让计划只留在会话历史里。
- 不要跳过人工审查就直接执行多文件改动。
