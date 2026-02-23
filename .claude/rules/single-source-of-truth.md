---
paths:
  - "Figures/**/*"
  - "Papers/**/*.tex"
  - "Slides/**/*.tex"
---

# 单一真相源：实施协议

**LaTeX `.tex` 文件是所有内容的权威源。** 从论文或幻灯片派生的任何输出（PDF、图表）都是派生工件，永远不要独立编辑。

## SSOT 链

```
LaTeX .tex （真相源）
  ├── 编译 → PDF（派生）
  ├── TikZ 图表 → PDF → SVG（派生）
  ├── Bibliography_base.bib（共享文献库）
  └── Figures/（输入图表，Python 生成）

永远不要独立编辑派生工件。
始终从源 → 派生传播更改。
```

---

## TikZ 新鲜度协议（强制性）

**在任何文件中使用 TikZ SVG 之前，验证它与当前 LaTeX 源匹配。**

### Diff-检查过程

1. 从 `.tex` 文件读取 TikZ 块
2. 从 `Figures/extract_tikz.tex` 读取相应块
3. 比较**每个**坐标、标签、颜色、不透明度和锚点
4. 如果存在任何差异：从 `.tex` 更新 `extract_tikz.tex`、重新编译、重新生成 SVG
5. 仅在之后引用 SVG

### 何时重新提取

在以下情况下重新提取所有 TikZ 图表：
- `.tex` 文件自上次提取以来已被修改
- 报告任何与 TikZ 相关的质量问题

---

## 图表一致性（强制性）

**每个在 `.tex` 文件中引用的图表必须存在于 `Figures/` 中。**

1. 扫描 `.tex` 源以查找所有 `\includegraphics` 命令
2. 验证每个引用的文件存在于 `Figures/`
3. 如果缺少任何图表，在编译前生成或添加

---

## 内容保真度检查清单

```
[ ] PDF 与 .tex 源一致（无陈旧 PDF）
[ ] 所有引用的图表存在于 Figures/
[ ] 所有引文在 Bibliography_base.bib 中解析
[ ] TikZ SVG 与当前 .tex 源匹配
[ ] 无独立编辑的 PDF 或图表
```
