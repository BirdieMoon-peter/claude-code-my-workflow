---
name: translate-to-quarto
description: 将 Beamer LaTeX 讲义翻译为 Quarto RevealJS。多阶段工作流，涵盖内容翻译、TikZ 提取、引文映射、图形处理、溢出审计和部署。
disable-model-invocation: true
argument-hint: "[Beamer .tex 文件名，例如 LectureN_Topic]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Edit", "Bash", "Task"]
---

# Beamer → Quarto 翻译工作流

将 Beamer LaTeX 讲义完整翻译为 Quarto RevealJS HTML 幻灯片。

**关键：Beamer .tex 文件是唯一的真实来源。**

---

## 第 0 阶段：飞行前检查

### 0A. 环境一致性审计
扫描 Beamer 中的所有自定义环境。验证 CSS 等效项是否存在于主题 SCSS 中。如果缺少任何内容，请先创建它们。

### 0B. TikZ 新鲜度验证
运行 `/extract-tikz` 以验证 SVG 与当前 Beamer 源匹配。

### 0C. RDS 数据清单
列出交互式图表所需的所有 RDS 文件。

### 0D. 引文密钥映射
从 Beamer 中提取所有引文，映射到参考文献密钥。

## 第 1 阶段：翻译前准备
- 读取完整的 Beamer 源，计数幻灯片
- 清点图形（TikZ → SVG、R 绘图 → plotly、其他 → SVG）

## 第 2 阶段：使用 YAML 标头创建 QMD 文件
- 标准的 RevealJS YAML，包含主题、徽标、页脚、参考文献
- 如果需要，为 R 数据加载设置块

## 第 3 阶段：逐幻灯片翻译
- 委托给 `beamer-translator` 智能体
- 1:1 幻灯片映射
- 逐字数学、环境一致性、无字体缩小

## 第 4 阶段：TikZ 图表集成
使用 0 索引参考提取的 SVG。

## 第 5 阶段：R 图形集成（Plotly 优先）
来自 RDS 数据的交互式 plotly，TikZ/复杂图形的静态 SVG。

## 第 6 阶段：首次渲染和内容保真度检查
渲染、计数幻灯片、检查每张幻灯片以查找问题。

## 第 6.5 阶段：教学法审查
在视觉润饰之前运行教学法审查者。

## 第 7 阶段：视觉润饰
语义颜色、过渡幻灯片、框架句子。

## 第 8 阶段：校对
在 QMD 文件上运行 `/proofread`。

## 第 9 阶段：最终验证和部署
渲染，在浏览器中打开，验证所有元素。

## 第 10 阶段：Beamer 源同步
将任何更正应用回 Beamer 源。

## 第 11 阶段：文档
更新 CLAUDE.md、会话日志、创建 PR。
