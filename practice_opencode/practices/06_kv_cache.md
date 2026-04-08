# 06 稳定前缀：让规则慢变，让长历史可压缩

## 核心习惯

对 OpenCode 来说，最该稳定的是 `AGENTS.md` 和 `instructions` 这一层；最该及时处理的是不断增长的对话历史。

## 在 OpenCode 里怎么做

- 让 `AGENTS.md` 慢变。
- `instructions` 的文件集合和顺序尽量稳定。
- 会话过长时，用 `/compact`，别名 `/summarize`，不要硬撑。
- 任务级信息放计划文件或 handoff 文件，不要塞回规则层。

OpenCode 在这章的关键点有两个：

- TUI 有正式的 `/compact`
- config 里有 `compaction.auto`、`compaction.prune`、`compaction.reserved`

这意味着它不是完全靠用户自己“感觉该重开了”，而是有正式的压缩层可用。

## 为什么要强调稳定前缀

因为模型前缀越稳定：

- 规则更可预期
- 历史压缩后更容易回到同一套项目共识
- 你也更容易判断问题到底来自规则、历史，还是当前提示词

## 不要这样做

- 不要频繁重写 `AGENTS.md` 去承载短期任务。
- 不要让 `instructions` 今天一套、明天一套、顺序也乱变。
- 不要把 `/compact` 当成 handoff 文件的替代品。

## 验收标准

- 根规则明显慢于任务文件变化。
- 历史变长时，你有 `/compact` 或重开机制。
- 当前任务状态已经外化，而不是全挤在长历史里。

## 对应支撑

见 `../commands/06_compact_and_stable_prefix_commands.md`。
