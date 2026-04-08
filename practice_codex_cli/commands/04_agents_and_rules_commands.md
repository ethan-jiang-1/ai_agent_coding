# 04 AGENTS.md 与规则发现

## 最常见的目录布局

```text
repo/
  AGENTS.md
  services/
    payments/
      AGENTS.md
      AGENTS.override.md
```

含义：

- 根目录 `AGENTS.md`：项目级规则
- 子目录 `AGENTS.md`：该子树追加或细化规则
- `AGENTS.override.md`：直接替代同目录的 `AGENTS.md`

## 当前官方文档的发现顺序

对当前工作目录，Codex 会优先看离你最近的规则文件：

1. 当前目录的 `AGENTS.override.md`
2. 当前目录的 `AGENTS.md`
3. 更近父目录的 `AGENTS.override.md`
4. 更近父目录的 `AGENTS.md`
5. 直到仓库根
6. 用户级 `~/.codex/AGENTS.md`

同一目录里，`AGENTS.override.md` 会盖掉 `AGENTS.md`。

## 快速验证当前指令链

```bash
codex exec --ephemeral "Summarize the current instructions."
codex exec --ephemeral -C services/payments "Show which instruction files are active."
```

适合：确认当前目录下到底吃到了哪些规则文件。

## 交互式对应

```text
/init
/memory
/config
/status
```

映射关系：

- `/init` ↔ Approximate mapping
  适合初始化当前仓库的协作说明，但不等同于外部规则发现命令
- `/memory` ↔ Approximate mapping
  适合管理会话或项目记忆，不等同于 `AGENTS.md`
- `/config` ↔ Combined mapping
  适合查看或修改当前配置层
- `/status` ↔ Combined mapping
  适合检查当前会话吃到的模型、沙箱和配置状态

这章里，slash commands 更像“交互态下的规则与配置观察入口”，不是严格的一一命令替身。

## 规则内容该放什么

- 长期稳定的构建和测试要求
- 目录级特殊约束
- 不该碰的文件或目录

不适合放：

- 一次性任务计划
- 临时调试过程
- 会频繁改写的大段说明

## 这章最容易写错的地方

- 不要写成 `codex.md`。
- 不要以为 `CLAUDE.md` 会自动被 Codex CLI 读取。
- 不要把所有规则都堆进仓库根的一个长文档。
- 不要把 `/memory` 当成 `AGENTS.md` 的替代品。
