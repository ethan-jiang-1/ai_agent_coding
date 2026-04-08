# 03 规划与执行命令

## 只读规划

```bash
claude --permission-mode plan \
  "分析 src/auth/ 的现有实现，给出 OAuth2 迁移计划"
```

## 只读规划并落盘

```bash
claude --permission-mode plan \
       --print \
       "分析 src/auth/ 的现有实现，给出 OAuth2 迁移计划" \
  > .claude/plans/oauth2-plan.md
```

## 审查计划后执行

```bash
claude --permission-mode default \
  "读取 .claude/plans/oauth2-plan.md，按计划实现第一阶段"
```

## 推荐的计划文件位置

```text
.claude/plans/
```

计划至少要写清：

- 受影响文件
- 每个文件的变更方向
- 明确不改范围
- 执行顺序
- 风险和验证方式

## 这章最容易写错的地方

- 不要在默认可编辑模式下“顺便先规划”。
- 不要让计划只留在会话历史里。
- 不要跳过人工审查就直接执行多文件改动。
