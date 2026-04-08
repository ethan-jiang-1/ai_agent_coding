# 09 session、share、export 与 handoff

## 列出历史会话

```bash
opencode session list
opencode session list --format json
```

适合：先看有哪些会话，再决定恢复、分叉或导出。

## 导出与导入

```bash
opencode export <session-id>
opencode import session.json
opencode import https://opncd.ai/s/abc123
```

适合：

- 跨机器搬运状态
- 给自己留档
- 从分享链接重新导入会话

## TUI 里的相关 slash commands

- `/sessions`：切换历史会话
- `/export`：导出当前对话到 Markdown
- `/share`：分享当前会话
- `/unshare`：取消分享并删除分享数据

这几组关系不要混：

- `/export` 是 Markdown 导出
- `opencode export` 是 JSON 导出
- `/share` 是当前 TUI 会话的分享动作
- `opencode run --share` 是 run 场景的分享入口

## share 配置

```json
{
  "$schema": "https://opencode.ai/config.json",
  "share": "manual"
}
```

官方 docs 当前给出的值：

- `"manual"`：默认，手动 `/share`
- `"auto"`：新会话自动分享
- `"disabled"`：彻底关闭分享

## share 的安全边界

官方 docs 当前明确写到：

- 分享会生成公开链接
- 任何拿到链接的人都可以访问
- 共享数据会保留到你显式 `unshare`

所以：

- 内部敏感项目默认不要开 `auto`
- 不要把 share link 当成私有交接渠道

## handoff 文件模板

```text
# 当前状态
- 已完成：
- 未完成：

# 已确认约束
- ...

# 下一步
1. ...
2. ...

# 风险
- ...
```

## 这章最容易写错的地方

- 不要把 session ID 当成跨人 handoff 方案。
- 不要把公开 share 当成内部安全协作。
- 不要让“下一步做什么”只留在会话历史里。
