# 2026-04-09 codex_cli__skills_over_agents_bloat deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `codex_cli__skills_over_agents_bloat`

## 为什么深挖这个点

这个候选的能力事实层很强，但 adoption 层明显弱于其他候选，而且研究结论存在方向性张力：

- 一篇研究显示 `AGENTS.md` 能提效
- 另一篇研究显示 context files 会诱导更广探索，但未必提升成功率

如果不把这个点拆开看，很容易误判成：

- “以后别写 AGENTS.md 了”
- 或 “以后全都写到 skills”

这两种结论都太粗。

## 深挖后的判断

当前更准确的说法是：

1. `AGENTS.md` 仍然有价值
   - 尤其适合作为稳定、短小、操作性的项目合同
2. skills 适合承接重流程、重操作、按需加载的知识
3. 真正应该避免的是：
   - 把所有上下文都塞进一个越来越长的 AGENTS 文件
   - 把技能、项目规则、一次性任务状态混在一起

## 这次深挖没有改变什么

- 没有把它升级成 `supported + Promote`
- 没有把它降成 `Reject`

## 这次深挖改变了什么

- 把它从一个模糊的“skills 很重要”收紧成：
  - `partial support`
  - 更像 `04` / `06` 的 merge 强化
  - 需要跨工具再比较之后再决定是否保留为 canonical 候选
