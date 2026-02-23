---
name: review-python
description: 对学术Python脚本执行代码审查。检查代码质量、可复现性、图表生成模式和主题合规性。在编写或修改Python脚本后使用。
disable-model-invocation: true
argument-hint: "[文件名或 'all' 或 'LectureN']"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Task"]
---

# 审查 Python 脚本

执行全面的 Python 代码审查协议。

## 步骤

1. **识别要审查的脚本:**
   - 如果 `$ARGUMENTS` 是特定的 `.py` 文件名：仅审查该文件
   - 如果 `$ARGUMENTS` 是 `LectureN`：审查匹配该讲座的所有 Python 脚本
   - 如果 `$ARGUMENTS` 是 `all`：审查 `scripts/python/` 和 `Figures/*/` 中的所有 Python 脚本

2. **对每个脚本启动 `python-reviewer` 智能体**，指示其:
   - 遵循智能体说明中的完整协议
   - 阅读 `.claude/rules/python-code-conventions.md` 获取当前标准
   - 将报告保存到 `quality_reports/[script_name]_python_review.md`

3. **所有审查完成后**，呈现摘要:
   - 每个脚本发现的问题总数
   - 按严重程度分类（关键/高/中/低）
   - 前 3 个最关键的问题

4. **重要提示：不要编辑任何 Python 源文件。**
   仅生成报告。修复在用户审查后应用。
