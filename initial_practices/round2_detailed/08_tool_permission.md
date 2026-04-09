# 第八章（深化）：工具权限克制——联网与 MCP 按需开关

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**默认采用最小权限。sandbox 模式和 approval policy 是两个独立的维度，需要分别选择，而不是把"有沙箱"等同于"安全"。**

---

## 正确的做法

### 深化：Codex CLI 的两维权限矩阵

round1 里的 sandbox 描述是对的，但把两个独立维度混在一起写容易造成误解。正确的理解是：

**维度一：sandbox（-s）— 控制文件系统访问范围**

| 值 | 实际含义 |
|----|---------|
| `read-only` | 只能读取文件，不能写入或执行会改变文件的命令 |
| `workspace-write` | 可以在工作目录（及 `--add-dir` 指定的额外目录）写入 |
| `danger-full-access` | 完全文件系统访问，无范围限制 |

**维度二：approval（-a）— 控制命令执行前是否需要人工审批**

| 值 | 实际含义 |
|----|---------|
| `untrusted` | 只有"可信"命令（ls、cat、sed 等）可无审批执行；其他命令请求人工确认 |
| `on-request` | 由模型决定何时请求审批（推荐用于交互式场景）|
| `never` | 永不请求审批，执行失败直接返回给模型（推荐用于非交互式自动化场景）|
| `on-failure` | **已废弃**，官方建议用 `on-request` 或 `never` 替代 |

`[A/B: codex resume --help 本机核验]`

**两维组合选择逻辑**（`I`）：

```
场景 → sandbox + approval 的推荐组合

只读分析 / 规划阶段：
  -s read-only -a untrusted
  "我只需要读文件，不允许任何写操作"

受控执行（交互式）：
  -s workspace-write -a on-request
  等价于 --full-auto，模型自主决定何时请示
  "允许修改工作区，但重要动作由模型判断是否请示"

自动化批处理（非交互式）：
  -s workspace-write -a never
  配合 codex exec --ephemeral
  "全自动执行，失败直接报错给模型处理"

不确定阶段（谨慎）：
  -s read-only -a on-request  或  --full-auto
  "宁可多问，不可乱改"
```

### 深化：Claude Code 的工具白名单比"关联网"更精确

round1 把 Claude Code 的权限控制简化成"关闭联网"，但实际上有更细粒度的控制：

```bash
# 精确指定工具白名单（A/B: claude --help 本机核验）
claude --allowedTools "Read Edit" "只读和编辑，禁止 Bash"

# 禁用所有工具（A/B）
claude --tools "" "只用知识回答，不调用工具"

# 规划模式（A/B: claude --help 本机核验）
claude --permission-mode plan "分析 src/auth/ 给出计划"
# plan 模式下：只能读取分析，禁止修改任何文件
```

`--allowedTools` 和 `--tools` 的区别（`I`）：
- `--allowedTools`：白名单，未列出的工具被禁用
- `--tools`：完整替换工具集，`""` 表示禁用所有，`"default"` 表示所有工具

---

## 错误的可能

### 新增错误：把 `on-failure` 写进脚本

`on-failure` 已被 Codex CLI 官方标记为**废弃（DEPRECATED）**。如果你的脚本或配置里还在用 `-a on-failure`，应该替换为：
- 交互式场景 → `-a on-request`
- 非交互式场景 → `-a never`

`[A/B: codex resume --help 本机核验]`

### 新增错误：误解 `danger-full-access` 的含义

`danger-full-access` 不是"比 workspace-write 稍微危险一点"，而是**完全不受文件系统范围限制**。它的正确用途是：在你自己已经通过外部机制（如 Docker 容器、CI 沙箱）确保安全的环境中，让 Codex 有最大自由度。在本地开发机器上，不应该默认使用这个模式。

---

## 背后的原理

### 深化：sandbox 和 approval 为什么是两个独立维度

这两个维度解决的是**不同层面的安全问题**：

- `sandbox` 解决的是：**如果 AI 决策出错，后果能不能被控制**（影响范围限制）
- `approval` 解决的是：**AI 在执行前是否需要人确认**（决策流程控制）

两者的组合形成了不同的风险控制策略（`I`）：

