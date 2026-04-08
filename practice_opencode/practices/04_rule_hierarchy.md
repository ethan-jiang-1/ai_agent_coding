# 04 规则分层：AGENTS 稳定，instructions 按需补

## 核心习惯

OpenCode 的规则层目标不是“写越多越好”，而是让真正稳定、长期有效的规则进入 `AGENTS.md`，把可复用但不该塞进根规则的材料放进 `instructions`。

## 在 OpenCode 里怎么做

- 项目级长期规则放进 `AGENTS.md`。
- 个人长期偏好放进 `~/.config/opencode/AGENTS.md`。
- 额外规范、共享文档、glob 规则，用 `instructions` 补。
- 需要初始化或整理规则入口时，用 `/init`。

实用理解：

- `AGENTS.md` 负责慢变、跨任务的项目共识
- `instructions` 负责把其他规则文件按路径、glob 或远程来源并进来
- `CLAUDE.md` 只是 fallback，不是 OpenCode 首选载体

## 该放进规则层的内容

- 构建、lint、test 的稳定命令
- 长期存在的目录禁区和验证要求
- 团队长期采用的代码风格和协作约束
- 只有项目内部人才知道的 repo 结构与坑点

## 不该放进规则层的内容

- 这次任务的计划
- 一次性的 debug 过程
- 某个 feature 本周的拆分步骤
- 会很快过期的上下文

## 不要这样做

- 不要把任务计划写进 `AGENTS.md`。
- 不要把所有外部规则全文复制进根规则。
- 不要忘了 OpenCode 有 `CLAUDE.md` fallback，从而误判当前到底加载了哪层规则。

## 验收标准

- 根规则短、稳、跨任务有效。
- 外部规范通过 `instructions` 按需并入。
- 临时任务状态已经落在计划文件或 handoff 文件里，而不是污染规则层。

## 对应支撑

见 `../commands/04_rules_and_instructions_commands.md`。
