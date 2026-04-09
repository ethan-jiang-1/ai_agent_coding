# round1 工具 Grounding 与逐文件校准计划

日期：2026-04-07
范围：`/Users/bowhead/ai_agent_coding/overall/round1`
目标目录：`/Users/bowhead/ai_agent_coding/overall/reference`

## 1. 任务理解

这次工作不是“润色 round1”，而是先补齐一套可追溯的官方 Grounding Truth，再逐文件核对并修正 `round1` 中所有涉及 AI Coding 工具的具体说法，尤其包括：

- 命令名、CLI 参数、slash commands
- 配置文件路径、配置键名、规则文件位置
- 会话模型、Plan/Ask/Agent 等模式定义
- 子智能体 / 多智能体 / worktree / 后台 agent 等能力边界
- MCP、联网、sandbox、approval 等权限模型
- 价格、额度、配额、默认行为这类高时效信息

验收标准不是“看起来合理”，而是“能追溯到官方文档或本机 CLI 实测帮助输出”。

---

## 2. Source of Truth 规则

### 2.1 证据优先级

1. 官方产品文档
2. 本机已安装 CLI 的 `--help` / 内置命令帮助
3. 官方 GitHub 仓库 README / docs
4. 官方定价页 / 官方 changelog
5. 社区材料仅用于“案例”或“用户反馈”，不能作为规范性用法依据

### 2.2 当前已确认的官方来源入口

- Codex / Codex CLI
  - OpenAI Codex 文档：`developers.openai.com/codex/*`
  - OpenAI 官方仓库：`github.com/openai/codex`
  - 本机：`codex --help`、`codex exec --help`、`codex resume --help`、`codex fork --help`
- Claude Code
  - Anthropic Claude Code 文档：`docs.anthropic.com/en/docs/claude-code/*`
  - 本机：`claude --help`、`claude agents --help`、`claude mcp --help`
- Cursor
  - 官方文档：`docs.cursor.com/*`
- Aider
  - 官方文档：`aider.chat/docs/*`
  - 本机：`aider --help`

### 2.3 写作约束

- 规范性陈述必须能被官方 source 验证
- 如果官方文档没有明确写，就改写成“工作流建议”，不能写成“工具内置事实”
- 所有价格、额度、默认值都必须加“最后核验日期”
- 同一个工具在不同章节中的叫法必须统一，不能本章说“Plan Mode”，下章换成另一个未定义名词

---

## 3. 预先发现的高风险点

这些不是最终结论，而是后续必须优先核验的地方。

### 3.1 Codex CLI 相关

- `round1` 多处把 Codex CLI 写成“每条命令天然新会话、没有持续会话概念”
  - 风险原因：本机 `codex` 明确存在 `resume` / `fork` / interactive session
- `round1` 把 Codex 规则文件写成 `codex.md`
  - 风险原因：OpenAI Codex 文档导航明确存在 `AGENTS.md` 专章，需优先核对
- `round1` 把 Codex 默认联网写成“默认关闭”或只在命令显式开启时可用
  - 风险原因：OpenAI Codex config 文档显示 `web_search = "cached"` 为默认值
- `round1` 对 Codex 多智能体/子智能体的描述较强
  - 风险原因：官方确实有 `multi_agent` / subagents 文档，但具体触发方式、默认行为、CLI 用法不能凭印象写

### 3.2 Claude Code 相关

- 需要核对 `.claude/rules/`、`CLAUDE.md`、`/memory`、auto memory、`/compact`
  - 当前初步看，官方文档支持这些概念，但细节边界必须统一
- 需要核对 Plan Mode 的入口、快捷键、权限语义
  - 当前初步看，`plan` permission mode、`Shift+Tab` 切换在官方文档中可见

### 3.3 Cursor 相关

- `round1` 把 Cursor 写成“Plan Mode”
  - 风险原因：官方文档当前更明确的是 `Agent / Ask / Manual / Custom` modes；“Plan”更像 custom mode 示例，不一定是内置标准模式名
