---
name: visual-audit
description: 对Beamer幻灯片执行对抗性视觉审核，检查溢出、字体一致性、盒子疲劳和布局问题。
disable-model-invocation: true
argument-hint: "[TEX 文件名，例如 Talk_Topic.tex]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Task"]
---

# 幻灯片视觉审核

对 Beamer 幻灯片进行全面的视觉布局审核。

## 步骤

1. **读取幻灯片文件** 在 `$ARGUMENTS` 中指定（在 `Slides/` 中查找）

2. **对于 Beamer (.tex) 文件:**
   - 检查 overfull hbox 警告（编译时）
   - 逐张幻灯片分析内容密度

3. **审核每张幻灯片的:**

   **溢出:** 超出幻灯片边界的内容、overfull hbox 风险
   **字体一致性:** 过度使用 `\footnotesize` 或 `\tiny`
   **盒子疲劳:** 一张幻灯片上有 2 个或更多彩色盒子
   **间距:** 方程式前后空白不足、图像对齐问题
   **布局:** 缺少过渡、缺少框架句、语义颜色使用

4. **生成报告**，按幻灯片组织，包含严重程度和建议

5. **遵循间距优先原则:**
   1. 使用 `\vspace` 调整垂直间距
   2. 合并列表或拆分幻灯片
   3. 将显示的公式改为内联（若合适）
   4. 减小图片大小
   5. 最后手段：减小字体大小（不低于 `\small`）
