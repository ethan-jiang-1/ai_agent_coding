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

## server / web / attach：同一状态，不同客户端

```bash
opencode serve
opencode web --port 4096
opencode attach http://localhost:4096
opencode run --attach http://localhost:4096 "继续已有 backend 的工作"
```

官方 docs 当前明确写到：

- 运行 `opencode` 时本身就会启动 TUI 和 server
- `serve` 可启动 standalone server
- `web` 可提供浏览器界面
- `attach` 和 `run --attach` 可接入已有 backend
- web 与 terminal 可共享同一套 sessions 和 state

这组能力更适合：

- 同一状态跨客户端继续
- 降低重复冷启动
- 在 terminal 与 web 之间来回切换

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
- `serve` / `web` / `attach` 是共享同一 backend 状态

更实用的分工是：

- 人类 handoff / 复盘：优先 `/export`
- 会话搬运 / 备份 / 重新导入：优先 `opencode export` / `import`

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
- 不要把 `serve` / `web` / `attach` 写成 scheduler 或后台 worker。
