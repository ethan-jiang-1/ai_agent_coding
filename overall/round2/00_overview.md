# 导言（深化）：从 Prompt 工程师到上下文工程师

> 本章为 round1 导言的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 你已经读过 round1，现在读什么

Round1 建立了框架：上下文窗口是计算内存，任务边界即会话边界，规则文件要精简，信息投喂要渐进。

Round2 做的事是：把框架里的工程推论（`I`）尽可能替换成有官方依据的事实（`A/B`），并对 round1 发现的几处重要错误做修正。

---

## Round1 的主要修正（来自 grounding audit）

以下是 round1 grounding audit 后发现的重要错误，round2 各章已做修正：

| 错误位置 | Round1 错误写法 | 正确写法 | 来源 |
|---------|----------------|---------|------|
| 多章 | Codex CLI 规则文件是 `codex.md` | 正式载体是 `AGENTS.md` | `A/B`：OpenAI Codex 文档 |
| 01、09 等 | "Codex CLI 天然每次命令都是独立新会话" | Codex CLI 有 `resume`、`fork`、`exec --ephemeral` 三种工作流，并非天然无状态 | `A/B`：本机 CLI 核验 |
| 08 | `approval: on-failure` 作为推荐选项 | `on-failure` 已废弃，推荐 `on-request`（交互式）或 `never`（非交互式）| `A/B`：本机 CLI 核验 |
| 03 | Cursor 有内置"Plan Mode" | Cursor 的 Ask 模式更接近只读规划；"Plan Mode"更像是自定义模式的一种 | `A/B`：Cursor 官方文档 |
| 05 | Aider "后台构建整个代码库的 AST 图谱" | Aider 使用 repo map（图排名算法），不是 AST 图谱 | `A/B`：Aider 官方文档 |
| 11 | 把 Codex CLI 列为旗舰模型的替代 | Codex CLI 是工具，不是模型；它通过 `--model` 选择底层模型 | `A/B`：本机 CLI 核验 |

---

## 工具生态的当前定位（基于 grounding audit）

Round1 对各工具的定位做了大量描述，但在 grounding audit 之前，部分描述混淆了"工具能力"和"用户工作流"。修正后的定位（`A/B`）：

**Claude Code**：
- 核心能力：交互式会话（--continue/--resume/--fork-session）、非交互式（--print）、subagents（.claude/agents/）、worktree（-w）
- 成本控制：--max-budget-usd（仅 --print）、--permission-mode plan、--model 切换
- 状态管理：auto-memory（MEMORY.md，自动），session 文件（可 resume）

**Aider**：
- 核心能力：repo map（图排名，自动），三模型分工（--model / --editor-model / --weak-model），--cache-prompts，/undo
- 规则加载：必须显式加载（--read 或 .aider.conf.yml），不会自动感知 CONVENTIONS.md
- 状态管理：.aider.chat.history.md（自动），AIDER_RESTORE_CHAT_HISTORY（可配置）

**Codex CLI**：
- 核心能力：exec（非交互式，支持 --ephemeral）、resume/fork（会话管理）、沙箱（-s）+ 审批（-a）两维权限矩阵
- 规则载体：AGENTS.md（不是 codex.md）
- 本地模型：--oss（LM Studio 或 Ollama）

**Cursor**：
- 规则载体：.cursor/rules/*.mdc（推荐）和 AGENTS.md；.cursorrules 是 legacy
- 工作模式：Ask（只读探索）、Agent（执行）；"Plan Mode"是用户自定义模式

---

## Round2 的已知局限

Round2 通过官方文档研究和本机 CLI 核验，解决了 round1 里主要的事实错误。但仍然有一些无法从公开资料核验的内容，在各章里标注为"目前无可靠公开数据，待实测验证"。

主要的待验证项目：
- Context rot 的量化临界点（需要系统性实测）
- 各工具规划阶段的实际效果（需要对照实验）
- 缓存命中率的实际数字（依赖 provider 的内部统计）
- 本地模型在不同任务类型上的可用性边界（需要系统性评测）

这些问题不是 round2 回避了，而是目前确实没有可靠的公开数据来回答。

---

## 全文结构速览（round2）

每章在 round1 基础上深化的核心方向：

```
01 会话生命周期     ← 修正 Codex CLI 会话机制；Aider 的历史摘要自动化
02 三次纠错法则     ← Aider /undo；Codex CLI fork 作为 Edit & Rerun
03 规划先于执行     ← --permission-mode plan 的工具级保障；-s read-only
04 规则文件分层     ← AGENTS.md 是 Codex 的正式载体；Aider 必须显式加载
05 渐进式信息投喂   ← Aider repo map 的工作机制；各工具引用机制对比
06 KV Cache 经济学  ← Aider --cache-prompts 的官方范围；/compact 的规则保护
07 模型路由分级     ← Aider 三模型分工的详细说明；--oss 的本地模型入口
08 工具权限克制     ← sandbox + approval 两维矩阵；on-failure 已废弃
09 跨会话状态传递   ← Codex resume/fork 的区别；auto-memory 的存储行为
10 多智能体分流     ← Claude Code subagents 的存储路径；exec 串行模式
11 订阅额度陷阱     ← --max-budget-usd 仅在 --print 有效；三模型分工的成本含义
```
