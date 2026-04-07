# 第七章（深化）：模型路由分级——按任务复杂度选择正确的模型

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**Aider 的三模型分工（主模型 / 编辑模型 / 弱模型）是目前工具层面最精细的模型路由机制，值得深入理解和利用。其他工具的路由主要靠手动切换。**

---

## 正确的做法

### 深化：Aider 的三模型分工机制（官方确认）

Aider 提供了三个独立的模型配置（`A/B`：Aider 官方文档）：

| 参数 | 负责什么 | 默认值 |
|------|---------|--------|
| `--model` | 主推理：理解需求、提出方案、architect 模式下的逻辑设计 | 取决于配置 |
| `--editor-model` | 代码编写：在 architect 模式下，把主模型的方案翻译成具体的文件编辑 | 取决于主模型 |
| `--weak-model` | 辅助任务：生成 commit 消息、总结聊天历史（超出软上限时） | 取决于主模型 |

**architect 模式的工作流**（`A/B`）：

```bash
# architect 模式：主模型推理，编辑模型执行
aider --model claude-sonnet-4-6 \
      --editor-model claude-haiku-4-5 \
      src/auth/login.ts

# 工作流：
# 1. 用户提出需求
# 2. --model（Sonnet）理解需求，给出修改方案（自然语言 + 伪代码）
# 3. --editor-model（Haiku）把方案翻译成实际的 diff 并应用
# 4. 用户审查修改
```

**weak-model 的使用场景**（`A/B`）：

```bash
aider --model claude-sonnet-4-6 \
      --weak-model claude-haiku-4-5

# weak-model 何时介入：
# 1. 每次 git commit 时，自动生成 commit 消息（Haiku 生成，不消耗 Sonnet 额度）
# 2. 聊天历史超出软上限时，自动做历史摘要（Haiku 处理，不消耗 Sonnet 额度）
```

**三模型全配置示例**（`A/B + I`）：

```bash
aider \
  --model claude-sonnet-4-6 \      # 主推理
  --editor-model claude-haiku-4-5 \ # 代码编写（architect 模式）
  --weak-model claude-haiku-4-5 \   # commit 消息 + 历史摘要
  --cache-prompts \                  # 缓存稳定前缀
  --read CONVENTIONS.md \
  src/auth/
```

这个配置的成本结构（`I`）：
- 所有"理解"工作由 Sonnet 处理（贵，但值得）
- 所有"写代码"工作由 Haiku 处理（便宜，准确率足够）
- 所有辅助任务由 Haiku 处理（很便宜）

### 深化：Claude Code 的模型切换

Claude Code 的模型切换比较直接（`A/B`：claude --help 核验）：

```bash
# 指定模型（支持别名）
claude --model opus "跨模块架构分析"
claude --model sonnet "常规功能实现"
claude --model haiku "简单格式化任务"

# 支持完整模型名
claude --model claude-opus-4-6 "..."
claude --model claude-sonnet-4-6 "..."
```

**注意**：Claude Code 目前没有类似 Aider 的"自动分层路由"机制——所有工具调用、文件读取、代码生成都使用同一个模型。如果需要模型分层，只能通过 subagents（手动指定不同 subagent 使用不同模型）来实现（`I`）。

### 深化：Codex CLI 的 --oss 和本地模型

`--oss` 是 Codex CLI 官方支持的本地模型入口（`A/B`：codex --help 核验）：

```bash
# 等价于 -c model_provider=oss
codex --oss "总结这段错误日志"

# 指定本地 provider
codex --oss --local-provider ollama "..."
codex --oss --local-provider lmstudio "..."
```

**使用条件**：需要本地运行 LM Studio 或 Ollama（`A/B`：CLI 帮助文本说明"verifies a local LM Studio or Ollama server is running"）。

**适合的场景**（`I`）：
- 日志分析、代码格式化、简单的变量重命名
- 不需要跨文件理解的单点修改
- 对延迟不敏感的批处理任务

**不适合的场景**（`I`）：
- 需要理解大型代码库结构的任务
- 架构设计和复杂的逻辑推理
- 需要高准确率的安全敏感代码修改

---

## 错误的可能

### 深化：editor-model 和主模型不必须是同一家

`--editor-model` 可以来自不同的 provider（`A/B`：Aider 官方）。例如：

