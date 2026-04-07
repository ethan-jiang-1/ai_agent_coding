# Aider 官方能力核验

来源：Aider 官方文档（经 agent 研究，本机 aider 文档目录：/Users/bowhead/aider/）
核验日期：2026-04-07

---

## 1. Repository Map（Repo Map）

**官方定义**：整个 git 仓库的简明映射，包含最重要的类、函数及其类型和调用签名。

**工作机制**：
- 使用**图排名算法**（graph ranking algorithm）分析依赖关系
- 将每个源文件视为节点，依赖关系视为边
- 识别并选出与当前聊天状态最相关的部分

**大小控制**：
- `--map-tokens`：控制 repo map 的 token 预算，**默认 1k tokens**
- 对大型仓库，Aider 自动优化地图以适应"活跃 token 预算"
- 聊天中没有添加任何文件时，Aider 可能显著扩大地图大小

**与 context window 的关系**：
- repo map 是 context window 的一部分
- Aider 动态平衡 repo map 大小和文件内容大小

**重要**：repo map 不等于 AST 图谱。round1 里"后台构建整个代码库的 AST 图谱"的说法**不准确**，官方说法是 repo map（图排名算法，不是 AST）。

---

## 2. Prompt Caching（--cache-prompts）

**官方说明**：启用后，Aider 会组织聊天历史以利用 LLM 提供商（Anthropic、DeepSeek）的缓存功能，**降低成本并提高速度**。

**缓存范围**（官方明确列出）：
1. 系统提示词（System prompt）
2. 通过 `--read` 或 `/read` 添加的**只读文件**
3. 仓库地图（Repository map）
4. 已添加到聊天中的**可编辑文件**（Editable files）

**使用方式**：
```bash
aider --cache-prompts --read CONVENTIONS.md src/auth/login.ts
```

---

## 3. 模型分工（--weak-model / --editor-model）

### --weak-model

**官方说明**：用于生成 commit 消息和**总结聊天历史**。
- 处理对推理能力要求较低的辅助任务
- 默认值取决于所选的主模型

```bash
aider --model claude-sonnet-4-6 --weak-model claude-haiku-4-5
```

### --editor-model

**官方说明**：在 `architect` 模式中，负责将主模型的提议**翻译成具体的文件编辑指令**。
- 适用于主模型推理强但生成编辑格式（diff）能力弱的场景（如 o1）
- 在 architect 模式下：主模型负责逻辑，editor 模型负责实际代码改写

**三模型分工**：
| 角色 | 使用的模型 | 职责 |
|------|-----------|------|
| Architect（主模型）| `--model` | 理解需求，提出逻辑解决方案 |
| Editor（编辑模型）| `--editor-model` | 执行具体代码改写（architect 模式下） |
| Weak Model（弱模型）| `--weak-model` | commit 消息生成、聊天历史总结 |

---

## 4. CONVENTIONS.md 和 --read 的加载方式

**官方明确说明**：CONVENTIONS.md **不会被自动感知**，必须显式加载：

1. `/read CONVENTIONS.md`（交互式命令）
2. `aider --read CONVENTIONS.md`（启动参数）
3. `.aider.conf.yml` 中设置 `read: [CONVENTIONS.md]`（配置文件）

**--read 与 /read 的区别**：
- 功能相同：都将文件标记为只读（不被模型修改）
- 都支持缓存（启用 --cache-prompts 时）
- 区别仅在于**操作时机**：--read 是启动参数，/read 是交互式命令

---

## 5. 其他已确认的 Aider 参数

| 参数 | 说明 |
|------|------|
| `--map-tokens <N>` | 控制 repo map 的 token 预算（默认 1024） |
| `--cache-prompts` | 启用提示词缓存（需要支持缓存的 provider） |
| `--weak-model <model>` | commit 消息和历史总结使用的轻量模型 |
| `--editor-model <model>` | architect 模式下的代码编辑模型 |
| `--read <file>` / `/read` | 只读文件引用（不允许 AI 修改）|
| `/add <file>` | 可编辑文件（允许 AI 修改）|
| `/undo` | 撤销上一次 AI 的代码修改 |
| `/clear` | 清空聊天历史（保留文件范围）|

---

## 6. 不确认的内容

- repo map 的具体图排名算法（类似 PageRank，但官方未明确是否就是 PageRank）
- --cache-prompts 在不同 provider 的实际命中率数据 — 无公开基准数据
- --weak-model 和 --editor-model 分别节省多少成本 — 无公开量化数据
