# 05 输入输出与信息外化

## 从 stdin 读完整提示词

```bash
echo "分析这段日志" | codex exec -
```

适合：上游脚本已经生成了完整指令。

## 提示词 + stdin 追加块

```bash
cat error.log | codex exec "分析根因并给出修复建议"
```

当前 CLI 行为：

- 如果只给 `-`，stdin 就是完整提示词
- 如果同时给提示词和 stdin，stdin 会作为 `<stdin>` 块追加

## 把最后一条消息写入文件

```bash
codex exec --ephemeral -s read-only \
  "分析 src/auth/ 的接口边界" \
  -o /tmp/auth-interfaces.md
```

## 把上一步结果喂给下一步

```bash
cat /tmp/auth-interfaces.md | \
  codex exec -s workspace-write \
  "基于这些接口说明实现 OAuth2 中间件"
```

## 建议用它来外化什么

- 分析摘要
- 计划文件
- handoff 说明
- 结构化检查清单

## 交互式对应

```text
/mentions
/diff
```

映射关系：

- `/mentions` ↔ Approximate mapping
  适合在交互态里高效引用路径、文件和实体
- `/diff` ↔ Approximate mapping
  适合查看当前改动，不等同于把中间结果外化成文件

这章里，交互式命令主要提升“会话内引用和查看”的效率，但真正的 handoff 外化仍然更适合落文件。

## 这章最容易写错的地方

- 不要把大量日志手工复制粘贴进长会话。
- 不要让关键中间结果只留在终端输出里。
- 不要把“继续上一轮会话”当成“已经外化了信息”。
- 不要把 `/diff` 当成 handoff 文件的替代品。
