# 07 模型选择命令

## 显式指定模型

```bash
codex -m <model> "做跨模块架构分析"
codex exec -m <model> "实现这个明确范围的改动"
```

适合：你已经知道这次任务需要更强或更便宜的模型。

## 使用本地 OSS provider

```bash
codex --oss "总结这段日志"
codex --oss --local-provider ollama "重命名这几个变量"
codex exec --oss --local-provider lmstudio "格式化这个文件"
```

前提：本地已经运行 Ollama 或 LM Studio。

## 交互式对应

```text
/model
```

映射关系：

- `/model` ↔ Exact mapping
  是交互态下最直接的模型切换入口

实践上：

- shell 外切模型：`-m` / `--model` 或 `--oss`
- 会话内切模型：优先 `/model`

## 当前 Codex CLI 的模型路由现实

- 主模型切换靠 `-m`
- 本地模型入口是 `--oss`
- 没有 Aider 那种内建 `editor-model` / `weak-model` 分工

如果你要做更细的模型分工，通常有两种办法：

- 用不同批次分别跑不同模型
- 用 subagents 为不同角色配置不同模型

## 这章最容易写错的地方

- 不要把 `--oss` 当成通用替代方案。
- 不要把“单文件任务”自动等同为“轻任务”。
- 不要在长期文档里硬编码某个具体模型列表。
- 不要在交互态里还回到 shell 重新起命令，只为了切模型。