- 需要核对 `.cursor/rules/*.mdc`、`alwaysApply`、`AGENTS.md`、`.cursorrules` 的现状和层级关系

### 3.4 Aider 相关

- `round1` 把 Aider 描述成“后台构建整个代码库的 AST 图谱”
  - 风险原因：官方和本机帮助更明确的是 repo map / repository map，不应擅自等同成 AST 图谱
- 需要核对 `/clear`、`/undo`、`--read`、只读工作流等具体命令是否与文稿一致

### 3.5 计费和额度相关

- `11_billing_tips.md` 风险最高
  - Cursor / Windsurf / Trae / Claude / Copilot 的价格、额度、队列、倍率都属于高时效信息
  - 这部分不能靠旧印象，必须逐项按当前官方页重验，并在文中标注核验日期

---

## 4. reference 目录建设方案

`overall/reference` 先不做“大杂烩素材堆”，而是做两层结构：

### 4.1 第一层：工具级权威摘要

计划产出：

- `reference/2026-04-07_codex_cli_official.md`
- `reference/2026-04-07_claude_code_official.md`
- `reference/2026-04-07_cursor_official.md`
- `reference/2026-04-07_aider_official.md`

每份摘要统一包含：

- 官方来源列表
- 最后核验日期
- 已确认能力
- 未确认 / 文档未写明部分
- 不应在 round1 中写成确定事实的说法

### 4.2 第二层：横向专题矩阵

计划产出：

- `reference/2026-04-07_tool_capability_matrix.md`
- `reference/2026-04-07_pricing_limits_matrix.md`

矩阵字段建议：

- 会话持久化
- 规则文件
- 子智能体 / 后台 agent
- 权限模式
- 沙箱与联网
- 只读规划模式
- 多工作区 / worktree / 远程环境
- 价格 / 配额 / 默认值 / 最后核验日期

---

## 5. round1 逐文件审查顺序

### 第一优先级：先修“工具事实密度最高”的文件

1. `03_plan_before_code.md`
2. `04_rule_hierarchy.md`
3. `07_model_routing.md`
4. `08_tool_permission.md`
5. `09_memento_workflow.md`
6. `10_multi_agent.md`
7. `11_billing_tips.md`
8. `00_overview.md`

### 第二优先级：再修“局部用法示例”

9. `01_session_lifecycle.md`
10. `02_edit_not_append.md`
11. `05_progressive_feed.md`
12. `06_kv_cache.md`

---

## 6. 每个文件的审查焦点

### `00_overview.md`

- 只保留各工具最稳的高层能力描述
- 删除或改写未经官方证实的“并发子智能体”“AST 图谱”等强说法

### `01_session_lifecycle.md`

- 核对 `/clear`、`/compact`、Aider 会话重置、Codex session/resume 语义
- 重点修正“Codex 天然没有持续会话”这类说法

### `02_edit_not_append.md`

- 核对 Cursor 是否真的“编辑旧提示后自动撤销旧改动”
- 核对 Aider 的 `/undo`
- 把 Codex CLI 的“rerun”写成真实可用的 session / exec / rerun 语义

### `03_plan_before_code.md`

- Claude Code：Plan Mode / permission mode / 只读约束
- Cursor：Ask / Manual / Custom Mode 与“Plan”说法的关系
- Aider：是否只能通过提示词和 `--read` 模拟只读规划
- Codex：官方是否有 plan 类工作流、subagents、non-interactive planning 的更准写法

### `04_rule_hierarchy.md`

- Claude Code：`CLAUDE.md`、`.claude/rules/`、skills
- Cursor：`.cursor/rules/*.mdc`、`AGENTS.md`、`.cursorrules` 的新旧关系
- Codex：`AGENTS.md`、`.codex/config.toml`、rules/hooks/skills/subagents
- Aider：没有内建规则分层时，如何准确表述 `--read` / conventions 类替代方案

### `05_progressive_feed.md`

- 核对各工具对 `@file` / 文件引用 / stdin / MCP 的真实支持方式
- 区分“产品原生特性”和“用户工作流技巧”

