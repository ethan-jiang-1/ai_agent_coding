# 04 规则层与上下文文件

## 项目级规则

```text
<project-root>/CLAUDE.md
```

适合放：

- 长期稳定的项目共识
- 构建与测试要求
- 不该碰的目录或约束

## 路径级规则

```text
.claude/rules/*.md
```

这些文件通常用 YAML frontmatter 做路径匹配。

示例：

```yaml
---
paths:
  - "src/api/**/*.ts"
---
## API 规则
...
```

## 本地个人覆盖

```text
CLAUDE.local.md
```

适合放只对你本机有效、且不应提交到仓库的个人偏好。

## 用户级规则

```text
~/.claude/CLAUDE.md
```

## 最小化模式

```bash
claude --bare
```

`--bare` 会跳过：

- auto-memory
- `CLAUDE.md` 自动发现
- 一些自动背景行为

适合：你要最小上下文、最少自动附加信息时。

## 这章最容易写错的地方

- 不要把任务计划写进 `CLAUDE.md`。
- 不要把个人偏好混进团队规则。
- 不要忘了 `--bare` 会跳过 `CLAUDE.md` 自动发现。
