---
name: extract-tikz
description: 从Beamer源文件提取TikZ图表，编译为PDF，转换为SVG（使用0基索引）。更新Quarto幻灯片的TikZ图表时使用。
disable-model-invocation: true
argument-hint: "[LectureN, 例如 Lecture2]"
allowed-tools: ["Read", "Bash", "Glob"]
---

# 提取 TikZ 图表为 SVG

从 Beamer 源文件提取 TikZ 图表，编译为多页 PDF，并将每一页转换为 SVG 以用于 Quarto 幻灯片。

## 步骤

### 步骤 0: 新鲜度检查（必需）

**在编译之前，验证 `extract_tikz.tex` 与当前 Beamer 源文件匹配。**

1. 查找 Beamer 源文件：`ls Slides/$ARGUMENTS*.tex`
2. 从 Beamer 提取所有 `\begin{tikzpicture}` 块
3. 与 `Figures/$ARGUMENTS/extract_tikz.tex` 对比
4. 如果存在任何差异：从 Beamer 源更新 extract_tikz.tex
5. 如果 extract_tikz.tex 不存在：从头创建

### 步骤 1: 导航到讲座的 Figures 目录
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

### 步骤 5: 同步到 docs/ 进行部署
```bash
cd ../..
./scripts/sync_to_docs.sh $ARGUMENTS
```

### 步骤 6: 验证 SVG 文件
- 读取 2-3 个 SVG 文件以确认包含有效的 SVG 标记
- 确认文件大小合理（不是 0 字节）

### 步骤 7: 报告结果

## 单一真相源提醒
TikZ 图表必须先在 Beamer `.tex` 文件中编辑，然后逐字复制到 `extract_tikz.tex`。参见 `.claude/rules/single-source-of-truth.md`。
