---
name: visual-audit
description: 对Quarto或Beamer幻灯片执行对抗性视觉审核，检查溢出、字体一致性、盒子疲劳和布局问题。
disable-model-invocation: true
argument-hint: "[QMD 或 TEX 文件名]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Task"]
---

# 幻灯片视觉审核

对幻灯片进行全面的视觉布局审核。

## 步骤

1. **读取幻灯片文件** 在 `$ARGUMENTS` 中指定

2. **对于 Quarto (.qmd) 文件:**
   - 使用 `quarto render Quarto/$ARGUMENTS` 渲染
   - 在浏览器中打开以检查每张幻灯片

3. **对于 Beamer (.tex) 文件:**
   - 编译并检查 overfull hbox 警告

4. **审核每张幻灯片的:**

   **溢出:** 超出幻灯片边界的内容
   **字体一致性:** 内联字体大小覆盖、不一致的大小
   **盒子疲劳:** 一张幻灯片上有 2 个或更多彩色盒子、错误的盒子类型
   **间距:** 缺少负边距、缺少 fig-align
   **布局:** 缺少过渡、缺少框架句、语义颜色

5. **生成报告**，按幻灯片组织，包含严重程度和建议

6. **遵循间距优先原则:**
   1. 使用负边距减少垂直间距
   2. 合并列表
   3. 将显示的公式改为内联
   4. 减小图片/SVG 大小
   5. 最后手段：减小字体大小（绝不低于 0.85em）
