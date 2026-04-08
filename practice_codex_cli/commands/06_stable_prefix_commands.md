# 06 稳定前缀与指令链

## 用户级规则文件

```text
~/.codex/AGENTS.md
```

适合放：

- 个人长期偏好
- 通用的审查要求
- 常用工作习惯

不适合放：

- 某个仓库的一次性任务
- 具体 feature 的实施计划

## 替换默认规则文件名

可以在配置里改项目规则文件名：

```toml
project_doc_fallback_filenames = ["TEAM_AGENTS.md", ".agents.md"]
```

这会改变“Codex 默认找什么文件名”，适合团队有统一命名约定时使用。

## 调整指令链大小上限

```toml
project_doc_max_bytes = 65536
```

官方默认总读取上限是 32 KiB。只有在你真的需要更多稳定规则时才调大。

## 使用自定义 CODEX_HOME

```bash
CODEX_HOME=$(pwd)/.codex codex exec "列出当前会加载的个人规则"
```

适合：为某个项目或实验环境隔离用户级配置。

## 稳定前缀的实践重点

- 让 `AGENTS.md` 慢变
- 让计划和 handoff 快变但单独落文件
- 不要把两者混在一起

## 这章最容易写错的地方

- 不要把任务状态长期写进 `AGENTS.md`。
- 不要频繁改规则文件然后期待上下文还能稳定。
- 不要把“有 resume”当成“就不需要外化任务状态”。
