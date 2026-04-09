# Codex CLI 官方参考手册

最后核验日期：2026-04-07  
本机版本：`codex-cli 0.118.0`

## 1. 参考定位

这份文件不是“几条结论”，而是给 `round1` 写作时当作 grounded reference 用的手册级底稿。

本文件把证据分成三层：

- `A 级`：OpenAI 官方文档正文可直接读取，或本机 `codex` 帮助可直接核验
- `B 级`：OpenAI 官方仓库文档/配置 schema/源码树可直接读取
- `C 级`：官方搜索结果摘要能确认方向，但正文没有在当前抓取里逐行展开

当前 Codex CLI 部分以 `A/B 级` 为主，可信度最高。

## 2. 官方来源

### A 级：官方文档正文

- `https://developers.openai.com/codex/`
- `https://developers.openai.com/codex/config-basic`
- `https://developers.openai.com/codex/noninteractive`
- `https://developers.openai.com/codex/agent-approvals-security`

### B 级：OpenAI 官方仓库 / 原始文档

- `https://github.com/openai/codex`
- `https://raw.githubusercontent.com/openai/codex/main/docs/config.md`
- `https://raw.githubusercontent.com/openai/codex/main/docs/agents_md.md`
- `https://raw.githubusercontent.com/openai/codex/main/docs/skills.md`
- `https://raw.githubusercontent.com/openai/codex/main/codex-rs/core/config.schema.json`
- `https://raw.githubusercontent.com/openai/codex/main/codex-rs/core/hierarchical_agents_message.md`

### A 级：本机 CLI 交叉核验

- `codex --version`
- `codex --help`
- `codex exec --help`
- `codex resume --help`
- `codex fork --help`
- `codex features list`

## 3. 产品边界

写 `round1` 时必须先区分三层：

- `Codex CLI` 是工具
- `GPT-5 / gpt-5.4 / gpt-5.1` 是模型或模型别名
- `Codex App / Codex Web / IDE integration` 是不同产品入口

不能把 `Codex CLI` 写成一个模型名，也不能把一次 `codex exec` 的体验写成整个 Codex 产品的唯一工作方式。

## 4. 会话模型

### 4.1 交互式会话是默认路径

本机 `codex --help` 明确写着：

- 不带子命令时进入交互式 CLI
- 支持直接输入 prompt 启动 session

所以它不是“只支持一次性命令运行”的工具。

### 4.2 会话是可持续的

本机帮助直接提供：

- `codex resume`
- `codex fork`

这意味着：

- 会话可以恢复
- 会话可以分叉
- “每次运行天然就是全新无状态隔离会话”这个说法不成立

### 4.3 非交互式执行是正式能力，但只是其中一种能力

本机帮助和官方 non-interactive 文档共同确认：

- `codex exec` 用于非交互式运行
- 支持从参数或 `stdin` 读 prompt
- 支持 `--json`
- 支持 `--output-schema`
- 支持 `--output-last-message`
- 支持 `--ephemeral`

`--ephemeral` 的语义非常关键：

- 它表示这次 `exec` 不落盘 session 文件
- 它不是说 Codex 整个产品天然都没有会话持久化

### 4.4 针对 `round1` 的安全写法

可以写：

- “如果你要做一次性、可重放的批处理，`codex exec --ephemeral` 是官方路径”
- “如果你要延续上下文，Codex CLI 也支持 `resume` / `fork`”

不要写：

- “Codex 没有 mixed-session 问题，因为每次命令都是全新会话”

## 5. 配置体系

官方 `config-basic` 与官方仓库文档共同确认：

- 用户级配置文件：`~/.codex/config.toml`
- 项目级配置文件：`.codex/config.toml`
- CLI 参数可以覆盖配置
- 支持 profile

官方搜索摘要还能确认：

- 配置可以给默认 profile 和命名 profile 设不同默认值
- 当前目录被标记为 trusted project 后，项目配置会自动生效

对 `round1` 的意义：

- Codex 不是只有 prompt，没有正式配置面
- sandbox / approval / web search / model / mcp 都是正式配置项

## 6. 指令、规则与 AGENTS.md

### 6.1 不能再把规则文件写成 `codex.md`

官方文档和官方仓库都明确使用：

- `AGENTS.md`
- `Rules`
- `Skills`
- `Subagents`

所以写 `codex.md` 是错误 grounding。

### 6.2 AGENTS.md 的作用域和覆盖顺序

官方仓库里的 `hierarchical_agents_message.md` 直接说明：

- 每个 `AGENTS.md` 管辖其所在目录及所有子目录
- 深层目录的 `AGENTS.md` 覆盖上层目录的冲突内容
- 系统 / developer / user prompt 优先级高于 `AGENTS.md`

这是写“规则层级”时最值得直接引用的底层原则。

### 6.3 官方搜索摘要给出的更细约束

官方搜索结果可确认这些点：

- Codex 会在工作目录向上递归查找 `AGENTS.md`
- 默认最多注入 32KiB 的 `AGENTS.md` 内容
- 还支持 `AGENTS.override.md`
- `project_doc_fallback_filenames` 可以扩展等价规则文件名

这些点说明：

- AGENTS 体系是正式、可配置、层级化的
- 不是“碰巧读一个根目录说明文件”

## 7. Skills

