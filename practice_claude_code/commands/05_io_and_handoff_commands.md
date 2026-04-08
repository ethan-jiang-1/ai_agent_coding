# 05 输入输出与 handoff

## 管道输入到交互式回答

```bash
cat error.log | claude "分析根因"
```

## 非交互式打印输出

```bash
cat prompt.txt | claude --print
cat error.log | claude --print "找出根因"
```

## 流式输入格式

```bash
cat stream-input.json | claude --print --input-format stream-json
```

## 下载 file resources

```bash
claude --file file_abc:doc.txt --file file_def:image.png "分析这些材料"
```

## 会话内对应 slash commands

- `Type A` `/add-dir <path>`：给当前会话追加可访问目录，对应 `--add-dir`。
- `Type C` `/export [filename]`：导出当前会话，适合 handoff、留痕和复盘。
- `Type D` `/copy [N]`：复制最近消息，适合摘取短结论或命令片段。

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

- 不要把关键中间结论只留在会话历史里。
- 不要把大段日志手工复制成残缺片段。
- 不要把“能 resume”理解成“不需要 handoff 文件”。
