# 08 permission 与工具边界

## 最简单的收权限方式

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "ask",
    "bash": "ask"
  }
}
```

OpenCode 当前官方 docs 明确写到：

- 默认大多数权限是 `allow`
- `doom_loop` 和 `external_directory` 默认是 `ask`

## 当前没有内建 `/permissions`

至少在这次官方 TUI 命令清单里，没有看到类似 Claude Code / Codex CLI 那样的内建 `/permissions`。

这意味着：

- OpenCode 的权限收紧主路径是 config
- 或者 agent config / agent markdown
- 而不是会话里临时打一条 built-in slash command

## 整体默认 + 工具覆盖

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "*": "ask",
    "bash": "allow",
    "edit": "deny"
  }
}
```

## 对 bash 做 pattern 级控制

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "bash": {
      "*": "ask",
      "git *": "allow",
      "npm test *": "allow",
      "rm *": "deny"
    }
  }
}
```

官方 docs 当前说明：

- 规则按 pattern match 计算
- 最后一个匹配项获胜

## 对路径做更细粒度 edit 控制

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": {
      "*": "deny",
      "docs/**/*.md": "allow"
    }
  }
}
```

## agent 级权限覆盖

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "bash": {
      "*": "ask",
      "git *": "allow",
      "git push *": "deny"
    }
  },
  "agent": {
    "build": {
      "permission": {
        "bash": {
          "*": "ask",
          "git *": "allow",
          "git commit *": "ask",
          "git push *": "deny"
        }
      }
    }
  }
}
```

官方 docs 当前说明：

- agent permissions 会和全局 config 合并
- agent rules 优先级更高

## 只读 reviewer agent 示例

```md
---
description: Code review without edits
mode: subagent
permission:
  edit: deny
  bash: ask
  webfetch: deny
---
Only analyze code and suggest changes.
```

文件位置：

```text
.opencode/agents/review.md
~/.config/opencode/agents/review.md
```

## 这章最容易写错的地方

- 不要只靠提示词说“别改代码”。
- 不要忘了规则顺序会改变结果。
- 不要忽略 `external_directory`、`.env` 默认保护和 agent 级覆盖。
