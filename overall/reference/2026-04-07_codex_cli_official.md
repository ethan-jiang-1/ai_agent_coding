# Codex CLI 官方核验摘要

最后核验日期：2026-04-07
工具版本：`codex-cli 0.118.0`
核验方式：OpenAI 官方文档 + 本机 `codex --help`

## 官方来源

- OpenAI Codex 文档总入口：`https://developers.openai.com/codex/`
- Config Basics：`https://developers.openai.com/codex/config-basic`
- OpenAI 官方仓库：`https://github.com/openai/codex`
- 本机帮助：
  - `codex --help`
  - `codex exec --help`
  - `codex resume --help`
  - `codex fork --help`
  - `codex features list`

## 已确认事实

### 1. 会话不是“天然不可持续”

- `codex` 默认会启动交互式 CLI。
- 本机帮助明确存在：
  - `resume`
  - `fork`
  - `exec`
- 这说明 Codex CLI 既支持非交互式单次运行，也支持恢复/分叉已有会话。

### 2. 非交互式执行是正式能力

- `codex exec` 是官方命令。
- `codex exec --help` 明确说明可：
  - 直接传 prompt
  - 从 `stdin` 读 prompt
  - 用 `--ephemeral` 禁止持久化 session 文件

### 3. sandbox / approval 是一等概念

- 本机帮助和官方 config 文档都明确有：
  - `sandbox`
  - `ask-for-approval`
- 官方 config basics 当前示例值：
  - `approval_policy = "on-request"`
  - `sandbox_mode = "workspace-write"`

### 4. web search 不是默认关闭

- 官方 config basics 写明：
  - `web_search = "cached"` 为默认
  - `web_search = "live"` 等价于 `--search`
  - `web_search = "disabled"` 可显式关闭
- 所以不能把 Codex CLI 写成“默认完全无联网能力”。

### 5. 规则 / 指令体系不应写成 `codex.md`

- OpenAI Codex 文档导航当前明确有：
  - `Rules`
  - `Hooks`
  - `AGENTS.md`
  - `Skills`
  - `Subagents`
- 当前更稳的官方术语是 `AGENTS.md`，不是 `codex.md`。

### 6. Subagents / Multi-agent 是真实能力

- 官方文档导航明确有 `Subagents`。
- 官方 config basics 特性表出现：
  - `multi_agent`：stable，默认 `true`
- 本机 `codex features list` 也显示：
  - `multi_agent stable true`

### 7. 本机 CLI 当前还有这些关键命令

- `review`
- `mcp`
- `sandbox`
- `apply`
- `app`
- `cloud`

这意味着 Codex 的能力边界不能只写成“一个命令跑完就结束的本地脚本工具”。

## 对 round1 的直接修正建议

### 可以安全写成事实的

- Codex CLI 支持交互式会话、非交互式执行、恢复会话、分叉会话。
- Codex CLI 有正式的 sandbox / approval / web search 配置。
- Codex 文档体系包含 `AGENTS.md`、Rules、Skills、Subagents。
- Codex 具备多智能体/子智能体相关能力，但具体触发方式应以官方文档当前写法为准。

### 不应继续写成事实的

- “每次 `codex` 命令天然就是新会话，没有持续会话概念”
- “Codex 的规则文件是 `codex.md`”
- “Codex 默认关闭联网搜索”
- “Codex CLI 的多智能体能力等于用户在一个 prompt 里写‘并行执行三个任务’”

## round1 中最容易误写的点

- 会把 `tool`、`model`、`product` 混成一层
  - `Codex CLI` 是工具，不是模型名
- 会把 `exec` 的一次性执行语义误扩展到整个产品
- 会把 `features list` 里的能力直接等同成所有用户界面都稳定暴露

## 建议在正文里采用的表述

- 如果要强调一次性执行，写：
  - “用 `codex exec` 可以做无状态、非交互式执行”
- 如果要强调会话延续，写：
  - “Codex CLI 也支持 `resume` / `fork` 这样的持续会话工作流”
- 如果要强调规则文件，写：
  - “先核对 `AGENTS.md` / Rules 的官方约定，再写入实践建议”

