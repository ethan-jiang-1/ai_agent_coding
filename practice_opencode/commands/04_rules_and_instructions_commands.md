# 04 规则层与 instructions

## 项目级规则

```text
<project-root>/AGENTS.md
```

这是 OpenCode 当前首选的项目规则载体。

## 全局规则

```text
~/.config/opencode/AGENTS.md
```

适合放个人长期偏好，不要混进项目仓库。

## 兼容 fallback

OpenCode 当前官方说明：

- 如果没有项目级 `AGENTS.md`，会回退到 `CLAUDE.md`
- 如果没有全局 `~/.config/opencode/AGENTS.md`，会回退到 `~/.claude/CLAUDE.md`

所以如果项目里历史上遗留了 `CLAUDE.md`，要清楚它是否还在被加载。

## 规则优先级

官方 docs 当前可确认：

1. 从当前目录向上遍历本地 `AGENTS.md` / `CLAUDE.md`
2. `~/.config/opencode/AGENTS.md`
3. `~/.claude/CLAUDE.md`

并且：

- 同一类别里 first matching file wins
- 如果同层同时有 `AGENTS.md` 和 `CLAUDE.md`，只用 `AGENTS.md`

## 初始化规则入口

```text
/init
```

适合：让 OpenCode 帮你生成或改进 `AGENTS.md` 草稿。

## 用 `instructions` 补充额外规则

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "docs/engineering-guidelines.md",
    "docs/testing/*.md"
  ]
}
```

官方 docs 当前说明 `instructions` 支持：

- 文件路径
- glob
- 远程 URL

## 这章最容易写错的地方

- 不要把任务计划写进 `AGENTS.md`。
- 不要忘了 `CLAUDE.md` fallback 的存在。
- 不要把所有规则全文堆进根规则，而不利用 `instructions` 分层。
