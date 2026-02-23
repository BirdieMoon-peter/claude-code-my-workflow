---
name: extract-tikz
description: 从Beamer或LaTeX源文件提取TikZ图表，编译为PDF，转换为SVG（使用0基索引）。在需要单独使用TikZ图表时使用。
disable-model-invocation: true
argument-hint: "[文件名或文件夹名，例如 Lecture2 或 Slides/Talk_Topic.tex]"
allowed-tools: ["Read", "Bash", "Glob"]
---

# 提取 TikZ 图表为 SVG

从 LaTeX/Beamer 源文件提取 TikZ 图表，编译为多页 PDF，并将每一页转换为 SVG。

## 步骤

### 步骤 0: 新鲜度检查（必需）

**在编译之前，验证 `extract_tikz.tex` 与当前 LaTeX 源文件匹配。**

1. 查找源文件：`ls Slides/$ARGUMENTS*.tex` 或 `ls Papers/$ARGUMENTS*.tex`
2. 从源文件提取所有 `\begin{tikzpicture}` 块
3. 与 `Figures/$ARGUMENTS/extract_tikz.tex` 对比
4. 如果存在任何差异：从源文件更新 extract_tikz.tex
5. 如果 extract_tikz.tex 不存在：从头创建

### 步骤 1: 导航到 Figures 目录
```bash
cd Figures/$ARGUMENTS
```

### 步骤 2: 编译 extract_tikz.tex 文件
```bash
TEXINPUTS=../../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode extract_tikz.tex
```

### 步骤 3: 统计页数
```bash
pdfinfo extract_tikz.pdf | grep "Pages:"
```

### 步骤 4: 使用0基索引将每一页转换为 SVG

**关键：PDF 页面是1基索引，但输出的 SVG 文件是0基索引！**

```bash
PAGES=$(pdfinfo extract_tikz.pdf | grep "Pages:" | awk '{print $2}')
for i in $(seq 1 $PAGES); do
  idx=$(printf "%02d" $((i-1)))
  pdf2svg extract_tikz.pdf tikz_exact_$idx.svg $i
done
```

### 步骤 5: 验证 SVG 文件
- 读取 2-3 个 SVG 文件以确认包含有效的 SVG 标记
- 确认文件大小合理（不是 0 字节）

### 步骤 6: 报告结果

## 单一真相源提醒
TikZ 图表必须先在 `.tex` 文件中编辑，然后逐字复制到 `extract_tikz.tex`。参见 `.claude/rules/single-source-of-truth.md`。
