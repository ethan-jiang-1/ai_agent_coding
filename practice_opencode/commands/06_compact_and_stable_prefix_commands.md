# 06 压缩历史与稳定前缀

## 手动压缩

```text
/compact
```

官方 TUI 文档当前说明：

- `/compact` 会压缩当前 session
- 别名是 `/summarize`

适合：历史已经明显变长，但任务边界还没变，不想直接重开。

## compaction 配置

```json
{
  "$schema": "https://opencode.ai/config.json",
  "compaction": {
    "auto": true,
    "prune": true,
    "reserved": 10000
  }
}
```

官方 docs 当前确认：

- `auto`：达到阈值时自动 compact
- `prune`：compact 后从上下文里移除旧消息
- `reserved`：给下次请求预留 token

## 稳定前缀的实践重点

- 让 `AGENTS.md` 慢变
- 让 `instructions` 的内容和顺序稳定
- 让计划文件和 handoff 文件承担快变状态
- 历史变长时及时 `/compact`

## provider 级缓存辅助

官方 config docs 当前还给了一个更进阶的 knob：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "anthropic": {
      "options": {
        "setCacheKey": true
      }
    }
  }
}
```

`setCacheKey` 的官方说明是：为指定 provider 总是设置 cache key。

这不是“自动省钱按钮”，但对支持前缀缓存的 provider 来说，是一个值得保留的稳定前缀辅助项。

## 这章最容易写错的地方

- 不要把 `/compact` 当成 handoff 文件的替代品。
- 不要频繁改 `AGENTS.md` 去承载临时任务。
- 不要让 `instructions` 的顺序反复变化。
