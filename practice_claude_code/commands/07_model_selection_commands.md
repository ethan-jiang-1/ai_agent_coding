# 07 模型选择命令

## 显式切模型

```bash
claude --model opus "做跨模块架构分析"
claude --model sonnet "做常规功能实现"
claude --model haiku "做简单格式化和轻量整理"
```

也支持完整模型名，例如：

```bash
claude --model claude-opus-4-6 "..."
claude --model claude-sonnet-4-6 "..."
```

## 非交互式回退模型

```bash
claude --print \
       --model sonnet \
       --fallback-model haiku \
       "总结这批日志"
```

`--fallback-model` 当前只对 `--print` 有意义。

## 当前 Claude Code 的模型路由现实

- 主模型切换靠 `--model`
- 没有 Aider 那样内建的 `editor-model` / `weak-model`
- 更细的分工通常要靠 subagents 或不同批次任务

## 这章最容易写错的地方

- 不要把所有任务都默认升到 `opus`。
- 不要把所有轻任务都丢给 `haiku`，不看风险。
- 不要把 `--fallback-model` 当成通用交互式路由机制。
