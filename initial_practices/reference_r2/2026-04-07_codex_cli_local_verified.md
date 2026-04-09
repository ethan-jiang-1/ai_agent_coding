# Codex CLI 官方能力核验

来源：本机 `codex --help`、`codex exec --help`、`codex resume --help`、`codex fork --help`
核验日期：2026-04-07
Codex CLI 版本：本机已安装版本

---

## 1. 主命令结构

```
codex [OPTIONS] [PROMPT]          # 交互式会话
codex [OPTIONS] <COMMAND> [ARGS]  # 非交互式子命令
```

子命令列表（已确认）：
- `exec` / `e` — 非交互式执行
- `review` — 非交互式代码审查
- `resume` — 恢复历史交互式会话
- `fork` — 分叉历史交互式会话
- `sandbox` — 在 Codex 沙箱中运行命令
- `apply` / `a` — 将 agent 产生的最新 diff 应用到本地工作树
- `mcp` — 管理外部 MCP 服务器
- `cloud` — [EXPERIMENTAL] 浏览 Codex Cloud 任务并本地应用变更

---

## 2. Sandbox 模式（-s / --sandbox）

三种值，均已在 `exec` 和 `resume`/`fork` 的帮助中确认：

| 值 | 说明 |
|----|------|
| `read-only` | 只读沙箱，模型生成的命令只能读取文件 |
| `workspace-write` | 可写入工作区，但受限范围 |
| `danger-full-access` | 完全访问权限，不推荐用于不可信环境 |

**注意**：`codex exec --help` 里还有 `--dangerously-bypass-approvals-and-sandbox`，说明是"跳过所有确认提示并在无沙箱情况下执行命令，极其危险，仅用于外部已沙箱化的环境"。

---

## 3. Approval Policy（-a / --ask-for-approval）

四种值（均在 `resume` 和 `fork` 帮助中有完整说明）：

| 值 | 说明 |
|----|------|
| `untrusted` | 只有"可信"命令（如 ls, cat, sed）可无需审批执行；其他命令会请求用户审批 |
| `on-failure` | **已废弃（DEPRECATED）**。所有命令无需审批，只在命令失败时才升级请求审批。官方建议用 `on-request` 或 `never` 替代 |
| `on-request` | 由模型决定何时请求用户审批 |
| `never` | 永不请求用户审批，执行失败直接返回给模型 |

**便捷别名**：`--full-auto` = `-a on-request --sandbox workspace-write`

---

## 4. exec 命令的关键参数

```bash
codex exec [OPTIONS] [PROMPT]
```

关键参数：
- `--ephemeral`：不将会话文件持久化到磁盘（临时会话，不可 resume）
- `-s` / `--sandbox <MODE>`：sandbox 模式（read-only / workspace-write / danger-full-access）
- `-a` / `--ask-for-approval <POLICY>`：审批策略
- `--full-auto`：= `-a on-request --sandbox workspace-write` 的便捷别名
- `--cd <DIR>`：指定工作根目录
- `--add-dir <DIR>`：指定额外可写目录
- `--output-last-message <FILE>`：将 agent 最后一条消息写入文件
- `-` 作为 PROMPT：从 stdin 读取指令（支持管道）

---

## 5. resume 命令

```bash
codex resume [SESSION_ID] [PROMPT]
```

- `SESSION_ID`：会话 UUID 或线程名。UUID 优先。
- `--last`：直接恢复最近一次会话（不显示选择器）
- `--all`：显示所有会话（不过滤当前目录）
- `--include-non-interactive`：在选择器中包含非交互式会话（exec 产生的）

resume 和 fork 都支持 `-s` sandbox、`-a` approval policy，与 exec 一致。

---

## 6. fork 命令

```bash
codex fork [SESSION_ID] [PROMPT]
```

- 从历史会话创建新分支（独立上下文，保留历史）
- `SESSION_ID`：要分叉的会话 UUID
- `--last`：从最近一次会话分叉
- 分叉后的新会话是全新的独立会话，不影响原会话

**resume vs fork 的区别**：
- `resume`：继续同一会话，追加历史
- `fork`：基于历史会话创建新分支，原会话不变

---

## 7. --oss 和 --local-provider

```bash
codex --oss              # 使用本地 OSS provider（LM Studio 或 Ollama）
codex --oss --local-provider ollama
codex --oss --local-provider lmstudio
```

等价于 `-c model_provider=oss`。

---

## 8. Web Search

`--search` 参数（在 resume 和 fork 中确认）：
- 启用时，native Responses `web_search` 工具对模型可用
- **无需每次调用都审批**（和 MCP 的 web search 不同）

---

## 9. config.toml 覆盖

`-c key=value` 语法可覆盖 `~/.codex/config.toml` 中的任意配置：
- `-c model="o3"`
- `-c 'sandbox_permissions=["disk-full-read-access"]'`
- `-c shell_environment_policy.inherit=all`

---

## 10. 不确认的内容（本机 CLI 未提供）

- sandbox 的 OS 级实现细节（macOS Seatbelt / Linux Landlock）— 在帮助文本中未提及
- `subagents` / `multi_agent` 的具体触发方式 — 帮助文本未提及
- `AGENTS.md` 的加载行为 — 帮助文本未提及（仅 `--no-project-doc` 提示存在项目文档加载）
- history 配置（maxSize、saveHistory、sensitivePatterns）的详细说明 — 帮助文本未提及

---

## 11. 已修正的 round1 错误

| round1 错误写法 | 正确写法 |
|----------------|---------|
| `-s read-only -a untrusted` | `-s read-only -a untrusted`（这个是对的）|
| `-s workspace-write -a on-request` | `-s workspace-write -a on-request`（对，等价于 `--full-auto`）|
| `codex exec --sandbox read-only` | 正确写法是 `codex exec -s read-only`（`--sandbox` 是全名，`-s` 是简写，两者都对）|
| `codex --oss "..."` | 确认正确 |
| `approval: on-failure` | **已废弃**，官方建议用 `on-request` 或 `never` |
