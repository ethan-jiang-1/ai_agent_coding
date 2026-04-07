# Aider 官方核验摘要

最后核验日期：2026-04-07
工具版本：`aider 0.86.3.dev+less`
核验方式：Aider 官方文档 + 本机 `aider --help`

## 官方来源

- Usage：`https://aider.chat/docs/usage.html`
- In-chat commands：`https://aider.chat/docs/usage/commands.html`
- Git integration：`https://aider.chat/docs/git.html`
- FAQ：`https://aider.chat/docs/faq.html`
- Repository map 相关文章：`https://aider.chat/2023/10/22/repomap.html`
- 本机帮助：`aider --help`

## 已确认事实

### 1. Aider 的准确术语是 repository map / repo map

- 官方 FAQ 和帮助都明确使用：
  - `repository map`
  - `repo map`
  - `--show-repo-map`
  - `--map-tokens`
- 不应把它随意改写成“后台构建 AST 图谱”。

### 2. `--read` 是正式只读上下文入口

- 本机 `aider --help` 明确有：
  - `--read FILE`
- 官方 FAQ 也明确提到：
  - 用 `/read` 加只读文件
- 这比“假装 Aider 有原生 Plan Mode”更准确。

### 3. `/ask`、`/architect`、`/clear`、`/undo` 都是正式内置命令

- 官方 in-chat commands 当前明确列出：
  - `/add`
  - `/architect`
  - `/ask`
  - `/clear`
  - `/reset`
  - `/undo`
- `/undo` 的官方描述是：
  - 撤销 aider 做的上一次 git commit

### 4. Aider 默认强绑定 git

- 官方 git integration 文档明确：
  - aider 编辑文件时会提交 git commit
  - `/undo` 基于 git 回退
- 本机帮助也明确存在：
  - `--auto-commits`
  - `--dirty-commits`
  - `--no-git`

### 5. architect mode 是正式模式

- 本机帮助有：
  - `--architect`
  - `--auto-accept-architect`
- 官方命令文档也有：
  - `/architect`

### 6. prompt caching 是正式能力

- 本机帮助明确有：
  - `--cache-prompts`
  - `--cache-keepalive-pings`

## 对 round1 的直接修正建议

### 可以安全写成事实的

- Aider 有 repository map / repo map 机制。
- Aider 有 `--read`、`/ask`、`/architect`、`/clear`、`/undo`。
- Aider 的编辑与撤销高度依赖 git。

### 不应继续写成事实的

- “Aider 会在后台构建整个代码库的 AST 图谱”
- “Aider 没有只读探索方式”
- “/undo 只是撤销上一次文本输出”

## 建议在正文里采用的表述

- 若讲大仓探索：
  - “Aider 更准确的底层机制是 repo map，而不是可直接等同为 AST 图谱”
- 若讲规划阶段：
  - “Aider 可用 `/ask` + `--read` / `/read` 组合出只读规划工作流”
- 若讲撤销：
  - “Aider 的 `/undo` 依赖它自己的 git 提交边界”

