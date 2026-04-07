# 第十一章（深化）：订阅额度陷阱——先分清你买的到底是什么

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**Claude Code 提供了工具层面的成本控制参数（`--max-budget-usd`、`--permission-mode plan`），这是比"靠自律少用"更可靠的成本控制机制。**

---

## 正确的做法

### 深化：Claude Code 的成本控制工具

round1 提到了这些参数，round2 补充具体用法（`A/B`：claude --help 核验）：

**--max-budget-usd**（仅在 --print 模式有效）：

```bash
# 设置单次非交互式调用的最大成本
claude --print \
       --max-budget-usd 2 \
       --model sonnet \
       "总结这批日志，只输出根因"

# 达到上限时，Claude Code 停止执行并退出
# 适合：批处理脚本、CI/CD 管道里的成本控制
```

**--permission-mode plan 的成本含义**（`A/B + I`）：

plan mode 不只是"规划阶段的工具保障"，也有成本含义：
- plan mode 里 AI 只读取文件，不执行代码、不修改文件
- 读取操作通常比修改操作消耗更少的 token（短上下文，短输出）
- 先用 plan mode 确认方向，再切换到 default mode 执行，可以避免在错误方向上消耗昂贵的执行 token

```bash
# 成本控制工作流
claude --permission-mode plan --print \
  "分析 src/auth/ 并给出 OAuth2 迁移计划" \
  > /tmp/plan.md  # 快速、便宜的规划

cat /tmp/plan.md  # 人工审查

# 只有确认方向正确后，才进入执行阶段
claude "按 /tmp/plan.md 的计划实现..."  # 昂贵但有价值的执行
```

### 深化：Aider 的成本控制手段

Aider 的成本控制比订阅制工具更透明，因为直接对接 API（`A/B`）：

```bash
# 三模型分工（最有效的成本控制）
aider \
  --model claude-sonnet-4-6 \      # 主推理（贵，用于理解）
  --editor-model claude-haiku-4-5 \ # 代码改写（便宜）
  --weak-model claude-haiku-4-5 \   # commit + 摘要（很便宜）
  --cache-prompts \                  # 缓存稳定前缀
  src/auth/

# 对于简单的格式化任务，降级主模型
aider --model claude-haiku-4-5 \
      --weak-model claude-haiku-4-5 \
      src/utils/  # 格式化 / 重命名 / 简单修改
```

`--map-tokens` 也影响成本（`A/B + I`）：
- repo map 越大，每次对话消耗的 token 越多
- 对小项目，1024 tokens 的默认值通常已经足够
- 不需要把 repo map 调大来"帮 AI 看更多代码"——这会增加每次对话的成本

### 深化：Codex CLI 的成本边界

Codex CLI 是 BYOK（自带 API Key）模式，成本直接取决于 OpenAI API 的用量（`A/B + I`）：

```bash
# 控制成本的核心：选择合适的入口
codex exec --ephemeral -s read-only "..."  # 只读分析，成本低
codex exec -s workspace-write "..."        # 执行修改，成本中等
codex --dangerously-bypass-approvals-and-sandbox "..."  # 极危险，不推荐
```

自动化程度和成本的关系（`I`）：
- `exec --ephemeral`（一次性，非交互式）：成本最可预测，适合批处理
- 交互式 session：成本取决于会话长度，容易超出预期
- `--full-auto`（workspace-write + on-request）：模型自主决定是否请示，可能产生多轮工具调用

**成本控制建议**（`I`）：
- 对非交互式任务，`codex exec --ephemeral` 配合明确的任务边界，成本最可控
- 对交互式任务，注意 session 的长度，适时重置

---

## 错误的可能

### 新增错误：把 --max-budget-usd 用在交互式模式

`--max-budget-usd` 仅在 `--print` 模式下有效（`A/B`：claude --help 核验）。在交互式会话里设置这个参数，不会产生任何效果。

```bash
# 无效（交互式模式不支持 --max-budget-usd）
claude --max-budget-usd 5 "帮我实现..."

# 有效（--print 模式）
claude --print --max-budget-usd 5 "帮我实现..."
```

