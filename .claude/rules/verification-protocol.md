---
paths:
  - "Papers/**/*.tex"
  - "Slides/**/*.tex"
---

# 任务完成验证协议

**在每项任务结束时，Claude 必须验证输出工作正确。** 这是不可协商的。

## 对于 LaTeX 论文（Papers/*.tex）：

1. 使用 3 遍 xelatex + bibtex 编译并检查错误
2. 验证无未定义引用（检查 .log 文件中 `undefined references`）
3. 检查 overfull hbox 警告（> 10pt 为重要问题）
4. 打开 PDF 以验证图形、表格渲染正确
5. 抽查关键方程式、表格、图表标题
6. 向用户报告验证结果

## 对于 Beamer 幻灯片（Slides/*.tex）：

1. 使用 3 遍 xelatex + bibtex 编译并检查错误
2. 打开 PDF 以验证图形渲染
3. 检查 overfull hbox 警告
4. 验证每张幻灯片内容未溢出
5. 向用户报告验证结果

## 对于 TikZ 图表：

1. **绝不**将 TikZ 图表直接嵌入文档时跳过验证
2. 提取并单独编译 TikZ 以确认渲染正确
3. 如需 SVG：使用 `pdf2svg input.pdf output.svg`
4. **新鲜度检查：** 使用任何 TikZ SVG 之前，验证其与当前 LaTeX 源匹配

## 对于 Python 脚本：

1. 运行 `python3 scripts/script_name.py`
2. 验证输出文件（PDF、PNG、CSV）已创建且大小不为零
3. 抽查估计是否合理
4. 验证图表标签、坐标轴单位正确

## 常见陷阱：

- **陈旧的 PDF**：始终重新编译 .tex 以确保 PDF 是最新的
- **假设成功**：始终验证输出文件存在且包含正确内容
- **陈旧的 TikZ SVG**：在修改 .tex 后必须重新提取

## 验证检查清单：

```
[ ] 输出文件成功创建
[ ] 无编译/渲染错误（或记录了原因）
[ ] 图像/图形正确显示
[ ] 关键方程式/表格/结果已抽查
[ ] 向用户报告结果
```
