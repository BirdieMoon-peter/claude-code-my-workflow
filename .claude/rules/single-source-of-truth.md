---
paths:
  - "Figures/**/*"
  - "Quarto/**/*.qmd"
  - "Slides/**/*.tex"
---

# 单一真相源：实施协议

**Beamer `.tex` 文件是所有内容的权威源。** 其他一切都是派生的。

## SSOT 链

```
Beamer .tex （真相源）
  ├── extract_tikz.tex → PDF → SVG（派生）
  ├── Quarto .qmd → HTML（派生）
  ├── Bibliography_base.bib（共享）
  └── Figures/LectureN/*.rds → plotly 图表（数据源）

永远不要独立编辑派生的工制品。
始终从源 → 派生传播更改。
```

---

## TikZ 新鲜度协议（强制性）

**在 Quarto 幻灯片中使用任何 TikZ SVG 之前，验证它与当前 Beamer 源匹配。**

### Diff-检查过程

1. 从 Beamer `.tex` 文件读取 TikZ 块
2. 从 `Figures/LectureN/extract_tikz.tex` 读取相应块
3. 比较**每个**坐标、标签、颜色、不透明度和锚点
4. 如果存在任何差异：从 Beamer 更新 `extract_tikz.tex`、重新编译、重新生成 SVG
5. 仅在之后在 QMD 中引用 SVG

### 何时重新提取

在以下情况下重新提取所有 TikZ 图表：
- Beamer `.tex` 文件自上次提取以来已被修改
- 开始新的 Quarto 翻译
- 报告任何与 TikZ 相关的质量问题
- 在任何包含 QMD 更改的提交之前

---

## 环境一致性（强制性）

**在翻译开始前，每个 Beamer 环境必须有 CSS 等效项。**

1. 扫描 Beamer 源以查找所有自定义环境
2. 对照你的主题 SCSS 文件检查每个
3. 如果 SCSS 中缺少任何环境，在翻译前创建它

---

## 内容保真度检查清单

```
[ ] 帧数：Beamer 帧 == Quarto 幻灯片
[ ] 数学检查：每个方程都以相同的记号出现
[ ] 引用检查：每个 \cite 在 Quarto 中都有 @key
[ ] 环境检查：每个 Beamer 框都有 CSS 等效项
[ ] 图形检查：每个 \includegraphics 都有 SVG 或 plotly 等效项
[ ] 无添加内容：Quarto 不发明 Beamer 中不存在的幻灯片
[ ] 无丢失内容：每个 Beamer 想法都出现在 Quarto 中
```