### 深化：更强自治 ≠ 更省钱

round1 提到了这个观点，round2 从工具层面解释原因（`I`）：

"更强自治"意味着：
1. 更多的工具调用（每次工具调用都消耗 token）
2. 更多的自我验证循环（AI 执行 → 检查结果 → 修正 → 再执行）
3. 可能的错误方向（AI 在没有人工确认的情况下做出了错误决策，然后花大量 token 执行错误的方向）

实际上，**保留适当的人工审批点**（`-a on-request`）往往比`-a never`（全自动）更省钱——因为它防止了 AI 在错误方向上无限制地消耗 token。

---

## 背后的原理

### 深化：三种成本结构的工具级差异

round1 提出了三种成本结构（订阅/额度型、BYOK、混合），round2 从工具操作层面补充（`I`）：

**订阅/额度型（Cursor 等）**：
- 成本控制手段主要是"选择合适的工作模式"（Ask vs Agent，默认模型 vs 旗舰模型）
- 超额后通常是限速而不是拒绝，成本的边界感比 BYOK 弱

**BYOK（Aider、Codex CLI）**：
- 成本控制手段更精细（可以指定模型、控制 token 上限、分层路由）
- 超额后是直接的财务成本，边界感更强
- 但成本预测需要更多主动管理

**混合型（Claude Code）**：
- 既有 API key 直连（BYOK 成本结构）
- 也有订阅内的免费额度
- `--max-budget-usd` 是 API key 模式下的成本控制工具

### 深化：plan mode 降低"规划成本"的原理

用 plan mode 规划比直接执行更省钱，原因（`I`）：

1. plan mode 只读取文件（Input token）+ 输出文字计划（Output token）
2. 实际执行会额外产生：工具调用（多次 API 往返）+ 代码修改（更长的 Output token）

规划阶段失败（AI 给出错误方向的计划）的成本 << 执行阶段失败（AI 朝错误方向修改了 10 个文件）的成本。

这是 plan mode 的核心经济学价值：用低成本的规划过滤掉高成本的错误执行。

---

## 延伸的习惯

### 深化：用 --print 模式做成本可控的批处理

对于不需要交互的任务（日志分析、代码摘要、格式检查），--print 模式是最省钱的选择（`A/B + I`）：

```bash
# 批处理：分析多个日志文件，每个设置成本上限
for log_file in logs/*.log; do
  claude --print \
         --max-budget-usd 0.5 \
         --model haiku \
         "分析这个日志，只输出 ERROR 级别的根因" \
         < "$log_file"
done

# 每次调用独立，成本可预测，总成本 = N × 每次上限
```

---

## 个人 / 协作注意

**个人使用**：
- 如果你用 Claude Code 做批处理或 CI 集成，`--max-budget-usd` 是防止意外超支的护栏
- 如果你用 Aider，`.aider.conf.yml` 里的三模型分工配置是一次性投入，长期收益

**团队协作**：
- 在 CI/CD 管道里使用 Claude Code 时，`--max-budget-usd` 应该是标配，防止 CI 任务因为意外的长会话产生大额账单
- 团队共享的 `.aider.conf.yml` 里明确三模型分工，避免不同成员使用不同模型配置导致成本不一致

---

## 延伸调查方向

1. **Cursor 高阶模式的真实消耗曲线**：Background Agents vs 前台 Agent vs Ask 模式，在真实团队里的额度消耗速度差异，目前无公开量化数据。

2. **Claude Code 预算护栏的实践**：`--max-budget-usd` 在自动化脚本里的最优设置方式——太低会频繁中断，太高等于没设。有没有成熟的动态设置策略？目前无公开最佳实践。

3. **Aider 三模型分工的实际节省比例**：把 `--editor-model` 从 Sonnet 降到 Haiku，在不同类型任务上分别节省多少成本？对代码质量有多大影响？目前无公开数据。

4. **Codex CLI 本地/云端切换的实际边界**：哪些任务真的可以用本地 OSS 模型处理？延迟、准确率和成本的三角权衡，没有系统性评测数据。