```
sandbox=read-only + approval=untrusted → 最保守，适合初始探索
sandbox=workspace-write + approval=on-request → 中等，适合日常开发
sandbox=workspace-write + approval=never → 信任 AI，适合批处理自动化
sandbox=danger-full-access + approval=never → 仅在外部已沙箱环境中使用
```

理解这两个维度分离，能避免一个常见的误解："有 sandbox 就安全了"。sandbox 只控制**能做什么**，approval 控制**做之前是否问人**。即使在 `read-only` sandbox 里，如果没有好的 approval policy，AI 仍然可以大量读取你不希望它读的内容。

### 深化：--permission-mode plan 的实际含义

`plan` 是 Claude Code 正式支持的 permission mode（`A/B`：claude --help 本机确认）。

官方文档说明：在 `plan` 模式下，Claude 只能读取文件和进行分析，被禁止修改任何文件。这使得规划阶段和执行阶段的分离有了**工具层的保障**，而不只是靠提示词"不要写代码"来约束。

```bash
# 规划阶段（A/B）
claude --permission-mode plan "分析现有认证架构，给出 OAuth2 迁移计划"

# 用户审查计划后，切换到执行阶段
claude --permission-mode default "按刚才的计划实现..."
```

---

## 延伸的习惯

### 深化：Codex CLI 的 `--ephemeral` 和任务类型的关系

`--ephemeral` 控制的是**会话文件是否持久化到磁盘**（`A/B`：codex exec --help 本机核验）。

这与 sandbox 和 approval 是第三个独立维度：

| 维度 | 控制什么 |
|------|---------|
| `sandbox` | 文件系统访问范围 |
| `approval` | 执行前是否请求人工确认 |
| `ephemeral` | 会话历史是否保存（是否可以 resume）|

**实践建议**（`I`）：
- 一次性批处理任务 → 加 `--ephemeral`（不需要恢复的会话，不要占用磁盘）
- 需要分步执行的复杂任务 → 不加 `--ephemeral`（保留 resume 能力）
- 探索性实验 → 加 `--ephemeral`（干净隔离，失败了重来）

### 深化：MCP 按场景连接的底层逻辑

Claude Code 提供了 `--strict-mcp-config` 参数（`A/B`：claude --help 本机核验），只加载 `--mcp-config` 指定的 MCP，忽略项目和全局的所有其他 MCP 配置。

这个参数的使用场景：当你需要完全控制当前任务的 MCP 访问权限时（比如只允许访问 Sentry，不允许访问数据库 MCP），用 `--strict-mcp-config` 确保其他 MCP 不会被意外激活。

```bash
# 只允许访问 Sentry，其他 MCP 全部禁用（A/B）
claude --mcp-config sentry.json --strict-mcp-config "分析生产错误"
```

---

## 个人 / 协作注意

**个人使用**：在日常脚本中，把 sandbox 和 approval 的选择和任务类型绑定，而不是每次都临时决定。建立自己的"三档配置"：只读探索 / 受控执行 / 批处理自动化，对应固定的 sandbox+approval 组合。

**团队协作**：
- 在团队脚本和 CI/CD 中禁止使用 `danger-full-access`，除非已有外部沙箱
- 明确区分"可以在本地用的配置"和"只能在 CI 环境用的配置"
- `--strict-mcp-config` 适合作为团队安全规范的一部分，强制限制共享脚本的 MCP 访问范围

---

## 延伸调查方向

1. **工具调用死循环的发生频率**：目前无可靠公开数据，待实测验证。关键是：不同工具是否有内置的工具调用次数上限？Claude Code 和 Codex CLI 是否有可配置的 max tool calls？

2. **Web Search vs MCP OpenAPI 在集成质量上的对比**：目前无系统性对比数据。合理的研究方向是：构造相同的 API 集成任务，对比两种方式的首次运行成功率。

3. **MCP 权限蔓延的实际案例**：目前无公开记录的事故案例。工程推论（`I`）：最高风险场景是数据库写入 MCP 在调试后未关闭，且后续任务是格式化或重构。

4. **Aider /add 精确限定 vs 开放目录的对比**：目前无公开对比数据。可以通过 `aider --read <所有文件> <目标文件>` 和 `aider .` 两种方式，在同一任务上对比 AI 的修改范围和准确率。
