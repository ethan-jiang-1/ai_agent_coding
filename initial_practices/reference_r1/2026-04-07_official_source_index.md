# 官方来源索引

最后核验日期：2026-04-07

## 1. 这份索引的用途

这个目录的目标不是收集“看起来像资料的东西”，而是给 `round1` 提供可回查的 grounding。

优先级原则：

1. 官方正文
2. 本机 CLI 帮助 / 版本输出
3. 官方仓库原始文档 / schema / 示例
4. 官方搜索结果摘要

只要高层级证据和低层级证据冲突，以高层级为准。

## 2. 证据等级定义

- `A`：官方正文可直接读取，或本机 CLI 可直接核验
- `B`：官方仓库原始文档、配置 schema、源码树、页面标题
- `C`：官方搜索结果摘要；可做方向判断，但不适合写死细节 UI

## 3. 工具索引

### 3.1 Codex CLI

主参考文件：

- `2026-04-07_codex_cli_official.md`

官方来源：

- `https://developers.openai.com/codex/` `A`
- `https://developers.openai.com/codex/config-basic` `A`
- `https://developers.openai.com/codex/noninteractive` `A`
- `https://developers.openai.com/codex/agent-approvals-security` `A`
- `https://github.com/openai/codex` `B`
- `docs/config.md` in official repo `B`
- `docs/agents_md.md` in official repo `B`
- `docs/skills.md` in official repo `B`
- `codex-rs/core/config.schema.json` in official repo `B`

本机核验：

- `codex --version`
- `codex --help`
- `codex exec --help`
- `codex resume --help`
- `codex fork --help`
- `codex features list`

### 3.2 Claude Code

主参考文件：

- `2026-04-07_claude_code_official.md`

官方来源：

- `https://docs.anthropic.com/en/docs/claude-code` `A`
- `https://code.claude.com/docs/en/memory` `A`
- `https://code.claude.com/docs/en/permission-modes` `A`
- `https://docs.anthropic.com/en/docs/claude-code/settings` `A`
- `https://docs.anthropic.com/en/docs/claude-code/sub-agents` `A`
- `https://docs.anthropic.com/en/docs/claude-code/common-workflows` `A`

本机核验：

- `claude --version`
- `claude --help`
- `claude agents --help`
- `claude mcp --help`

### 3.3 Cursor

主参考文件：

- `2026-04-07_cursor_official.md`

官方目标页面：

- `https://docs.cursor.com/context/rules-for-ai` `A/B`
- `https://docs.cursor.com/chat/manual` `A/B`
- `https://docs.cursor.com/chat/overview` `A/B`
- `https://docs.cursor.com/en/background-agents` `A/B`
- `https://docs.cursor.com/agent/tools` `A/B`
- `https://docs.cursor.com/agent/checkpoints` `A/B`
- `https://docs.cursor.com/agent/custom-modes` `A/B`

当前限制：

- 正文抓取被官方站点的 Vercel Security Checkpoint 拦截
- 因此本轮主要依赖：
  - 官方搜索结果摘要
  - 官方页面标题
  - 多条官方摘要之间的相互印证

写作策略：

- 可以写产品结构与能力边界
- 不写死某个快捷键或菜单文字

### 3.4 Aider

主参考文件：

- `2026-04-07_aider_official.md`

官方来源：

- `https://aider.chat/docs/usage.html` `A`
- `https://aider.chat/docs/usage/commands.html` `A`
- `https://aider.chat/docs/git.html` `A`
- `https://aider.chat/docs/faq.html` `A`
- `https://aider.chat/2023/10/22/repomap.html` `A`

本机核验：

- `aider --version`
- `aider --help`

## 4. 主题索引

### 4.1 规则文件 / 指令层级

- Codex CLI：看 `AGENTS.md`、层级覆盖、skills
- Claude Code：看 `CLAUDE.md`、`.claude/rules/`、auto memory
- Cursor：看 `.cursor/rules/*.mdc`、`AGENTS.md`、`.cursorrules`
- Aider：不要套用前三者的规则树模型

### 4.2 规划阶段

- Codex CLI：`read-only sandbox + exec/session discipline`
- Claude Code：`plan` permission mode
- Cursor：`Ask + Custom Modes`
- Aider：`--read` + `/ask` + `architect`

### 4.3 多 agent

- Codex CLI：subagents / multi-agent
- Claude Code：subagents
- Cursor：Background Agents
- Aider：不是官方主叙事

### 4.4 回退 / 撤销

- Cursor：checkpoints
- Aider：`/undo` 基于 git commit
- Claude Code：不要把 `/compact` 当撤销
- Codex CLI：本轮不虚构统一内建 undo

## 5. 当前仍需谨慎的地方

### 5.1 Cursor

这是当前唯一仍需显式保留不确定性的部分。原因不是信息不足，而是官方正文在当前环境无法直接抓取。

### 5.2 版本敏感的 UI 文案

四个工具里，以下内容都不应该轻易写死：

- 按钮名
- 快捷键
- 某个 beta 开关默认是否开启
- 某个模式在某个界面里是否默认显示

## 6. 对 `round1` 的总要求

- 先用这里的参考约束术语，再去改正文。
- 如果正文里一句话无法落到这里的某个来源，就先不要把它写成事实。
