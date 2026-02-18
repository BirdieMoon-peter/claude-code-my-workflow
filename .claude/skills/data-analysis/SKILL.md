---
name: data-analysis
description: 端到端Python数据分析工作流，从探索到回归再到发布级表格和图表
disable-model-invocation: true
argument-hint: "[数据集路径或分析目标描述]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Edit", "Bash", "Task"]
---

# 数据分析工作流

运行端到端的 Python 数据分析：加载、探索、分析并生成发布级输出。

**输入:** `$ARGUMENTS` — 数据集路径（例如 `data/county_panel.csv`）或分析目标描述（例如 "使用 CPS 数据回归教育对工资的影响，控制州固定效应"）。

---

## 约束条件

- **遵循 Python 代码规范** 在 `.claude/rules/python-code-conventions.md`
- **保存所有脚本** 到 `scripts/python/`，使用描述性名称
- **保存所有输出** （图表、表格、pickle）到 `output/`
- **使用 pickle** 保存每个计算对象 — Quarto 幻灯片可能需要它们
- **使用项目主题** 为所有图表（检查 `.claude/rules/` 中的自定义主题）
- **运行 python-reviewer** 在生成的脚本上，展示结果前

---

## 工作流阶段

### 阶段 1: 设置和数据加载

1. 阅读 `.claude/rules/python-code-conventions.md` 获取项目标准
2. 创建带有适当头部的 Python 脚本（标题、作者、目的、输入、输出）
3. 在顶部加载所需包（使用 `import`）
4. 设置随机种子一次：`np.random.seed(42)` 或 `random.seed(42)`
5. 加载并检查数据集

### 阶段 2: 探索性数据分析

生成诊断输出:
- **摘要统计:** `df.describe()`、缺失率、变量类型
- **分布:** 关键连续变量的直方图
- **关系:** 散点图、相关矩阵
- **时间模式:** 如果是面板数据，绘制随时间的趋势
- **组比较:** 如果有处理/对照，比较处理前均值

将所有诊断图表保存到 `output/diagnostics/`。

### 阶段 3: 主要分析

基于研究问题:
- **回归分析:** 使用 `statsmodels` 或 `linearmodels` 进行面板数据，`sklearn` 或 `statsmodels` 进行截面数据
- **标准误:** 在适当级别聚类（记录原因）
- **多种规格:** 从简单开始，逐步添加控制变量
- **效应大小:** 报告标准化效应以及原始系数

### 阶段 4: 可视化选择

**用户可选择可视化工具:**

**选项 A: matplotlib/seaborn（默认）**
- 使用 `matplotlib.pyplot` 和 `seaborn`
- 应用项目自定义主题/样式
- 适合: 标准学术图表、探索性分析

**选项 B: NanoBanana（科研绘图）**
- 引入: `from nano_banana import nb` 或 `import nano_banana as nb`
- 特别适合: 期刊级图表、统计结果可视化、回归系数图、边际效应图
- 安装: `pip install nano-banana`
- 参考 `.claude/rules/python-code-conventions.md` 中的 NanoBanana 最佳实践

### 阶段 5: 发布级输出

**表格:**
- 使用 `stargazer` 或 `pystout` 用于回归表（首选用于 LaTeX 兼容性）
- 或使用 `pandas.DataFrame.to_latex()` 用于摘要表
- 包含所有标准元素：系数、标准误、显著性星号、N、R平方
- 导出为 `.tex` 用于 LaTeX 包含和 `.html` 用于快速查看

**图表:**
- **使用 matplotlib/seaborn:**
  ```python
  import matplotlib.pyplot as plt
  plt.style.use('seaborn-v0_8')  # 或自定义样式
  fig.savefig(path, dpi=300, bbox_inches='tight', transparent=True)
  ```
- **使用 NanoBanana:**
  ```python
  import nano_banana as nb
  fig = nb.create_regression_plot(results, ...)
  nb.save_figure(fig, path, format='pdf', transparent=True)
  ```
- 设置 `transparent=True` 用于 Beamer 兼容性
- 包含适当的轴标签（句子大小写、单位）
- 明确尺寸导出：`figsize=(12, 5)` 或类似
- 保存为 `.pdf` 和 `.png`

### 阶段 6: 保存和审查

1. 使用 `pickle` 保存所有关键对象（回归结果、摘要表、处理后的数据）:
   ```python
   import pickle
   with open('output/results.pkl', 'wb') as f:
       pickle.dump(results, f)
   ```

2. 使用 `os.makedirs(..., exist_ok=True)` 根据需要创建 `output/` 子目录

3. 在生成的脚本上运行 python-reviewer 智能体:
   ```
   委托给 python-reviewer 智能体:
   "审查脚本 scripts/python/[script_name].py"
   ```

4. 处理审查中的任何关键或高优先级问题。

---

## 脚本结构

遵循此模板:

```python
#!/usr/bin/env python3
# ============================================================
# [描述性标题]
# 作者: [从项目上下文]
# 目的: [此脚本的功能]
# 输入: [数据文件]
# 输出: [图表、表格、pickle 文件]
# ============================================================

# 0. 设置 ----
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from statsmodels.formula.api import ols
from linearmodels import PanelOLS
import os

# 可选: NanoBanana 用于高级可视化
# import nano_banana as nb

np.random.seed(42)
os.makedirs("output/analysis", exist_ok=True)

# 1. 数据加载 ----
# [加载和清理数据]

# 2. 探索性分析 ----
# [摘要统计、诊断图]

# 3. 主要分析 ----
# [回归、估计]

# 4. 表格和图表 ----
# [发布级输出]

# 5. 导出 ----
# [pickle 保存所有对象，保存所有图表]
```

---

## 重要提示

- **复现，不要猜测。** 如果用户指定了回归，准确运行。
- **展示你的工作。** 在跳转到回归之前打印摘要统计。
- **检查问题。** 查找多重共线性、异常值、完美预测。
- **使用相对路径。** 所有路径相对于仓库根目录。
- **没有硬编码值。** 对样本限制、日期范围等使用变量。
