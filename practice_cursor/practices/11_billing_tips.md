# 11 成本与额度：先分清哪种消耗在烧钱

## 核心习惯

Cursor 的成本控制，不该只靠“少用一点”，而要靠模式选择、模型选择、Max Mode 和 Background Agents 的克制使用。

## 在 Cursor 里怎么做

- 方向不确定时，先用 `Ask`。
- 多文件执行前，先产计划再进 `Agent`。
- 预算敏感时，不要默认开 Max Mode。
- `Background Agents` 先少量使用，再看 usage。
- 经常看 usage dashboard，而不是等额度烧完才发现。

最实用的成本控制顺序：

1. Ask 只读分析
2. 计划落盘
3. 审查方向
4. 再进 Agent 或 Background Agents

## 不要这样做

- 不要把 Background Agents 当成“反正后台跑着，不算贵”。
- 不要把 Max Mode 当默认值。
- 不要在方向还没看清时就直接开执行模式长跑。

## 验收标准

- 你知道当前任务是在 Ask、Agent 还是 Background Agents 上跑。
- 你知道当前模型和 Max Mode 是否真的有必要。
- 你会定期看 usage，而不是只看主观感觉。

## 对应支撑

见 `../commands/11_cost_and_usage_commands.md`。
