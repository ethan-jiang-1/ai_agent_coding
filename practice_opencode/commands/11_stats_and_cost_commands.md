# 11 stats、模型成本与自动化控制

## 看 usage 和 cost

```bash
opencode stats
opencode stats --days 7
opencode stats --models 10
opencode stats --project ""
```

OpenCode 当前官方 docs 明确说明：

- `stats` 可以看 token usage 和 cost statistics

## 看模型元数据

```bash
opencode models --verbose
```

适合：先看模型列表和成本元数据，再决定默认模型。

## 交互状态下的 onboarding 入口

```text
/connect
/models
```

更稳的映射关系是：

- `/connect`：TUI 里加 provider credentials
- `opencode auth login`：CLI 里做同样类别的事
- `/models`：TUI 里看并选模型
- `opencode models`：CLI 里列模型清单

这组命令不是单纯“账号设置”，它直接决定你后续能用哪些模型，以及成本层面的选择空间。

## 成本更低的典型工作流

```bash
opencode run --agent plan \
  "分析 src/auth/ 并给出迁移计划" \
  > docs/plans/oauth2-plan.md

opencode run --agent build \
  "读取 docs/plans/oauth2-plan.md，只实现第一阶段"
```

这比一上来就让 `build` 做开放式多轮改动更可控。

## 用 `small_model` 承担轻量内部任务

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5"
}
```

## 自动化脚本的两个实用点

### 1. 用 `run`

```bash
opencode run --format json \
  "总结 failing tests，只输出 JSON"
```

适合：脚本、CI、可解析输出。

### 2. 用 `serve` + `attach`

```bash
opencode serve

opencode run --attach http://localhost:4096 \
  "继续这台常驻实例里的工作"
```

这更像是减少冷启动和工具重建开销，主要优化的是工作流延迟，不是官方意义上的 token 预算护栏。

## 当前这章最该保守理解的点

在这次官方 docs 范围里，没有看到类似 `--max-budget-usd` 的单次预算上限参数。

更稳的做法是靠：

- `plan` 先收方向
- 模型选择
- `small_model`
- `stats` 回看

## 这章最容易写错的地方

- 不要把别家工具的预算开关想当然搬过来。
- 不要在方向不清时直接高成本执行。
- 不要只靠感觉估成本，不看 `stats`。
