---
name: compile-latex
description: 使用XeLaTeX编译Beamer LaTeX幻灯片（3次编译 + bibtex）。用于编译讲座幻灯片。
disable-model-invocation: true
argument-hint: "[不含.tex扩展名的文件名]"
allowed-tools: ["Read", "Bash", "Glob"]
---

# 编译 Beamer LaTeX 幻灯片

使用 XeLaTeX 编译 Beamer 幻灯片，包含完整的引用解析。

## 步骤

1. **导航到 Slides/ 目录** 并使用3次编译序列：

```bash
cd Slides
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode $ARGUMENTS.tex
BIBINPUTS=..:$BIBINPUTS bibtex $ARGUMENTS
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode $ARGUMENTS.tex
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode $ARGUMENTS.tex
```

**替代方案 (latexmk):**
```bash
cd Slides
TEXINPUTS=../Preambles:$TEXINPUTS BIBINPUTS=..:$BIBINPUTS latexmk -xelatex -interaction=nonstopmode $ARGUMENTS.tex
```

2. **检查警告:**
   - 在输出中搜索 `Overfull \\hbox` 警告
   - 搜索 `undefined citations` 或 `Label(s) may have changed`
   - 报告发现的任何问题

3. **打开 PDF** 进行视觉验证:
   ```bash
   open Slides/$ARGUMENTS.pdf
   ```

4. **报告结果:**
   - 编译成功/失败
   - Overfull hbox 警告数量
   - 任何未定义的引用
   - PDF 页数

## 为什么需要3次编译？
1. 第一次 xelatex：创建包含引用键的 `.aux` 文件
2. bibtex：读取 `.aux`，生成格式化引用的 `.bbl` 文件
3. 第二次 xelatex：整合参考文献
4. 第三次 xelatex：解析所有交叉引用和最终页码

## 重要提示
- **始终使用 XeLaTeX**，不要使用 pdflatex
- **TEXINPUTS** 是必需的：你的 Beamer 主题在 `Preambles/` 目录
- **BIBINPUTS** 是必需的：你的 `.bib` 文件在仓库根目录
