# 09 会话续跑与 handoff

## 继续交互式会话

```bash
codex resume --last
codex resume <session-id>
```

## 把非交互历史带回 TUI

```bash
codex resume --include-non-interactive --last
```

适合：前面是 `exec` 跑的，现在想人工接着看。

## 继续非交互执行链

```bash
codex exec resume --last "继续下一步"
codex exec -s workspace-write resume --last "按刚才的分析实施改动"
```

## 从交互式历史分叉

```bash
codex fork --last "尝试另一种实现路径"
codex fork <session-id> "保留历史，但从这里分叉"
```

当前帮助里，`fork` 没有 `--include-non-interactive`。实践上，把它当成交互式会话分叉更稳。

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

- 不要只靠 `resume`，不写 handoff。
- 不要把 `fork` 当成跨人交接的主要手段。
- 不要让下一步依赖“AI 自己记得刚才看过什么”。
