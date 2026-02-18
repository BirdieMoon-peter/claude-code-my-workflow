---
name: pedagogy-review
description: 对讲座幻灯片进行全面的教学审查。检查叙事结构、学生前置知识、实例练习、符号清晰度和节奏。
disable-model-invocation: true
argument-hint: "[QMD 或 TEX 文件名]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Task"]
---

# 讲座幻灯片教学审查

执行全面的教学审查。

## 步骤

1. **识别文件** 在 `$ARGUMENTS` 中指定
   - 如果没有参数，询问用户要审查哪个讲座
   - 如果只是名称，在 `Quarto/` 或 `Slides/` 中查找

2. **启动教学审查员智能体** 使用完整的文件路径
   - 智能体检查 13 种教学模式
   - 执行整体分析（叙事结构、节奏、视觉节奏、符号）
   - 考虑学生视角（前置知识、异议）

3. **智能体生成报告** 保存到:
   `quality_reports/[FILENAME_WITHOUT_EXT]_pedagogy_report.md`

4. **向用户展示摘要:**
   - 遵循 vs 违反的模式（共 13 种）
   - 整体评估
   - 关键建议（前 3-5 项）

## 重要提示

- 这是**只读审查** — 不编辑文件
- 专注于**教学**而非视觉布局（使用 `/visual-audit` 进行视觉审核）
- 如需综合审查，使用 `/slide-excellence`
