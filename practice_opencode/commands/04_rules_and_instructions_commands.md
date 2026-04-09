# 04 规则层与 instructions

## 项目级规则

```text
<project-root>/AGENTS.md
```

这是 OpenCode 当前首选的项目规则载体。

## 全局规则

```text
~/.config/opencode/AGENTS.md
```

适合放个人长期偏好，不要混进项目仓库。

## 兼容 fallback

OpenCode 当前官方说明：

- 如果没有项目级 `AGENTS.md`，会回退到 `CLAUDE.md`
- 如果没有全局 `~/.config/opencode/AGENTS.md`，会回退到 `~/.claude/CLAUDE.md`

所以如果项目里历史上遗留了 `CLAUDE.md`，要清楚它是否还在被加载。

## 规则优先级

官方 docs 当前可确认：

1. 从当前目录向上遍历本地 `AGENTS.md` / `CLAUDE.md`
2. `~/.config/opencode/AGENTS.md`
3. `~/.claude/CLAUDE.md`

并且：

- 同一类别里 first matching file wins
- 如果同层同时有 `AGENTS.md` 和 `CLAUDE.md`，只用 `AGENTS.md`

## 初始化规则入口

```text
/init
```

适合：让 OpenCode 帮你生成或改进 `AGENTS.md` 草稿。

官方 rules docs 当前还明确写到：

- `/init` 会扫描仓库里重要文件
- 当代码库本身回答不了时，它可能会追问几个定向问题
- 它会就地改进已有 `AGENTS.md`，而不是盲目覆盖
- 它重点提炼的是 future agent sessions 最常需要的信息
- 官方建议把项目级 `AGENTS.md` 提交到 Git

## 用 `instructions` 补充额外规则

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "docs/engineering-guidelines.md",
    "docs/testing/*.md"
  ]
}
```

官方 docs 当前说明 `instructions` 支持：

- 文件路径
- glob
- 远程 URL

## 一个容易忽略的关键边界

官方 rules docs 当前明确写到：

- OpenCode 不会自动解析 `AGENTS.md` 里的外部文件引用

所以如果你的规则体系里存在：

- 团队共享规范
- 外部 style guide
- 目录级补充规则

更稳的做法通常是：

- 稳定规则材料放 `instructions`
- `AGENTS.md` 里只保留最关键、最稳定的入口与加载策略

## 当前交互命令面的真实边界

OpenCode 这次和规则层直接相关的主要是一条 built-in command：`/init`。

也就是说：

- 它没有一个像 `/rules` 这样的完整会话内规则管理面
- 更细的规则分层，仍然主要靠 `AGENTS.md`、`instructions` 和文件本身

## `/init` 对规则优先级的实际影响

如果一个项目此前只靠 `CLAUDE.md` fallback 生效，那么运行 `/init` 生成项目级 `AGENTS.md` 后，后续 session 的项目规则主入口就变了：

- 之前：主要靠 `CLAUDE.md`
- 之后：优先读取 `AGENTS.md`

这不是“多了一个文件”这么简单，而是整个项目级规则入口被正式切换了。

## 这章最容易写错的地方

- 不要把任务计划写进 `AGENTS.md`。
- 不要忘了 `CLAUDE.md` fallback 的存在。
- 不要把所有规则全文堆进根规则，而不利用 `instructions` 分层。
