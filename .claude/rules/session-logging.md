# 会话日志

**位置：** `quality_reports/session_logs/YYYY-MM-DD_description.md`
**模板：** `templates/session-log.md`

## 三个触发器（全部主动）

### 1. 计划后日志

计划批准后，立即捕获：目标、方法、理由、关键背景。

### 2. 增量日志

每当发生以下情况时追加 1-3 行：做出设计决策、解决问题、用户纠正内容或方法改变。不要批处理。

### 3. 会话结束日志

结束时：高层摘要、质量评分、未解决的问题、阻滞。

## 质量报告

**仅在合并时**生成 -- 不在每次提交或 PR 时生成。
保存到 `quality_reports/merges/YYYY-MM-DD_[branch-name].md`，使用 `templates/quality-report.md`。
