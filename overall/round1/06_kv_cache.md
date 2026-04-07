# 第六章：KV Cache 经济学——让稳定上下文摊薄成本

## 习惯

**把稳定、长期有效的规则和参考材料放在固定入口与固定顺序里；不要把“今天的任务”写进规则层。缓存收益来自稳定前缀，不来自把一切都塞进上下文。**

---

## 正确的做法

### 先分清两件事：缓存不是会话持久化

这章最容易写混的地方是把两件不同的机制当成一件事：
- **提示词缓存 / 前缀复用**：相同前缀不必每次重新计算
- **会话持久化**：工具允许你继续、恢复、分叉之前的会话

二者可能同时存在，但不是同一个东西。`resume`、`fork`、`continue` 解决的是会话连续性，不等于“缓存一定命中”；同理，某段规则能被缓存，也不等于工具就有可恢复 session。

### 能稳定成立的缓存条件

不论底层 provider 是哪家，通用规律都差不多：
1. 前缀内容必须稳定
2. 顺序必须稳定
3. 动态信息不能混进稳定层
4. 具体阈值、费率和命中细节要以当期 provider 文档为准

最值得长期执行的动作不是背数字，而是维护“稳定前缀纪律”。

### 各工具里能安全写成事实的做法

**Claude Code**
```bash
# CLAUDE.md、.claude/rules/ 和 auto memory 构成稳定指令层
# /compact 后，规则会从磁盘重新读取
claude "先按 CLAUDE.md 约束分析 src/auth/，不要直接改代码"
```

对 Claude Code，最有效的缓存纪律通常不是“多写一点规则”，而是：
- 保持 `CLAUDE.md` 稳定
- 把当前任务写进当次 prompt 或计划文件
- 在长会话里及时 `/compact`，避免历史越滚越长

**Aider**
```bash
# 稳定参考材料放 --read
aider --read CONVENTIONS.md --read docs/architecture.md src/auth/login.ts

# Aider 还有正式的 prompt caching 开关
aider --cache-prompts --read CONVENTIONS.md src/auth/login.ts
```

这轮能确认的是：
- `--read` 是正式只读入口
- `repo map` 是正式上下文压缩机制
- `--cache-prompts` 是正式参数

不要把 Aider 写成“自动构建 AST 缓存图谱”。它的正式说法是 `repo map`。

**Codex CLI**
```bash
# 正式规则入口是 AGENTS.md，不是 codex.md
codex "先按 AGENTS.md 约束阅读 src/auth/login.ts，再给修改方案"

# 非交互式只读分析
codex exec --sandbox read-only "先读取 docs/auth-flow.md，只输出实施计划"
```

对 Codex CLI，这轮最重要的校正有两个：
- 规则入口是 `AGENTS.md`
- 会话并不天然是“一次一条、完全无状态”，因为官方有 `resume` 和 `fork`

因此，正确说法是：稳定的 `AGENTS.md` 和稳定的项目级配置，有助于形成可复用的指令前缀；不要把它写成“每条命令都天然隔离，所以只看单次命令缓存”。

**Cursor**
- 当前主规则体系是 `.cursor/rules/*.mdc`，`.cursorrules` 是 legacy
- `AGENTS.md` 也被官方支持
- 对用户来说，更稳的实践是保持 always-apply 规则简短、稳定、少变

这轮不安全的写法是：把 Cursor 的内部索引、RAG 或缓存细节写成用户可精确控制的 KV cache 旋钮。官方能确认的是 rules/context 体系，不是一个明确暴露给用户的“缓存控制面板”。

### 结构化模板：稳定层在前，任务层在后

```text
[稳定层]
- CLAUDE.md / AGENTS.md / .cursor/rules/*.mdc
- docs/architecture.md
- 代码规范与边界

[任务层]
- 当前要改什么
- 成功标准
- 验证方式
```

如果你把“当前日期、今天要做的事、最近改了哪些文件”写进稳定层，就等于主动破坏前缀复用。

---

## 错误的可能

### 错误一：在规则文件里混入动态信息

```markdown
# 错误的 CLAUDE.md / AGENTS.md
## 项目规则
- 使用 TypeScript
- 当前日期：2026-04-07
- 今天优先修复：src/auth/login.ts

# 更好的写法
## 项目规则
- 使用 TypeScript
- 包管理器：pnpm
- 测试框架：Vitest
```

