# 第六章（深化）：KV Cache 经济学——让稳定上下文摊薄成本

> 本章为 round1 同章的深化版，假设读者已熟悉 round1 内容。
> 标注：`A/B` — 本机 CLI 或官方文档核验；`I` — 工程推论

---

## 习惯

**缓存的收益来自稳定的前缀。Aider 的 `--cache-prompts` 和 Anthropic 的提示词缓存机制，已经是生产级工具的正式特性，但它们缓存的内容和触发条件需要分别理解。**

---

## 正确的做法

### 深化：Aider --cache-prompts 的官方说明

Aider 提供了正式的 `--cache-prompts` 参数（`A/B`：Aider 官方文档）：

```bash
aider --cache-prompts \
      --read CONVENTIONS.md \
      --read docs/architecture.md \
      src/auth/login.ts
```

**缓存范围**（`A/B`，官方明确列出）：
1. 系统提示词（System prompt）
2. 通过 `--read` 或 `/read` 添加的只读文件
3. 仓库地图（Repo map）
4. 已添加到聊天中的可编辑文件（Editable files）

**适用的 provider**（`A/B`）：Anthropic、DeepSeek（这两家支持提示词缓存）

**实际收益**（`I`）：
- 如果你每天有 20+ 次 Aider 会话，每次都加载相同的 `CONVENTIONS.md`，缓存能把这个文件的重复加载成本降到接近零
- 对于大型 repo map（`--map-tokens` 设置较高时），缓存的收益更明显

### 深化：Claude Code 的规则层缓存行为

Claude Code 在使用 `/compact` 后的行为（`A/B`：官方文档）：

**压缩后发生什么**：
- 历史对话被压缩为摘要
- `CLAUDE.md` 等规则文件**从磁盘重新读取**并重新注入

这意味着：规则文件的内容是"缓存层"，而不是"会话层"。规则在 /compact 后不会丢失，但也不会带着历史一起传承。

**实践影响**（`I`）：
- 你可以随时 /compact，不用担心规则层的内容丢失
- /compact 后的会话，规则层是全新注入的，相当于"规则生效，历史清空"
- 这使得 /compact 成为一个"恢复规则、清除历史"的组合操作

### 深化：Codex CLI 的缓存和 AGENTS.md 的关系

Codex CLI 的规则载体是 `AGENTS.md`（`A/B`）。与 Claude Code 的规则加载类似，Codex CLI 在启动时加载 `AGENTS.md`（或 `--no-project-doc` 禁止加载）。

对于缓存，Codex CLI 底层调用的是 OpenAI API（`A/B`）。OpenAI 是否支持提示词缓存，以及 Codex CLI 是否利用了这个特性，**目前无公开文档明确说明**（`无法确认`）。

安全的实践（`I`）：保持 `AGENTS.md` 内容稳定，即使底层缓存机制不确定，稳定的前缀也能减少不必要的上下文重建。

### 深化：缓存和会话持久化的区别

round1 已经指出这两者的区别，但值得再次明确（`I`）：

| 机制 | 解决的问题 | 工具层面 |
|------|-----------|---------|
| 提示词缓存 | 相同前缀不需要重复计算 | Anthropic API / Aider `--cache-prompts` |
| 会话持久化 | 对话历史可以恢复 | Claude Code session / Codex CLI session / Aider `.aider.chat.history.md` |

**两者可以同时存在**：一个被 resume 的会话里，如果系统提示词部分和上次一致，会命中提示词缓存；历史对话部分是从磁盘读取的，不在缓存里（因为每次都可能不同）。

---

## 错误的可能

### 新增错误：以为 /compact 会丢失规则

因为 `/compact` 会清除历史，有些人担心规则也会丢失。但实际上（`A/B`）：
- /compact 后，Claude Code 从磁盘重新读取并注入 CLAUDE.md
- 规则层在 /compact 后依然完整

### 新增错误：以为 --cache-prompts 对所有 Aider provider 有效

