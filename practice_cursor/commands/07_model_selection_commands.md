# 07 模型选择与 Max Mode

## 日常默认

如果任务不复杂，优先用 Cursor 的日常默认模型或 Auto。

适合：

- 常规实现
- 小范围修 bug
- 一两个文件的修改

## 更强模型何时上

适合：

- 跨模块分析
- 大重构
- 架构判断
- 高风险逻辑修改

## 把模型和 mode 一起看

更稳的理解是：

- `Ask` + 强模型：高质量探索
- `Agent` + 日常模型：常规执行
- `Custom Modes`：把固定任务绑定到固定模型

## 这章为什么没有补很多 slash

这次重新深挖官方资料后，editor 侧没有确认到 `/model` 这类 built-in quick command。

所以对 Cursor editor 来说，这章的主要入口仍然是：

- model picker
- mode 选择
- `Custom Modes`

如果你印象里有 `/model`，更可能是 Cursor CLI 的 slash commands，而不是 editor chat 的 commands。

## Max Mode

适合：

- 仓库很大
- 文档很多
- 真的需要更大上下文

不适合：

- 只是简单改一个函数
- 你懒得开新 tab
- 方向还没确定

## 这章最容易写错的地方

- 不要把所有任务都默认升到最贵模型。
- 不要把 Max Mode 当成常驻开关。
- 不要把 CLI 的 `/model` 混写成 editor 的官方 quick command。
- 不要把 Cursor 写成有 Aider 那样内建三模型自动路由。
