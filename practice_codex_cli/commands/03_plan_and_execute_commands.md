# 03 规划与执行命令

## 只读规划并落盘

```bash
codex exec --ephemeral -s read-only \
  "分析 src/auth/ 的现有实现，给出 OAuth2 迁移计划，只输出计划" \
  -o .codex/plans/oauth2-plan.md
```

适合：先把改动范围和顺序讲清楚，再决定是否进入执行阶段。

## 基于计划执行

```bash
cat .codex/plans/oauth2-plan.md | \
  codex exec -s workspace-write \
  "按这份计划实现第一阶段，不要超出计划范围"
```

## 用同一条非交互链继续推进

```bash
codex exec -s read-only "分析 src/auth/ 并给出计划"
codex exec -s workspace-write resume --last "按刚才的计划实现中间件"
codex exec -s read-only resume --last "运行测试并总结风险"
```

## 推荐的计划文件位置

```text
.codex/plans/
```

计划文件至少应包含：

- 受影响文件
- 每个文件的变更方向
- 明确不改的范围
- 执行顺序
- 风险点和验证方式

## 这章最容易写错的地方

- 不要在规划阶段给工作区写权限。
- 不要只把计划留在会话历史里。
- 不要把 `-s` 写在 `resume` 后面。
