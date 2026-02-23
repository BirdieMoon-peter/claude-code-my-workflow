---
paths:
  - "Papers/**/*.tex"
  - "Slides/**/*.tex"
  - "scripts/**/*.py"
---

# 质量门槛和评分规则

## 阈值

- **80/100 = Commit** -- 足够好，可以保存
- **90/100 = PR** -- 准备好发布
- **95/100 = Excellence** -- 理想状态

## LaTeX 论文 (Papers/*.tex)

| 严重程度 | 问题 | 扣分 |
|----------|-------|-----------|
| Critical | XeLaTeX compilation 失败 | -100 |
| Critical | 未定义的引用 | -15 |
| Critical | 方程中的拼写错误 | -10 |
| Critical | 识别假设缺失 | -15 |
| Major | Overfull hbox > 10pt | -5 |
| Major | 记号不一致 | -5 |
| Major | 缺失图表标题/注释 | -5 |
| Minor | 长行 (>100 字符) | -1 (数学公式除外) |

## Beamer 幻灯片 (Slides/*.tex)

| 严重程度 | 问题 | 扣分 |
|----------|-------|-----------|
| Critical | XeLaTeX compilation 失败 | -100 |
| Critical | 未定义的引用 | -15 |
| Critical | Overfull hbox > 10pt | -10 |
| Major | 幻灯片内容过满 | -5 |
| Major | 记号不一致 | -3 |
| Minor | 字体大小减小 | -1 每张幻灯片 |

## Python 脚本 (scripts/*.py)

| 严重程度 | 问题 | 扣分 |
|----------|-------|-----------|
| Critical | 语法错误 | -100 |
| Critical | 领域特定的错误 | -30 |
| Critical | 硬编码的绝对路径 | -20 |
| Major | 缺少随机种子设置 | -10 |
| Major | 缺少图形生成 | -5 |

## 执行

- **Score < 80:** 阻止提交。列出阻止问题。
- **Score < 90:** 允许提交，发出警告。列出建议。
- 用户可以通过提供理由来覆盖。

## 质量报告

仅在合并时生成。使用 `templates/quality-report.md` 格式。
保存到 `quality_reports/merges/YYYY-MM-DD_[branch-name].md`。

## 容差阈值 (Research)

<!-- 为你的领域自定义 -->

| 数量 | 容差 | 理由 |
|----------|-----------|-----------|
| 点估计 | [例如 1e-6] | [数值精度] |
| 标准误差 | [例如 1e-4] | [MC 变异性] |
| 覆盖率 | [例如 +/- 0.01] | [MC with B reps] |
