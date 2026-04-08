# Codex CLI Slash Commands Reference

来源：

- OpenAI 官方 Codex CLI 文档：`Slash commands in Codex CLI`
- 本机 `codex --help`
- 本机 `codex review --help`

核验日期：2026-04-09

---

## 1. 官方术语

OpenAI 官方对这类交互式 `/...` 功能的叫法是：

- `slash commands`
- `built-in slash commands`

使用场景是：

- `interactive sessions`

这意味着在 Codex CLI 里，slash commands 是交互式会话命令面的一部分，不等同于 shell 外部子命令。

---

## 2. 官方列出的 built-in slash commands

官方文档当前列出的内置 slash commands 有：

- `/add-dir`
- `/agents`
- `/approval-mode`
- `/bug`
- `/clear`
- `/compact`
- `/config`
- `/cost`
- `/diff`
- `/doctor`
- `/help`
- `/init`
- `/mcp`
- `/memory`
- `/mentions`
- `/model`
- `/new`
- `/review`
- `/status`
- `/terminal-setup`

官方还说明：

- `/approvals` 是 `/approval-mode` 的隐藏别名

---

## 3. 与外部命令面的关系

这些 slash commands 和外部命令面的关系并不统一，至少分四类：

### Exact mapping

有些 slash commands 与外部命令或配置动作高度对应，例如：

- `/model` ↔ `-m` / `--model`
- `/review` ↔ `codex review`
- `/mcp` ↔ `codex mcp`
- `/agents` ↔ agent/subagent 配置与使用

### Combined mapping

有些 slash commands 需要和其他会话动作一起理解，才能对应外部命令工作流，例如：

- `/approval-mode` ↔ `-a` / `--ask-for-approval`
- `/add-dir` ↔ `--add-dir`
- `/new` ↔ 新会话 / 重开任务边界
- `/status` ↔ 查看当前模型、沙箱、审批等会话状态

### Approximate mapping

有些 slash commands 解决的是相近问题，但没有严格的外部一一对应，例如：

- `/clear`
- `/compact`
- `/cost`
- `/diff`
- `/memory`

### No direct external mapping

还有一些 slash commands 更像交互式工作流增强，本机帮助输出里没有直接等价的外部命令，例如：

- `/help`
- `/bug`
- `/doctor`
- `/mentions`
- `/terminal-setup`

这些是后续判断是否需要最后补充兜底章的重要来源。

---

## 4. 当前本机帮助能确认的外部命令面

本机帮助当前明确列出这些外部命令或能力：

- `codex exec`
- `codex review`
- `codex resume`
- `codex fork`
- `codex mcp`
- `-m` / `--model`
- `-s` / `--sandbox`
- `-a` / `--ask-for-approval`
- `--search`
- `--add-dir`

这意味着：

- 并不是所有 slash commands 都有外部命令对应
- 交互式命令面明显比外部命令面更丰富

---

## 5. 对当前 Codex CLI 整理工作的直接影响

后续在 `practice_codex_cli/commands/` 里，不能只写：

- `codex exec`
- `codex resume`
- `codex fork`

还需要把对应的 slash commands 补进去，并明确标注：

- Exact mapping
- Combined mapping
- Approximate mapping
- No mapping

后续在 `practice_codex_cli/practices/` 里，也要把“交互式会话里怎么做”补进去。

---

## 6. 最后补充章的候选来源

如果后续真的需要新增一个最后补充兜底章，它最可能承接这些内容：

- `/bug`
- `/doctor`
- `/mentions`
- `/terminal-setup`
- 其他很重要但很难自然并入现有章节的 interactive-only 命令

是否真的新增，要以命令融合之后的剩余情况为准，不预先强行新开。

---

## 7. 当前仍需保持谨慎的点

- 官方页面列出了 slash commands，但不同命令的详细行为边界并不全都能从本机 `--help` 直接确认
- 某些 slash commands 在交互式 TUI 里的副作用、输出形式、是否会修改配置，还需要以后按需进一步核验
- 所以这份 reference 适合作为 command-surface mapping 的基准，不适合把每个 slash command 都写成百分之百完整的行为规范
