---
paths:
  - "Slides/**/*.tex"
  - "Quarto/**/*.qmd"
  - "docs/**"
---

# 任务完成验证协议

**在每项任务结束时，Claude 必须验证输出工作正确。** 这是不可协商的。

## 对于 Quarto/HTML 幻灯片：
1. 运行 `./scripts/sync_to_docs.sh`（或 `./scripts/sync_to_docs.sh LectureN`）以渲染和部署
2. 在浏览器中打开 HTML：`open docs/slides/LectureX.html`
3. 通过读取 2-3 个图像文件来验证图像显示以确认有效内容
4. 检查 HTML 源代码的正确图像路径
5. 通过扫描密集幻灯片来检查溢出
6. 验证环境一致性：每个 Beamer 框环境在 QMD 中都有 CSS 等效项
7. 报告验证结果

## 对于 LaTeX/Beamer 幻灯片：
1. 使用 xelatex 编译并检查错误
2. 打开 PDF 以验证图形渲染
3. 检查过满 hbox 警告

## 对于 HTML/Quarto 中的 TikZ 图表：
1. 浏览器**无法**内联显示 PDF 图像 — 始终转换为 SVG
2. 使用 SVG（矢量格式）进行清晰渲染：`pdf2svg input.pdf output.svg`
3. **绝不使用 PNG 作为图表** — PNG 是光栅格式，看起来模糊
4. 验证 SVG 文件包含有效的 XML/SVG 标记
5. 通过 `sync_to_docs.sh` 将 SVG 复制到 `docs/Figures/LectureX/`
6. **新鲜度检查：** 在使用任何 TikZ SVG 之前，验证 extract_tikz.tex 与当前 Beamer 源匹配

## 对于 R 脚本：
1. 运行 `Rscript scripts/R/filename.R`
2. 验证输出文件（PDF、RDS）已创建且大小不为零
3. 抽查估计是否合理

## 常见陷阱：
- **HTML 中的 PDF 图像**：浏览器不内联渲染 PDF → 转换为 SVG
- **相对路径**：`../Figures/` 从 `Quarto/` 有效，但从 `docs/slides/` 无效 → 使用 `sync_to_docs.sh`
- **假设成功**：始终验证输出文件存在且包含正确内容
- **陈旧的 TikZ SVG**：extract_tikz.tex 与 Beamer 源不同步 → 始终进行 diff-check

## 验证检查清单：
```
[ ] 输出文件成功创建
[ ] 无编译/渲染错误
[ ] 图像/图形正确显示
[ ] 路径在部署位置（docs/）中解析
[ ] 在浏览器/查看器中打开以确认视觉外观
[ ] 向用户报告结果
```
