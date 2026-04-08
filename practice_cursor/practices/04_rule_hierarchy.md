# 04 规则分层：轻全局，精按需

## 核心习惯

Cursor 的规则体系目标不是“写得越多越好”，而是让真正相关的规则在真正需要的时候进入上下文。

## 在 Cursor 里怎么做

- 把慢变、跨任务的规则放进 `AGENTS.md` 或 always-apply 的 `.cursor/rules/*.mdc`。
- 把文件类型或目录相关的规则放进带 `globs` 的 `.cursor/rules/*.mdc`。
- 旧仓库如果还在用 `.cursorrules`，明确把它当 legacy，而不是主体系。
- Project Memories 负责持续积累的项目经验，不负责替代规则文件。

实用理解：

- `AGENTS.md` 适合简单全局共识
- `.cursor/rules/*.mdc` 适合当前主规则体系
- `alwaysApply` 负责最少量、最稳定的全局规则
- `globs` 负责路径敏感的专项规则

## 该放进规则层的内容

- 稳定的测试和验证要求
- 构建命令、包管理器约束
- 长期存在的代码风格和协作规则
- 明确不能碰的目录或文件

## 不该放进规则层的内容

- 这次任务的实施计划
- 临时调试过程
- 本周这个 feature 的拆分步骤
- 大段一次性背景材料

## 不要这样做

- 不要把 `.cursorrules` 写成 Cursor 的首选规则文件。
- 不要把所有规则都塞成 `alwaysApply: true`。
- 不要让专项规则在所有任务里都白白消耗注意力。

## 验收标准

- 全局规则短、稳、跨任务有效。
- 路径相关规则靠 `globs` 解决。
- 规则层没有被当前任务状态污染。

## 对应支撑

见 `../commands/04_rules_and_context_commands.md`。
