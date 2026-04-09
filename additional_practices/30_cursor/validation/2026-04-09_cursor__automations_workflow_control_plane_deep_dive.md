# 2026-04-09 cursor__automations_workflow_control_plane deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `cursor__automations_workflow_control_plane`

## 为什么深挖这个点

这个候选的能力事实层非常强，但和 `long_horizon_cross_surface_layering` 的边界容易重叠。

- Automations 已经是正式 changelog 能力
- 但如果不拆开，很容易把它写成：
  - “就是 Background Agents 换个触发入口”
  - 或 “Cursor 以后应该默认 always-on”

这两种都太粗。

## 深挖后的判断

当前更准确的写法是：

1. Automations 不是普通 Background Agent 的同义词
   - 它引入的是 schedule / event triggers / cross-run memory / template 化 workflow
2. 但它更像 Cursor 的 durable automation 放大器
   - 最稳妥的主落点仍是 `long_horizon_cross_surface_layering`
   - 只有在后续跨工具比较里仍明显独特，才值得保留为独立 agent-specific 候选
3. 因此当前判断应保持：
   - `partially_supported`
   - 先作为 Cursor-specific amplification
   - 不急于单独 promote

## 这次深挖没有改变什么

- 没有把 automations 降成 `reject`
- 没有把它写成“以后所有团队都该上 always-on agents”

## 这次深挖改变了什么

- 把它从一个宽泛的“Cursor 现在能自动化”收紧成：
  - durable automation 子层
  - 倾向并回 `long_horizon`
  - 但保留作为 Cursor-specific 增强信号
