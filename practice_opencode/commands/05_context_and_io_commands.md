# 05 文件引用、命令输出与 handoff 输入

## 在 TUI 里引用文件

```text
@src/auth/login.ts
@docs/spec/auth.md
```

OpenCode 当前官方说明：

- `@` 会做 fuzzy file search
- 选中的文件会自动加进上下文

## 在 TUI 里注入命令输出

```text
!git status --short
!npm test -- auth
```

适合：把当前工作区状态、测试输出、日志摘要补进会话，而不是手贴残缺片段。

## 用 `--file` 给 `run` 附件

```bash
opencode run \
  -f docs/spec/auth.md \
  -f tmp/auth-error.log \
  "结合这些材料，分析当前登录失败的根因"
```

## 长提示词用 `/editor`

```text
/editor
```

适合：需要组织一段较长输入时，先在 `$EDITOR` 里写好，再送入会话。

## 查看工具执行细节

```text
/details
```

适合：

- 你想看清 OpenCode 具体执行了哪些工具动作
- 当前会话里 `!command`、文件读取或工具链行为比较复杂

它没有稳定 external 等价物，所以更适合当作会话内操作来理解。

## 导出当前对话

```text
/export
```

适合：把当前结论导成 Markdown，作为 handoff 或复盘材料。

## custom commands 也能做输入编排

例如 `.opencode/commands/review-auth.md`：

```md
---
description: Review auth changes
agent: plan
subtask: true
---
Review @src/auth and summarize risk.

Recent test output:
!`npm test -- auth`
```

这类命令特别适合把重复输入流程固定下来。

## 这章最容易写错的地方

- 不要第一轮就贴满原始日志和大段文件内容。
- 不要把关键中间结论只留在会话里。
- 不要忽略 `@`、`!`、`--file` 这些比纯粘贴更干净的入口。
