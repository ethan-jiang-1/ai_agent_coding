# 04 规则层与上下文文件

## 当前主规则体系

```text
.cursor/rules/*.mdc
AGENTS.md
```

其中：

- `.cursor/rules/*.mdc` 是当前主 project rules 体系
- `AGENTS.md` 也是 Cursor 官方支持项
- `.cursorrules` 是 legacy

## `.mdc` 的核心字段

```yaml
---
description: React component rules
globs:
  - "src/components/**/*.tsx"
alwaysApply: false
---
```

最关键的几个字段：

- `description`
- `globs`
- `alwaysApply`

## 规则类型

官方规则体系里，至少应区分：

- Always
- Auto Attached
- Agent Requested
- Manual

## 嵌套规则目录

Cursor 官方文档还明确支持子目录自己的 `.cursor/rules`。

这意味着：

- 根目录 rules 负责更通用的约束
- 子目录 rules 负责局部专项约束

## `alwaysApply` 的正确用途

只放每次任务都相关的少量核心约束。

适合：

- 包管理器
- 测试命令
- 全项目都必须遵守的硬约束

## `globs` 的正确用途

只在匹配路径时附着专项规则。

适合：

- React 组件规范
- API 路由规范
- 测试文件规范

## legacy：`.cursorrules`

如果老仓库里还在用，可以过渡保留，但不要再把它写成首选机制。

## 会话内规则生成入口

Cursor 官方 `Commands` 页确认，聊天里存在内建 quick commands。

对这一章最相关的是：

- `/Generate Cursor Rules`

更稳的理解是：

- 它适合先生成规则草稿
- 真正长期生效的内容，还是要落到 `.cursor/rules/*.mdc` 或 `AGENTS.md`

## 规则落盘动作

如果你已经把规则想清楚了，官方文档还支持通过 command palette 创建规则，例如：

- `New Cursor Rule`

这类动作的实践意义是：

- slash / quick command 适合起草
- 规则文件才是稳定承载层

## 这章最容易写错的地方

- 不要把 `.cursorrules` 当主规则文件。
- 不要把所有规则都做成 `alwaysApply: true`。
- 不要把任务计划和临时调试过程塞进 rules。
- 不要把 `/Generate Cursor Rules` 产出的草稿直接当成无需审阅的最终规则。
