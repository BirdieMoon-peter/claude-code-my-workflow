---
name: slide-excellence
description: 综合幻灯片卓越审查，结合视觉审核、教学审查、校对和可选的TikZ/实质审查。生成多个报告和综合摘要。
disable-model-invocation: true
argument-hint: "[TEX 文件名，例如 Lecture1_Topic.tex]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Task"]
---

# 幻灯片卓越审查

对 Beamer 幻灯片进行全面的多维度审查。多个智能体独立分析文件，然后综合结果。

## 步骤

### 1. 识别文件

解析 `$ARGUMENTS` 获取文件名。在 `Slides/` 中解析路径。

### 2. 并行运行审查智能体

**智能体 1: 视觉审核** (slide-auditor)
- 溢出、字体一致性、盒子疲劳、间距、图片
- 保存：`quality_reports/[FILE]_visual_audit.md`

**智能体 2: 教学审查** (pedagogy-reviewer)
- 13种教学模式、叙事、节奏、符号
- 保存：`quality_reports/[FILE]_pedagogy_report.md`

**智能体 3: 校对** (proofreader)
- 语法、拼写、一致性、学术质量、引用
- 保存：`quality_reports/[FILE]_report.md`

**智能体 4: TikZ 审查** (仅当文件包含 TikZ 时)
- 标签重叠、几何精度、视觉语义
- 保存：`quality_reports/[FILE]_tikz_review.md`

**智能体 5: 实质审查** (可选，对 .tex 文件)
- 通过 domain-reviewer 协议进行领域正确性审查
- 保存：`quality_reports/[FILE]_substance_review.md`

### 3. 综合摘要

```markdown
# 幻灯片卓越审查: [Filename]

## 总体质量评分: [优秀 / 良好 / 需改进 / 较差]

| 维度 | 关键 | 中等 | 低 |
|-----------|----------|--------|-----|
| 视觉/布局 | | | |
| 教学 | | | |
| 校对 | | | |

### 关键问题（需要立即行动）
### 中等问题（下次修订）
### 建议的下一步
```

## 质量评分标准

| 评分 | 关键 | 中等 | 含义 |
|-------|----------|--------|---------|
| 优秀 | 0-2 | 0-5 | 可以展示 |
| 良好 | 3-5 | 6-15 | 小幅改进 |
| 需改进 | 6-10 | 16-30 | 重大修订 |
| 较差 | 11+ | 31+ | 大规模重构 |