官方仓库文档和文档导航可以确认：

- Skills 是正式概念
- Skills 不是社区自创术语
- 官方仓库自身就内置 `.codex/skills/...`

官方搜索摘要给出的关键机制：

- skills 可以显式触发，也可以隐式触发
- Codex 会从当前目录向 repo 根目录寻找 `.agents/skills`
- skills 强调 progressive disclosure，只在需要时载入

写 `round1` 时可以说：

- “Codex 有正式的 skills 机制，适合把专用工作流封装成局部知识包”

不要写：

- “Codex 只能靠一个大 system prompt”

## 8. Approvals / Sandbox / Network

### 8.1 这三件事是第一等概念

本机帮助里直接有：

- `--sandbox`
- `--ask-for-approval`
- `--search`

官方 config schema 也直接有：

- `sandbox_mode`
- `approval_policy`
- `web_search`

### 8.2 可确认的枚举值

本机帮助和 schema 共同确认：

- `sandbox_mode`:
  - `read-only`
  - `workspace-write`
  - `danger-full-access`
- `approval_policy`:
  - `untrusted`
  - `on-request`
  - `on-failure`（已标为 deprecated）
  - `never`
  - 以及 granular 细粒度对象形式
- `web_search`:
  - `disabled`
  - `cached`
  - `live`

### 8.3 官方文档当前默认建议

`config-basic` 当前示例和官方搜索摘要共同表明：

- `approval_policy = "on-request"`
- `sandbox_mode = "workspace-write"`
- `web_search = "cached"`

所以不能把 Codex CLI 写成：

- “默认完全不联网”
- “默认没有正式 web search 机制”

### 8.4 sandbox 的真实含义

官方安全文档搜索摘要可确认：

- `workspace-write` 默认允许写工作区和系统临时目录
- 默认阻止向其他目录写入
- 默认阻止出站网络
- 受保护路径包括 `.git/`、`.agents/`、`.codex/`
- macOS 用 Apple Seatbelt
- Linux 用 `bubblewrap`
- Windows 用 Windows Sandbox

因此：

- 可以写“Codex 有正式 OS 级沙箱实现”
- 不要写成笼统的“某种抽象权限模型”
- 也不要误写成“默认随便联网”

### 8.5 `round1` 的表述建议

推荐：

- “Codex 的 sandbox / approval / web search 都是正式配置项”
- “`workspace-write` 不是无限写权限，默认还有 protected paths 和默认无出站网络”

不推荐：

- “Codex 默认就是 kernel 级完全隔离，且所有网络都被永久禁掉”
- “Codex 默认关闭搜索能力”

## 9. Subagents / Multi-agent

### 9.1 这是官方能力，不是用户幻想

本机 `codex features list` 明确显示：

- `multi_agent stable true`

官方文档导航和搜索摘要还能确认：

- Codex 有独立的 Subagents 文档
- 子智能体可以有独立配置和职责
- 自定义子智能体可放在：
  - `~/.codex/agents/`
  - `.codex/agents/`

### 9.2 官方搜索摘要提供的可写事实

- 可用内置角色包括 `default`、`worker`、`explorer`
- 父级 runtime override 在实际运行时会高于 agent 自带默认值
- 公共文档描述里强调“明确任务边界”和“并行独立子任务”

### 9.3 但不要把能力偷换成固定 UX

可以写：

- “Codex 官方支持 subagents / multi-agent 能力”
- “具体触发体验取决于当前版本、界面和启用特性”

不要直接写：

- “用户只要在 prompt 里写一句并行开 3 个 agent，Codex 一定会以同样 UI 方式稳定执行”

## 10. 自动化与 CI

官方 non-interactive 文档和本机帮助确认：

- `codex exec` 是官方自动化入口
- 默认可在 CI 里使用
- 推荐显式设置 sandbox / approval
- `--ephemeral` 适合一次性自动化任务
- `resume` 可以把非交互式运行接回后续工作流

对 `round1` 的直接影响：

- 规划阶段可以用 `codex exec -s read-only --ephemeral`
- 执行阶段再切换到可写 sandbox

## 11. 写 `round1` 时的红线

### 11.1 可以直接写成事实的

- Codex CLI 支持交互式会话、恢复会话、分叉会话、非交互式执行
- `AGENTS.md` 是正式指令文件体系的一部分
- Codex 有正式的 sandbox / approval / web search 配置
- Codex 官方支持 subagents / multi-agent 能力

### 11.2 必须删掉或改写的

- “Codex 的规则文件叫 `codex.md`”
- “Codex 天然没有持续会话概念”
- “Codex 默认完全无联网能力”
- “Codex 的多 agent 就等于 prompt 里喊一句并行执行三个任务”

## 12. 给 `round1` 的推荐话术

### 会话

“Codex CLI 同时支持交互式 session、`codex exec` 非交互式运行，以及 `resume` / `fork` 这种延续型工作流。”

### 规划

“需要只读规划时，可用 `codex exec -s read-only --ephemeral`；需要执行时再切换到可写 sandbox。”

### 规则

“Codex 的正式项目指令体系应以 `AGENTS.md` 为准，并遵守目录层级覆盖规则。”

### 多 agent

“Codex 官方支持 subagents / multi-agent，但正文应避免把某个版本界面的具体 UX 写成永恒不变的产品事实。”
