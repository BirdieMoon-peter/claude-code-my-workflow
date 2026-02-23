---
name: deploy
description: 渲染Quarto幻灯片并同步到docs/以部署到GitHub Pages。在修改后部署讲座幻灯片时使用。
disable-model-invocation: true
argument-hint: "[LectureN 或 'all']"
allowed-tools: ["Read", "Bash"]
---

# 部署幻灯片到 GitHub Pages

渲染 Quarto 幻灯片并将所有文件同步到 `docs/` 以部署到 GitHub Pages。

## 步骤

1. **运行同步脚本:**
   - 如果提供了 `$ARGUMENTS`（例如 "Lecture4"）：`./scripts/sync_to_docs.sh $ARGUMENTS`
   - 如果没有参数：`./scripts/sync_to_docs.sh`（同步所有讲座）

2. **验证部署:**
   - 检查 HTML 文件是否存在于 `docs/slides/`
   - 检查 `_files/` 目录是否已复制（RevealJS 资源）
   - 检查 `docs/Figures/` 是否从 `Figures/` 同步

3. **验证交互式图表**（如适用）:
   - 在渲染的 HTML 中搜索交互式组件数量
   - 确认数量符合预期

4. **验证 TikZ SVG**（如适用）:
   - 检查所有引用的 SVG 文件是否存在于 `docs/Figures/LectureN/`

5. **在浏览器中打开** 进行视觉验证:
   - `open docs/slides/LectureX_Name.html`
   - 确认幻灯片渲染、图片显示、导航正常

6. **向用户报告结果**

## 同步脚本的功能:
- 渲染 `Quarto/` 中的所有 `.qmd` 文件（跳过 `*_backup*` 文件）
- 将 HTML 和 `_files/` 目录复制到 `docs/slides/`
- 将 Beamer PDF 从 `Slides/` 复制到 `docs/slides/`
- 使用 rsync 将 `Figures/` 同步到 `docs/Figures/`