### `06_kv_cache.md`

- 只保留官方明确可证的 prompt caching 机制
- 避免把工具级缓存、模型级缓存、RAG 索引混成一个概念

### `07_model_routing.md`

- 核对当前模型名、是否还存在、是否适用于对应工具
- 把“工具”与“模型”分层，不再把 `Codex CLI` 当作和 `GPT-5` 同一层对象
- 核对 Claude / Aider / Cursor / Codex 的模型切换方式与配置键

### `08_tool_permission.md`

- 核对 Codex / Claude 的 approval mode、sandbox、MCP、web search 默认行为
- 把“工具调用上限”与“产品内置 hard limit”分开写
- 删除或降级没有官方依据的具体配置片段

### `09_memento_workflow.md`

- 核对 Claude Code auto memory 真实存储路径、读取方式、`/memory`
- 核对 Codex 跨会话状态传递是“外部 handoff 文件建议”还是“工具内建 resume/fork/session”

### `10_multi_agent.md`

- Claude Code：subagents、agent teams、worktree、并行工作流
- Cursor：background agents 的真实边界
- Codex：subagents / multi_agent / app / cloud task / CLI 的准确分层

### `11_billing_tips.md`

- 逐平台逐项重验，不保留任何未核验数字
- 所有平台计费信息增加绝对日期
- 不能证实的“社区倍率传闻”全部移除或改成案例性描述

---

## 7. 执行方法

### 阶段 A：补 reference

- 先做 Codex CLI / Claude Code / Cursor / Aider 四份官方摘要
- 再做横向能力矩阵和价格矩阵

### 阶段 B：抽取 round1 中的“可核验陈述”

把每一条工具陈述分成五类：

- `CMD` 命令/快捷键
- `CFG` 配置文件/配置键
- `CAP` 能力/限制/默认行为
- `FLOW` 推荐工作流
- `PRICE` 价格/额度/倍率/套餐

### 阶段 C：逐条打标签

每条陈述打一个状态：

- `V-doc`：官方文档验证
- `V-local`：本机 CLI 帮助验证
- `V-both`：文档 + 本机都验证
- `Anecdote`：只能作为案例，不能作为规范
- `Drop`：无法证实，删除或重写

### 阶段 D：修 round1

修订原则：

- 先修事实错误，再修行文风格
- 工具名、模式名、文件名一律和官方一致
- 凡是容易变动的数字，加核验日期
- 同一类概念只保留一个主术语

### 阶段 E：全局一致性复查

- 会话模型是否前后冲突
- 规则文件命名是否前后冲突
- Plan / Ask / Agent / Manual / subagent 术语是否混用
- 价格、套餐、倍率是否跨章冲突

---

## 8. 本轮输出物

完成整个任务后，预期会得到：

1. `overall/reference/` 下的官方摘要与能力矩阵
2. `overall/round1/` 下逐文件修订后的稿件
3. 一份简短审计记录，说明哪些说法被确认、哪些被重写、哪些被删除

---

## 9. 先做什么

执行顺序建议固定为：

1. 先补 `reference/2026-04-07_codex_cli_official.md`
2. 再补 `reference/2026-04-07_claude_code_official.md`
3. 然后补 `reference/2026-04-07_cursor_official.md`
4. 再补 `reference/2026-04-07_aider_official.md`
5. 之后先修 `04_rule_hierarchy.md` 和 `10_multi_agent.md`
6. 再回头修 `03_plan_before_code.md`、`08_tool_permission.md`、`11_billing_tips.md`
7. 最后做全局一致性收口

原因：

- `04` 和 `10` 最容易把工具能力写错
- `11` 最容易因为时间变化而失真
- `00` 必须最后回收，才能和全文一致

---

## 10. 当前判断

你的要求我理解为：

- 先把“真实工具能力”这层地基打牢
- 再去判断 `round1` 里的建议是不是建立在真实工具之上
- 不接受“经验上大概这样”的写法

这个理解是对的。后续我就按这份计划，先补 `overall/reference`，再逐文件校准 `overall/round1`。