动态信息应该放到任务 prompt、计划文件或 issue 描述里，而不是规则层。

### 错误二：继续使用错误的规则载体

以下写法都应该从 `round1` 里清掉：
- 把 Codex CLI 的规则文件写成 `codex.md`
- 把 Cursor 的主规则入口写成 `.cursorrules`

这不仅是术语错误，也会让读者把“哪里该稳定”这件事做错。

### 错误三：把缓存当成“规则越大越好”的理由

缓存能摊薄重复前缀成本，但不能替你解决注意力稀释。一个巨大、低相关、很少真正被用到的规则包，即使部分可复用，也仍然会拖慢理解、污染上下文。

更稳的原则是：
- 稳定
- 精简
- 相关

三者缺一不可。

### 错误四：会话越长越舍不得压缩

会话一长，后面追加的历史会越来越多。即使前缀规则保持不变，真正稳定前缀在整段上下文里的占比也会下降。对 Claude Code 来说，这正是 `/compact` 有价值的原因；对其他工具来说，这也是为什么要及时开新会话、落盘计划和利用文件边界。

---

## 背后的原理

### 缓存本质上是“前缀复用”

从工程角度看，缓存的价值在于重复前缀不必每次都重新算一遍。对 AI coding 工作流来说，最适合复用的东西通常是：
- 项目级规则
- 架构约束
- 长期稳定的参考文档

最不适合复用的东西通常是：
- 今天的任务
- 当前分支的临时状态
- 最近一轮的探索性推理

### 缓存节省的是重复成本，不是总成本

这点很重要。缓存可以降低“重复前缀”的边际代价，但不能让你忽视两件事：
- 无关内容仍然会占注意力
- 每轮新增任务内容仍然要重新处理

因此，缓存纪律的目标不是“把上下文做大”，而是“把稳定层做对”。

### 规则层和任务层应该分治

一个成熟团队的写法通常是：
- 共享规则放 `CLAUDE.md`、`AGENTS.md`、`.cursor/rules/*.mdc`
- 任务计划放独立文件
- 当前问题放当次 prompt

这样既利于复用，也利于审查和更新。

---

## 延伸的习惯

### 让规则层慢变，让计划层快变

可以这样分层：

```text
慢变：
- CLAUDE.md
- AGENTS.md
- .cursor/rules/*.mdc
- docs/architecture.md

快变：
- plans/feature-x.md
- 当前 issue 描述
- 当次调试日志
```

### 在 Aider 里把参考材料和可编辑文件分开

```bash
aider \
  --cache-prompts \
  --read CONVENTIONS.md \
  --read docs/architecture.md \
  src/auth/login.ts
```

这样既保留稳定参考，也不至于让可编辑范围失控。

### 在 Claude Code 里用 `/compact` 恢复会话质量

`/compact` 不等于撤销，也不等于 magic reset。它更接近“清理长会话历史负担，同时重新从磁盘注入规则层”。

---

## 个人 / 协作注意

**个人使用**：每次想改 `CLAUDE.md`、`AGENTS.md` 或 always-apply `.mdc` 时，先问自己一句：这真是长期规则，还是只是今天的任务？

**团队协作**：
- 共享规则文件的修改，应该像改基础设施配置一样谨慎
- 任务说明应落到计划文件，不要天天改共享规则
- 规则层的 review 重点不是“写得多不多”，而是“长期是否稳定、是否真有必要”

---

## 延伸调查方向

1. **真实工作流里的缓存命中率**：`CLAUDE.md`、`AGENTS.md`、`.cursor/rules/*.mdc` 在长期团队使用中，命中率分别有多高？
2. **Cursor 的缓存透明度**：Cursor 官方未来是否会更明确暴露内部上下文复用或缓存信息？
3. **Codex CLI 的前缀复用边界**：在 `resume`、`fork`、`exec` 三类工作流里，稳定规则层对成本和延迟的影响分别多大？
4. **Aider 的 `--cache-prompts` 实际收益**：在大仓库 + 长期 `--read` 参考材料的场景里，延迟和成本改善有多明显？
