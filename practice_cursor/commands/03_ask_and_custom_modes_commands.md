# 03 Ask、Agent、Manual 与 Custom Modes

## Ask：只读探索

Cursor 官方更稳的规划 grounding 是 `Ask`。

适合：

- 学习代码库
- 先做分析
- 先问问题
- 先产计划

不适合：

- 直接让它大规模改文件

## Agent：执行模式

当你已经看清方向，需要搜索、读写、跑命令时，再进 `Agent`。

## Manual：窄范围编辑

`Manual` 更适合：

- 明确只改你选中的文件
- 不想让它自己大范围搜索
- 不想让它自己跑命令

## Custom Modes：把流程固定下来

Cursor 的官方叙事不是“内建固定 Plan Mode”，而是支持 `Custom Modes`。

补充边界：

- 当前官方文档把它写成 beta 能力
- 需要先在设置里启用

适合：

- Plan
- Debug
- Refactor
- Review

你可以在自定义 mode 里固定：

- 提示词
- 工具范围
- 工作方式

## 和 custom commands 的配合

如果某种“先想清楚再动手”的流程经常重复出现，除了 `Custom Modes`，还可以再配 project/global commands。

官方 `Commands` 页的示例里，`setup-new-feature` 就很适合这章：

- 用 command 把规划型提示词模板固定下来
- 用 `Ask` 或相应 mode 承接只读分析
- 计划审完后，再切 `Agent` 或 `Manual`

更稳的分工是：

- mode：决定工具边界和执行方式
- command：决定这次会话怎么起手

## 计划文件建议位置

```text
docs/plans/
```

对 Cursor 来说，这个位置比虚构一个工具专属计划目录更稳，因为 Ask 本身不是“自动落盘命令”。

## 这章最容易写错的地方

- 不要把 Cursor 写成“内建标准 Plan Mode”。
- 不要跳过 Ask，直接让 Agent 一边探索一边大改。
- 不要把 custom command 误写成 mode 的替代品。
- 不要让计划只停留在 tab 历史里。
