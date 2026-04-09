# 2026-04-09 opencode__skills_as_lazy_context_layer deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `opencode__skills_as_lazy_context_layer`

## 为什么深挖这个点

这个候选的能力事实层很强，但 adoption 层和边界都容易写糙：

- 官方已经给了 native skill tool
- 也兼容 `.claude/skills`、`.agents/skills`
- 但如果不深挖，很容易被写成：
  - “以后都别写 AGENTS.md 了”
  - 或 “skills 只是另一个 commands 目录”

这两种都不对。

## 深挖后的判断

当前更准确的分工是：

1. `AGENTS.md` / `instructions`
   - 稳定合同
   - 全局或项目长期约束
2. custom commands
   - 会话动作入口
   - 固定 workflow trigger
3. skills
   - 重流程、按需加载、可跨项目复用的知识块

所以真正应该避免的是：

- 把所有背景知识永久塞进静态规则文件
- 把本该按需加载的流程知识，硬写成每次都进上下文的长说明

## 这次深挖没有改变什么

- 没有把 `AGENTS.md` 或 `instructions` 降成次要层
- 没有把 skills 升格成当前已被大规模验证的 canonical promote 候选

## 这次深挖改变了什么

- 把这个候选从模糊的“OpenCode 也有 skills”收紧成：
  - `partial support`
  - 更像 `04` 章的 merge 强化
  - 可与 Codex / Claude 的 skills layering 做跨工具收敛