```bash
# 主模型用 OpenAI o1（推理强但生成 diff 格式差）
# editor-model 用 Claude Haiku（便宜，擅长生成 diff）
aider --model o1 \
      --editor-model claude-haiku-4-5 \
      src/auth/login.ts
```

这是 `--editor-model` 设计的初衷之一：让强推理模型只负责"想"，让擅长代码格式的模型负责"写"。

### 深化：不要把 weak-model 当成"所有轻任务的路由"

`--weak-model` 只处理 commit 消息和历史摘要（`A/B`）。代码分析、文件读取、代码修改都由主模型处理，不会自动路由到 weak-model。

"用轻量模型处理轻任务"在 Aider 里的实现，是**分任务**而不是**动态路由**：
- 重任务（逻辑分析）→ 用 `architect` 模式，`--model` 负责
- 轻任务（代码改写）→ 用 `architect` 模式，`--editor-model` 负责
- 辅助任务（commit / 摘要）→ `--weak-model` 负责

---

## 背后的原理

### 深化：architect 模式为什么能用更便宜的 editor-model

代码修改任务实际上有两个不同难度的子任务（`I`）：

1. **理解**：这里需要改什么？改成什么样？为什么？（需要推理能力）
2. **执行**：把"改成什么样"翻译成具体的 diff / edit 操作（需要格式化能力，不需要深层推理）

旗舰模型（Sonnet、Opus）的优势在于任务 1，而任务 2 对轻量模型（Haiku）来说也能完成得很好。architect 模式把这两个任务分给不同的模型，避免了为任务 2 付旗舰模型的价格。

这也解释了为什么 architect 模式是一个有实际意义的分层方案，而不只是"用便宜模型省钱"的简单策略。

### 深化：模型路由的边界在哪里

round1 的三级路由框架（重任务/中任务/轻任务）是合理的大方向，但"轻"和"中"的边界取决于具体的模型能力（`I`）：

一个判断框架：
- 任务是否需要跨文件理解 → 通常需要中档以上
- 任务是否需要理解业务逻辑背景 → 通常需要中档以上
- 任务是否可以从输入直接推导输出（格式转换、模式匹配）→ 通常轻量模型够用

用这个框架，而不是记忆"变量重命名=轻任务"这样的例子，能在遇到新场景时做出更准确的判断。

---

## 延伸的习惯

### 深化：在 .aider.conf.yml 里固化模型分工配置

```yaml
# .aider.conf.yml
model: claude-sonnet-4-6
editor-model: claude-haiku-4-5
weak-model: claude-haiku-4-5
cache-prompts: true
```

这样每次启动 Aider 都自动使用三模型分工，不需要每次在命令行指定。

### 深化：Claude Code 的 subagent 模型路由（实验性）

目前（`A/B + I`），Claude Code 的 `--agents` 参数支持为 subagent 指定不同的配置。理论上可以实现：
- 主会话用 Sonnet（负责高层决策）
- subagent 用 Haiku（负责文件探索）

但这需要通过 `--agents` 的 JSON 配置来实现，官方文档对 subagent 模型选择的说明目前不够详细（`A/B 待核验`）。

---

## 个人 / 协作注意

**个人使用**：对于 Aider 用户，三模型分工是性价比最高的配置改进之一——特别是对大量代码改写任务，把 `--editor-model` 设为 Haiku 能显著降低成本，同时不明显降低改写质量。

**团队协作**：在 `.aider.conf.yml` 里统一三模型配置，避免团队成员因为默认配置不同而有不同的成本结构。

---

## 延伸调查方向

1. **Sonnet vs Opus 在日常编码任务上的质量差距**：目前无系统性基准数据。architect 模式里，主模型的质量差距比不用 architect 模式时更明显（因为主模型只负责推理，不负责写代码）。具体差距需要实测。

2. **本地模型（Ollama/LM Studio）的实际可用性边界**：`codex --oss` 和 `aider --model ollama/...` 的实际使用体验，包括延迟、准确率和内存占用，没有系统性评测。

3. **工具内模型分工的真实边界**：Aider 的 `--editor-model` 在不同任务上分别节省了多少成本？在哪类任务上节省最多？目前无公开量化数据。

4. **推理 Token 的消耗分布**：extended thinking 模式（Claude）在不同任务类型下的推理 Token 比例，以及何时值得启用推理模式，目前没有系统性研究。官方有推理 Token 的价格说明（`A/B`），但任务级别的消耗分布没有公开数据。
