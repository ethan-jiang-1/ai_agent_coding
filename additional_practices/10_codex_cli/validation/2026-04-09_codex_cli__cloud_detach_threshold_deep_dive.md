# 2026-04-09 codex_cli__cloud_detach_threshold deep dive

Run ID: `2026-04-09_additional_practices_r1`
Tool Candidate: `codex_cli__cloud_detach_threshold`

## 为什么深挖这个点

这个候选有一个明确的文档漂移风险：

- `Codex web` 概览页仍有旧表述
- 但 CLI features / CLI reference 已把 `codex cloud` 写成正式终端入口

如果这里不做深挖，后面很容易把旧描述误当成当前事实。

## 深挖后的判断

当前应以 CLI 专页和 CLI release / reference 为准：

- 对 Codex CLI 来说，`codex cloud` 已经是正式命令面
- 这足以支持“何时从本地 loop 切到 detached task”的行为判断

## 这次深挖没有改变什么

- 没有把它升级成独立新 practice
- 没有把它写成“所有长任务都该上 cloud”

## 这次深挖改变了什么

- 把这个候选从“也许只是产品宣传信号”升级成“有正式 CLI 事实面支撑的 merge 候选”
