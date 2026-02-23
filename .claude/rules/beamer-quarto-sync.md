---
paths:
  - "Slides/**/*.tex"
  - "Quarto/**/*.qmd"
---

# Beamer → Quarto 自动同步规则 (强制)

**每次编辑 Beamer `.tex` 文件必须立即同步到对应的 Quarto `.qmd` 文件 — 自动进行,无需用户询问。** 这是强制的。

## 规则

修改 Beamer `.tex` 文件时,你必须也对 Quarto `.qmd` (如果存在) 应用等效更改 **在同一任务中**,然后报告完成。不要等待被要求。不要只是"标记偏差"。就做它。

## 讲座映射

<!-- 为你的讲座自定义此表 -->
| 讲座 | Beamer | Quarto |
|---------|--------|--------|
| 1 | `Slides/Lecture1_Topic.tex` | `Quarto/Lecture1_Topic.qmd` |
| 2 | `Slides/Lecture2_Topic.tex` | `Quarto/Lecture2_Topic.qmd` |
<!-- 创建讲座时添加行 -->

## 工作流 (每次)

1. 在 Beamer `.tex` 中应用修复
2. **立即**在 Quarto `.qmd` 中应用等效修复
3. 编译 Beamer (3-pass xelatex)
4. 渲染 Quarto (`./scripts/sync_to_docs.sh LectureN`)
5. 然后报告任务完成

## LaTeX → Quarto 转换参考

| Beamer | Quarto 等效 |
| ------ | ----------------- |
| `\muted{text}` | `[text]{style="color: #525252;"}` |
| `\key{text}` | `[**text**]{.emorygold}` |
| `\textcolor{positive}{text}` | `[text]{.positive}` |
| `\textcolor{negative}{text}` | `[text]{.negative}` |
| `\item text` | `- text` |
| `\begin{highlightbox}` | `::: {.highlightbox}` |
| `\begin{methodbox}` | `::: {.methodbox}` |
| `$formula$` | `$formula$` (相同) |

## 何时不同步

- Quarto 文件尚不存在
- 更改是仅 LaTeX 基础设施 (preamble、theme 文件)
- 明确要求跳过 Quarto 同步

## 执行

标记任何 Beamer 编辑任务完成前,检查:
> "我也更新了 Quarto 文件吗?"

如果答案是否且 Quarto 文件存在,**你没有完成。**

## 何时更新此表

创建新 Quarto 翻译后,将其添加到上面的映射表。
