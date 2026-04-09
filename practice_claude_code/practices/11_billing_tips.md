# 11 成本与额度：先把贵的环节关起来

## 核心习惯

Claude Code 的成本控制，不该只靠“少用一点”，而要靠工具参数先把贵的环节关起来。

## 在 Claude Code 里怎么做

- 批处理或 CI 任务，用 `--print`。
- 给批处理加 `--max-budget-usd`。
- 在不确定方向时，先用 `--permission-mode plan --print` 生成计划。
- 模型按任务难度切，不要默认全上最强。
- 交互中持续观察成本时，用 `/cost`、`/usage`、`/stats`；但预算护栏仍要靠启动参数。
- 如果用了 agent teams、scheduled tasks 或 GitHub Actions，要把“并行调用次数”和“重复调用次数”一起算进成本。

最实用的成本控制顺序：

1. 只读分析
2. 计划落盘
3. 审查方向
4. 再进入昂贵执行

## 不要这样做

- 不要把 `--max-budget-usd` 用在交互式模式里，指望它起作用。
- 不要在方向还没看清时就直接高成本执行。
- 不要把所有自动化脚本都跑在最贵模型上。
- 不要把 agent teams 当成“免费加速”；它通常更贵。

## 验收标准

- 你知道当前命令是不是 `--print`。
- 批处理路径上有预算护栏。
- 当前模型选择和任务价值匹配。
- 自动化路径有明确的 turns / timeout / recurrence 护栏。

## 对应支撑

见 `../commands/11_cost_and_batch_commands.md`。
