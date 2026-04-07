# 习惯 7：简单任务用 Haiku，复杂任务才用 Sonnet、Opus

## 原文定义

简单任务优先用 Haiku，只有复杂任务才升级到 Sonnet 或 Opus。

## 为什么原文这么强调

- 不同模型的成本档位不同
- 轻任务用高价模型，收益和成本不匹配
- 选对模型，是每天都会发生的成本决策

## 原文给出的任务分层

- Haiku：语法检查、头脑风暴、格式调整、快速翻译、简短回答
- Sonnet：日常核心工作
- Opus：深度推理

## 顺着原文补充的理解

这条习惯优化的是“模型能力和任务强度的匹配”。

## Claude Code 对应动作

- 用 `/model` 切换模型
- 用 `/status` 检查当前模型

## 对照原文

- [原始文章](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/Datawhale_不再触发Claude使用限制，大幅降低Token的10个有效习惯.md#L121-L138)
- [03_模型与价格.md](file:///Users/bowhead/ai_agent_coding/claude_code/raw_saving_cost/reference/03_模型与价格.md)