`--cache-prompts` 目前只对支持提示词缓存的 provider 有效（`A/B`：Aider 官方）：Anthropic、DeepSeek。如果你在 Aider 里使用 OpenAI 或其他 provider，`--cache-prompts` 实际上不会产生缓存效果（参数不会报错，但也不会命中缓存）。

---

## 背后的原理

### 深化：Aider repo map 的缓存价值

Aider 的 repo map 是上下文的一大组成部分（`A/B`：Aider 官方文档）。在 `--cache-prompts` 模式下，repo map 也会被缓存：

- 默认 repo map 大小：1024 tokens（`--map-tokens` 的默认值）
- 大型项目可能需要更大的 repo map（比如 4096 tokens）
- 如果不缓存，每次对话都重新计算 4096 tokens 的 repo map
- 如果缓存，后续对话里这部分几乎免费（命中缓存后成本大幅降低）

对于大型项目的 Aider 用户，`--cache-prompts` 的收益主要来自 repo map 的缓存，而不只是约定文件的缓存（`I`）。

### 深化："稳定前缀"的工程含义

"稳定前缀"不只是"内容不变"，还包括"顺序不变"（`I`）：

大多数 LLM 的提示词缓存是基于 token 序列的精确匹配——如果你把两个工具调用的顺序调换了，缓存会失效。

实践上，这意味着：
- 规则文件的内容不变（显然）
- 规则文件在上下文里的位置不变（重要但容易忽视）
- 在 Aider 里，`--read` 文件的加载顺序应该保持一致

```bash
# 每次都用同样的顺序（利于缓存）
aider --cache-prompts \
      --read CONVENTIONS.md \
      --read docs/architecture.md \
      src/auth/login.ts

# 不要每次改变顺序（破坏缓存）
aider --cache-prompts \
      --read docs/architecture.md \
      --read CONVENTIONS.md \   # 顺序变了，缓存失效
      src/auth/login.ts
```

---

## 延伸的习惯

### 深化：用 .aider.conf.yml 固化缓存配置

```yaml
# .aider.conf.yml
cache-prompts: true
read:
  - CONVENTIONS.md
  - docs/architecture.md
map-tokens: 2048   # 根据项目大小调整
```

这样每次启动 Aider 都自动使用相同的配置（同样的顺序，同样的文件），最大化缓存命中率。

### 深化：长会话里的缓存衰减

随着会话变长，对话历史的占比越来越大，而可缓存的稳定前缀（规则文件 + repo map）占整体 token 的比例越来越小（`I`）：

```
第 1 轮：规则 2000 + 任务 500 = 总 2500。缓存比例 80%
第 10 轮：规则 2000 + 历史 8000 + 任务 500 = 总 10500。缓存比例 19%
第 30 轮：规则 2000 + 历史 25000 + 任务 500 = 总 27500。缓存比例 7%
```

这是另一个"长会话应该及时压缩或重置"的经济学理由——不只是 context rot 的问题，也是缓存收益递减的问题。

---

## 个人 / 协作注意

**个人使用**：如果你频繁使用 Aider，`.aider.conf.yml` 里固定 `cache-prompts: true` 和 `read:` 列表，是最省心的做法。

**团队协作**：
- 把 `.aider.conf.yml` 纳入版本控制（如果团队都用 Aider）
- 确保 `.aider.conf.yml` 里的 `read:` 文件列表和顺序是团队统一的，避免不同成员的缓存行为不一致

---

## 延伸调查方向

1. **Aider --cache-prompts 的实际延迟和成本改善**：官方说明了缓存范围，但没有给出量化数据。这取决于 repo map 大小和约定文件大小，实测数据会因项目而异。

2. **Cursor 的缓存透明度**：Cursor 内部是否使用了提示词缓存？用户有没有办法观察到缓存命中情况？目前无公开说明。

3. **Codex CLI 的前缀复用边界**：resume、fork、exec 三种工作流里，AGENTS.md 是否会被缓存？缓存是否跨 session 持续？目前无公开文档说明。

4. **/compact 后的缓存重建成本**：/compact 后规则从磁盘重新注入，这次重新注入是否会重建缓存？还是每次 /compact 都需要付一次"缓存写入"的成本？目前无公开数据。
