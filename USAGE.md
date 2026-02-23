# Claude Code 学术工作流完整使用指南

> 本指南提供从零开始使用 Claude Code 学术工作流撰写论文的完整步骤。

---

## 目录

1. [快速开始](#1-快速开始)
2. [环境准备](#2-环境准备)
3. [项目初始化](#3-项目初始化)
4. [完整工作流：7个阶段](#4-完整工作流7个阶段)
5. [Skills 速查表](#5-skills-速查表)
6. [Agents 说明](#6-agents-说明)
7. [质量门控说明](#7-质量门控说明)
8. [探索文件夹使用说明](#8-探索文件夹使用说明)
9. [NanoBanana 绘图指南](#9-nanobanana-绘图指南)
10. [常见问题 FAQ](#10-常见问题-faq)
11. [建议的项目文件结构](#11-建议的项目文件结构)

---

## 1. 快速开始

### 从 Fork 到第一次运行（5分钟）

#### 步骤 1: Fork 并克隆仓库

```bash
# 在 GitHub 上 Fork 这个仓库，然后：
git clone https://github.com/YOUR_USERNAME/claude-code-my-workflow.git my-project
cd my-project
```

将 `YOUR_USERNAME` 替换为你的 GitHub 用户名。

#### 步骤 2: 启动 Claude Code 并粘贴初始化提示词

```bash
claude
```

**使用 VS Code？** 打开 Claude Code 面板。一切功能相同。

然后粘贴以下内容，填入你的项目详情：

> 我开始在这个仓库中工作 **[项目名称]**。**[用 2-3 句话描述你的项目 — 你在构建什么、给谁用、使用什么工具。]**
>
> 我希望我们的合作是结构化的、精确的和严谨的。在创建可视化内容时，一切都必须精致且符合出版标准。
>
> 我已经设置了 Claude Code 学术工作流（从 `BirdieMoon-peter/claude-code-my-workflow` fork）。配置文件已经在这个仓库中。请阅读它们，理解工作流，然后**更新所有配置文件以适合我的项目** — 在 `CLAUDE.md` 中填入占位符，根据需要调整规则，并提出任何特定于我用例的自定义建议。
>
> 之后，对所有非平凡任务使用"计划优先"工作流。一旦我批准了计划，切换到承包商模式 — 自主协调一切，只有在有歧义或需要做决定时才回来找我。
>
> 进入计划模式，首先为这个项目调整工作流配置。

**这会做什么：** Claude 读取所有配置文件，填入你的项目名称、机构和偏好，然后进入承包商模式 — 自主计划、实施、审查和验证。你批准计划，Claude 处理其余部分。

---

## 2. 环境准备

### 必需工具

| 工具 | 用途 | 安装方式 |
|------|------|---------|
| [Claude Code](https://code.claude.com/docs/en/overview) | 一切的核心 | `npm install -g @anthropic-ai/claude-code` |
| XeLaTeX | LaTeX 编译 | [TeX Live](https://tug.org/texlive/) 或 [MacTeX](https://tug.org/mactex/) |
| [Quarto](https://quarto.org) | Web 幻灯片 | [quarto.org/docs/get-started](https://quarto.org/docs/get-started/) |
| Python | 数据分析 | [python.org](https://www.python.org/) （建议 Python 3.8+）|
| pdf2svg | TikZ 转 SVG | `brew install pdf2svg` (macOS) 或 `apt-get install pdf2svg` (Linux) |
| [gh CLI](https://cli.github.com/) | PR 工作流 | `brew install gh` (macOS) 或参见官方文档 |

### Python 包（根据需要安装）

```bash
# 基础数据分析
pip install numpy pandas matplotlib seaborn

# 统计和回归
pip install statsmodels scipy scikit-learn linearmodels

# 科研绘图（可选）
pip install nano-banana

# 表格导出
pip install stargazer pystout

# Jupyter（可选，用于探索）
pip install jupyter notebook
```

### 验证安装

```bash
# 检查 Claude Code
claude --version

# 检查 XeLaTeX
xelatex --version

# 检查 Quarto
quarto --version

# 检查 Python
python --version

# 检查 pdf2svg
pdf2svg --help

# 检查 gh CLI
gh --version
```

---

## 3. 项目初始化

### 初始化提示词模板（中文版）

在第一次与 Claude Code 交互时使用此模板：

```
我正在开始一个名为 [项目名称] 的学术项目。

项目背景：
- 研究领域：[例如：发展经济学、因果推断、教育政策]
- 目标受众：[例如：博士课程、期刊投稿、会议演讲]
- 主要工具：LaTeX/Beamer + Python + Quarto

我已经配置了 Claude Code 学术工作流。请：

1. 阅读 CLAUDE.md、所有 rules、skills 和 agents
2. 填写 CLAUDE.md 中的占位符：
   - [YOUR PROJECT NAME] → [实际项目名]
   - [YOUR INSTITUTION] → [你的机构]
   - Beamer 自定义环境（如果有）
   - Quarto CSS 类（如果有）
3. 根据我的领域调整 domain-reviewer.md
4. 填写 knowledge-base-template.md 中的符号和应用
5. 建议任何特定于我领域的自定义

完成配置后，进入计划模式并建议第一个任务。
```

### Claude 会做什么

Claude 会：
1. 读取所有配置文件
2. 更新 `CLAUDE.md` 中的项目信息
3. 根据你的领域自定义 `domain-reviewer.md`
4. 提出第一批任务（可能是创建第一个讲座或设置数据分析）

---

## 4. 完整工作流：7个阶段

### 从0到1写一篇论文的完整流程

#### 阶段 1: 环境搭建与配置（已完成 ✓）

参见上面的"快速开始"和"环境准备"部分。

#### 阶段 2: 研究构思

**目标：** 将模糊的想法提炼为具体的研究问题。

**使用的 Skills:**
- `/interview-me [topic]` — 互动式研究访谈
- `/research-ideation [topic]` — 生成研究问题和策略

**典型对话:**

```
你：/interview-me "教育支出对学生表现的影响"

Claude：开始一系列问题来明确你的研究：
- 具体的教育支出类型？（教师工资、基础设施、技术）
- 学生表现如何衡量？（考试成绩、毕业率、就业）
- 因果推断策略？（DID、RDD、IV）
- 数据可得性？

[经过 10-15 分钟对话]

Claude：生成结构化的研究提案保存到 quality_reports/research_proposal.md
```

**输出:**
- `quality_reports/research_proposal.md` — 结构化研究提案
- `quality_reports/empirical_strategy.md` — 识别策略详情

#### 阶段 3: 文献综述

**目标：** 系统地审查相关文献并识别研究缺口。

**使用的 Skill:**
- `/lit-review [topic]` — 文献搜索、综合和缺口识别

**典型对话:**

```
你：/lit-review "教育支出的因果效应"

Claude：
1. 搜索相关论文（使用 WebSearch）
2. 按主题组织文献（理论、方法、实证）
3. 识别方法论方法（RCT、DID、IV、RDD）
4. 突出研究缺口
5. 建议你的论文如何贡献

保存报告到 quality_reports/literature_review.md
```

**输出:**
- `quality_reports/literature_review.md` — 结构化文献综述
- 关键论文列表
- 研究缺口识别
- 你的贡献定位

#### 阶段 4: 实证分析

**目标：** 在 `explorations/` 中进行数据分析，直到稳定。

##### 4.1 创建探索项目

```
你：请在 explorations/ 中创建一个新的探索项目，用于教育支出分析。

Claude：
创建 explorations/education-spending-did/
├── README.md           # 目标、假设、状态
├── src/                # Python 脚本
│   ├── 01_data_prep.py
│   ├── 02_eda.py
│   └── 03_did_analysis.py
├── data/               # 数据文件（如果小）
├── output/             # 结果
│   ├── figures/
│   └── tables/
└── notes.md            # 研究笔记
```

##### 4.2 运行数据分析

```
你：/data-analysis "使用 DID 分析教育支出对考试成绩的影响"

Claude：
1. 读取 python-code-conventions.md
2. 创建 src/01_data_prep.py
3. 进行探索性数据分析
4. 运行 DID 规格
5. 生成诊断图表
6. 导出表格为 LaTeX
7. 保存所有对象为 pickle
8. 运行 python-reviewer 进行代码审查

询问是否使用 matplotlib/seaborn 或 NanoBanana 进行可视化
```

##### 4.3 迭代和改进

在 `explorations/` 中自由实验：
- 质量阈值较低（60/100）
- 更快的反馈循环
- 没有多轮审查

一旦稳定，"毕业"到生产：
```
你：分析看起来不错。请将代码毕业到 scripts/python/ 并更新质量标准到 80/100。

Claude：
1. 移动脚本到 scripts/python/
2. 运行完整的 python-reviewer
3. 应用 PEP8 和最佳实践
4. 更新 pickle 路径
5. 生成最终的质量报告
```

#### 阶段 5: 论文撰写

**目标：** 使用 LaTeX/Beamer 创建幻灯片或论文。

##### 5.1 创建新讲座/章节

```
你：/create-lecture

Claude：引导你完成：
1. 讲座编号和标题
2. 从模板或从头开始
3. 包含 TikZ 图表？
4. 对应的 Quarto 版本？
5. 初始内容大纲

创建 Slides/Lecture01_Topic.tex
```

##### 5.2 编译和验证

```
你：/compile-latex Lecture01_Topic

Claude：
1. 运行 3-pass XeLaTeX + bibtex
2. 检查 overfull hbox 警告
3. 验证引用
4. 打开 PDF 进行视觉检查
5. 报告结果
```

##### 5.3 提取 TikZ 图表（如果需要）

```
你：/extract-tikz Lecture01

Claude：
1. 从 Beamer 提取 TikZ 块
2. 编译为 PDF
3. 转换为 SVG（0基索引）
4. 同步到 docs/ 用于部署
```

##### 5.4 翻译到 Quarto（如果需要 Web 版本）

```
你：/translate-to-quarto Lecture01_Topic.tex

Claude：执行 11 阶段翻译：
1. 读取 Beamer 和 Quarto 规则
2. 分析 Beamer 结构
3. 映射环境到 CSS 类
4. 转换 TikZ 到 SVG 引用
5. 翻译 R 图表到 Quarto 块
6. ... 11 步完整翻译
7. 运行 QA 循环（评审员 vs 修复员）
```

#### 阶段 6: 审核打磨

**目标：** 在最终确定之前审查和改进内容。

##### 6.1 多智能体审查

```
你：/slide-excellence Lecture01_Topic.tex

Claude：并行运行 6 个智能体：
1. proofreader — 语法、拼写、一致性
2. slide-auditor — 溢出、字体、布局
3. pedagogy-reviewer — 13 种教学模式
4. tikz-reviewer — TikZ 视觉质量（如果有）
5. domain-reviewer — 领域正确性
6. （可选）parity check — Beamer vs Quarto

生成综合报告到 quality_reports/
```

##### 6.2 恶魔代言人审查

```
你：/devils-advocate

Claude：挑战你的设计决策：
- 为什么用这种估计器？
- 符号是否一致？
- 假设是否合理？
- 图表是否必要？

强制你在提交前辩护选择
```

##### 6.3 论文审查（对于论文）

```
你：/review-paper manuscript.tex

Claude：审查手稿：
- 结构（引言、文献、方法、结果）
- 计量经济学方法
- 预见审稿人异议
- 写作清晰度

生成详细报告
```

#### 阶段 7: 提交部署

**目标：** 提交代码并部署幻灯片到 GitHub Pages。

##### 7.1 部署到 GitHub Pages

```
你：/deploy Lecture1

Claude：
1. 渲染 Quarto 幻灯片
2. 复制 HTML 和资源到 docs/
3. 同步 Beamer PDF
4. 同步所有图表
5. 在浏览器中验证
6. 报告部署状态
```

##### 7.2 提交和 PR

```
你：/commit "完成 Lecture 1: 因果推断简介"

Claude：
1. 暂存所有更改
2. 提交并附消息
3. 创建 PR
4. 合并到 main
5. 报告完成
```

---

## 5. Skills 速查表

### 核心编译和部署

| Skill | 用途 | 示例 |
|-------|------|------|
| `/compile-latex [file]` | 3-pass XeLaTeX + bibtex 编译 | `/compile-latex Lecture01` |
| `/deploy [LectureN]` | 渲染 Quarto 并同步到 docs/ | `/deploy Lecture1` |
| `/extract-tikz [LectureN]` | TikZ → PDF → SVG | `/extract-tikz Lecture2` |

### 审查和质量

| Skill | 用途 | 示例 |
|-------|------|------|
| `/proofread [file]` | 语法、拼写、溢出审查 | `/proofread Lecture01.tex` |
| `/visual-audit [file]` | 幻灯片布局审核 | `/visual-audit Lecture01.qmd` |
| `/pedagogy-review [file]` | 教学质量审查 | `/pedagogy-review Lecture01.tex` |
| `/review-python [file]` | Python 代码质量审查 | `/review-python analysis.py` |
| `/qa-quarto [LectureN]` | 对抗性 Quarto vs Beamer QA | `/qa-quarto Lecture1` |
| `/slide-excellence [file]` | 综合多智能体审查 | `/slide-excellence Lecture01.tex` |

### 翻译和工作流

| Skill | 用途 | 示例 |
|-------|------|------|
| `/translate-to-quarto [file]` | Beamer → Quarto 翻译 | `/translate-to-quarto Lecture01.tex` |
| `/validate-bib` | 交叉引用验证 | `/validate-bib` |
| `/devils-advocate` | 挑战设计决策 | `/devils-advocate` |
| `/create-lecture` | 完整讲座创建工作流 | `/create-lecture` |
| `/commit [msg]` | 暂存、提交、PR、合并 | `/commit "完成第1讲"` |

### 研究工作流

| Skill | 用途 | 示例 |
|-------|------|------|
| `/lit-review [topic]` | 文献搜索和综合 | `/lit-review "DID methods"` |
| `/research-ideation [topic]` | 研究问题生成 | `/research-ideation "教育"` |
| `/interview-me [topic]` | 互动研究访谈 | `/interview-me "劳动经济学"` |
| `/review-paper [file]` | 手稿审查 | `/review-paper paper.tex` |
| `/data-analysis [dataset]` | 端到端 Python 分析 | `/data-analysis county_panel.csv` |

---

## 6. Agents 说明

### 幻灯片和内容智能体

| Agent | 角色 | 何时使用 |
|-------|------|---------|
| **proofreader** | 语法、拼写、一致性专家 | 最终确定幻灯片/论文前 |
| **slide-auditor** | 视觉布局审核员 | 检查溢出、间距、盒子疲劳 |
| **pedagogy-reviewer** | 教学质量专家 | 评估叙事、节奏、学生视角 |
| **tikz-reviewer** | TikZ 图表批评家 | 审核 TikZ 视觉质量 |
| **domain-reviewer** | 领域特定实质审查员 | 检查领域正确性（需自定义） |

### 翻译和 QA 智能体

| Agent | 角色 | 何时使用 |
|-------|------|---------|
| **beamer-translator** | Beamer → Quarto 翻译专家 | 将 Beamer 转换为 RevealJS |
| **quarto-critic** | 对抗性 Quarto 审核员 | 将 Quarto 与 Beamer 基准对比 |
| **quarto-fixer** | Quarto 修复实施员 | 应用评审员发现的修复 |
| **verifier** | 任务完成验证员 | 端到端验证 |

### 代码审查智能体

| Agent | 角色 | 何时使用 |
|-------|------|---------|
| **python-reviewer** | Python 代码质量审查员 | 审查 Python 脚本的可复现性和最佳实践 |

---

## 7. 质量门控说明

### 80/90/95 评分体系

| 分数 | 门控 | 含义 | 典型用途 |
|------|------|------|---------|
| **60** | 探索阈值 | 足以继续实验 | `explorations/` 中的快速原型 |
| **80** | 提交门控 | 足够好可以保存 | 提交到仓库 |
| **90** | PR 门控 | 准备部署 | 合并到 main |
| **95** | 卓越标准 | 期望目标 | 出版级工作 |

### 如何评分

评分由 `scripts/quality_score.py` 自动完成：

```bash
python scripts/quality_score.py Quarto/Lecture1_Intro.qmd
```

**评分标准:**
- **溢出:** 内容超出边界？（-10 每处）
- **一致性:** 符号、术语、引用一致？（-5 每处不一致）
- **图表质量:** 透明背景、适当尺寸、标签？（-3 每处）
- **代码质量:** PEP8、docstrings、类型提示？（-2 每处）
- **文档:** 所有函数/类有文档？（-1 每处缺失）

### 容差阈值

某些问题有容差：

| 问题类型 | 容差 | 示例 |
|---------|------|------|
| 数值比较 | < 0.01 | 复现检查中的舍入 |
| 行长度（数学） | 无限制 | 长公式实现 |
| 轻微格式 | 3 次以下 | 缺少尾随换行符 |

---

## 8. 探索文件夹使用说明

### `explorations/` 的工作流

**目的：** 快速实验和原型的沙盒，质量要求较低。

#### 创建新探索

```bash
explorations/
├── my-new-idea/           # 进行中
│   ├── README.md          # 目标、假设、状态
│   ├── src/               # Python 脚本
│   ├── data/              # 数据（如果小）
│   ├── output/            # 结果
│   └── notes.md           # 研究笔记
└── ARCHIVE/               # 已完成或放弃
    ├── completed_idea1/   # 已毕业到生产
    └── abandoned_idea2/   # 记录为什么停止
```

#### README.md 模板

参见 `templates/exploration-readme.md`：

```markdown
# [项目名称]

## 状态
- [ ] 探索中
- [ ] 准备毕业
- [ ] 已放弃

## 目标
[你在尝试什么？]

## 假设
- H1: [具体假设]
- H2: ...

## 成功标准
- [ ] 数据加载并清理
- [ ] 初步结果显示 [X]
- [ ] 通过 60/100 质量阈值

## 当前发现
[...] ```

#### 毕业到生产

一旦探索稳定：

```
你：这个分析已经稳定。请将其毕业到生产。

Claude：
1. 移动脚本从 explorations/my-idea/src/ 到 scripts/python/
2. 提高质量标准到 80/100
3. 运行完整的 python-reviewer
4. 应用所有 PEP8 和最佳实践
5. 更新所有路径引用
6. 移动 README 到 ARCHIVE/completed_my-idea/
7. 在 MEMORY.md 中记录学习内容
```

#### 放弃探索

如果探索没有成功：

```
你：这个方法行不通。请归档它。

Claude：
1. 移动到 ARCHIVE/abandoned_my-idea/
2. 使用 templates/archive-readme.md 模板
3. 记录：为什么放弃、尝试了什么、学到了什么
4. 在 MEMORY.md 中添加 [LEARN] 标签
```

### 快速跑道规则

在 `explorations/` 中：
- ✅ 质量阈值：60/100（而不是 80/100）
- ✅ 简化协调器：实施 → 验证 → 评分 → 完成
- ✅ 没有多轮审查循环
- ✅ 更快的反馈
- ✅ 允许更长的代码行（数学公式）

---

## 9. NanoBanana 绘图指南

### 什么是 NanoBanana？

NanoBanana 是一个专门为科研绘图设计的 Python 可视化库，特别适合：
- 期刊级图表
- 统计结果可视化
- 回归系数图
- 边际效应图
- 预测区间图

### 安装

```bash
pip install nano-banana
```

### 何时使用 NanoBanana vs matplotlib/seaborn？

| 使用场景 | 推荐工具 | 原因 |
|---------|---------|------|
| 探索性数据分析 | matplotlib/seaborn | 快速、灵活 |
| 简单散点图、直方图 | matplotlib/seaborn | 轻量级 |
| 回归系数图 | **NanoBanana** | 专门设计的统计可视化 |
| 边际效应可视化 | **NanoBanana** | 内置统计功能 |
| 预测区间 | **NanoBanana** | 自动置信区间 |
| 期刊级多面板图 | **NanoBanana** | 一致的学术风格 |
| 自定义复杂图表 | matplotlib | 更多控制 |

### 基本使用示例

#### 1. 回归系数图

```python
import nano_banana as nb
import pandas as pd
from statsmodels.formula.api import ols

# 运行回归
model = ols('y ~ x1 + x2 + x3', data=df).fit()

# 创建系数图
fig = nb.coef_plot(
    model,
    title='回归系数及 95% 置信区间',
    xlabel='系数估计值',
    vars_to_plot=['x1', 'x2', 'x3'],
    var_labels={'x1': '教育年限', 'x2': '经验', 'x3': '性别'}
)

# 保存为发布级质量
nb.save_figure(fig, 'output/coef_plot.pdf', dpi=300, transparent=True)
```

#### 2. 边际效应可视化

```python
import nano_banana as nb

# 计算边际效应
me = model.get_margeff()

# 可视化
fig = nb.marginal_effects_plot(
    me,
    at_values={'x2': [0, 5, 10, 15]},  # 在不同经验水平
    title='教育的边际效应（按经验水平）',
    xlabel='教育年限',
    ylabel='对工资的边际效应'
)

nb.save_figure(fig, 'output/marginal_effects.pdf', transparent=True)
```

#### 3. 预测区间

```python
import nano_banana as nb

# 生成预测
predictions = model.get_prediction(new_data)

# 可视化预测及置信区间
fig = nb.prediction_plot(
    predictions,
    actual=df['y'],
    title='模型预测 vs 实际值',
    xlabel='观察值',
    ylabel='工资',
    show_ci=True,
    ci_level=0.95
)

nb.save_figure(fig, 'output/predictions.pdf', transparent=True)
```

#### 4. 回归诊断（四面板）

```python
import nano_banana as nb

# 自动生成 4 面板诊断图
fig = nb.regression_diagnostics(
    model,
    panels=['residuals', 'qq', 'scale_location', 'leverage'],
    figsize=(12, 10)
)

nb.save_figure(fig, 'output/diagnostics.pdf', transparent=True)
```

### NanoBanana 最佳实践

1. **透明背景用于 Beamer**
   ```python
   nb.save_figure(fig, path, transparent=True)
   ```

2. **一致的图表尺寸**
   ```python
   fig = nb.coef_plot(..., figsize=(10, 6))  # Beamer 16:9
   ```

3. **使用项目色板**
   ```python
   nb.set_color_palette({
       'primary': '#012169',
       'accent': '#f2a900',
       'positive': '#15803d',
       'negative': '#b91c1c'
   })
   ```

4. **发布级 DPI**
   ```python
   nb.save_figure(fig, path, dpi=300)  # 期刊标准
   ```

5. **中文标签支持**
   ```python
   # 如需中文标签，配置字体
   import matplotlib.pyplot as plt
   plt.rcParams['font.sans-serif'] = ['SimHei']  # 或其他中文字体
   plt.rcParams['axes.unicode_minus'] = False
   ```

### 与工作流集成

在 `/data-analysis` skill 中，Claude 会询问：

```
你希望使用哪个可视化工具？
1. matplotlib/seaborn（默认，适合探索）
2. NanoBanana（科研绘图，适合发布）
```

选择后，Claude 将：
- 使用相应的库生成图表
- 应用项目主题/色板
- 确保 Beamer 兼容性（透明背景）
- 运行 python-reviewer 检查可视化质量

---

## 10. 常见问题 FAQ

### 一般问题

#### Q: Claude Code 是免费的吗？

A: Claude Code 本身需要 Anthropic API 访问。请查看 [claude.ai](https://claude.ai) 的定价。

#### Q: 我必须使用 Python 吗？可以用其他语言吗？

A: 这个工作流优化了 Python，但你可以调整规则和智能体以支持其他语言（R、Julia、Stata）。主要需要修改：
- `python-code-conventions.md` → 你的语言规范
- `python-reviewer.md` → 你的语言审查员
- `/data-analysis` skill → 你的语言工作流

#### Q: 我不做学术研究，可以用这个吗？

A: 可以！核心工作流（计划模式、质量门控、多智能体审查）适用于任何结构化写作项目。移除或调整特定于学术的部分（教学审查、领域审查员）。

#### Q: 可以在 VS Code 中使用吗？

A: 是的！Claude Code 与 VS Code 完全集成。所有 skills、agents 和 rules 都能无缝工作。

### 工作流问题

#### Q: 什么是"计划模式"vs"承包商模式"？

A: 
- **计划模式：** Claude 生成详细计划并等待批准
- **承包商模式：** 计划批准后，Claude 自主执行所有步骤，只在歧义时返回

#### Q: 质量门控太严格了怎么办？

A: 在 `explorations/` 中使用快速跑道（60/100）。或在 `.claude/rules/quality-gates.md` 中调整阈值。

#### Q: 如何跳过某些审查智能体？

A: 使用特定的 skill 而不是 `/slide-excellence`：
- 只要语法：`/proofread`
- 只要布局：`/visual-audit`
- 只要教学：`/pedagogy-review`

#### Q: 可以同时处理多个项目吗？

A: 建议每个项目使用单独的仓库克隆，因为 `CLAUDE.md` 是项目特定的。

### Python 和 NanoBanana 问题

#### Q: NanoBanana 是必需的吗？

A: 不是。它是可选的可视化工具。matplotlib/seaborn 是默认的，完全足够。

#### Q: 如何在 NanoBanana 和 matplotlib 之间切换？

A: 在 `/data-analysis` skill 中，Claude 会在可视化阶段询问你的偏好。或在你的请求中明确指定：

```
/data-analysis county_panel.csv  # 使用 matplotlib

或

使用 NanoBanana 对县面板数据进行分析  # 明确请求
```

#### Q: Python 环境管理呢？

A: 最佳实践：
```bash
# 创建虚拟环境
python -m venv venv
source venv/activate  # Linux/Mac
venv\Scripts\activate  # Windows

# 安装依赖
pip install -r requirements.txt

# 冻结依赖
pip freeze > requirements.txt
```

Claude 会在 `python-code-conventions.md` 中检查这些实践。

#### Q: 如何处理大型数据集？

A: 
1. 不要提交到 git（添加到 `.gitignore`）
2. 使用相对路径：`data/large_file.csv`
3. 考虑使用 `pandas` 的分块读取：
   ```python
   chunks = pd.read_csv('large.csv', chunksize=10000)
   ```
4. 保存处理后的数据为 pickle（更快）

### LaTeX 和 Beamer 问题

#### Q: 必须使用 XeLaTeX 吗？

A: 这个工作流假定使用 XeLaTeX。如果你更喜欢 pdflatex，需要修改 `/compile-latex` skill 和 `verification-protocol.md`。

#### Q: TikZ 图表太复杂怎么办？

A: 选项：
1. 在 Python 中生成图表并包含为 PNG/PDF
2. 使用 Inkscape 或 Adobe Illustrator 创建 SVG
3. 简化 TikZ 代码（tikz-reviewer 会建议）

#### Q: 如何自定义 Beamer 主题？

A: 
1. 编辑 `Preambles/header.tex`
2. 在 `CLAUDE.md` 中记录你的自定义环境
3. 在 `beamer-translator.md` 中映射到 Quarto CSS 类

### 部署和 GitHub 问题

#### Q: GitHub Pages 设置？

A: 
1. 转到仓库设置 → Pages
2. Source: Deploy from a branch
3. Branch: main
4. Folder: `/docs`
5. 保存

#### Q: 如何更新已部署的幻灯片？

A: 
```
/deploy Lecture1  # 重新渲染并同步
/commit "更新 Lecture 1"  # 提交并推送
```

GitHub Pages 会在几分钟内自动更新。

### 审查和质量问题

#### Q: python-reviewer 标记太多问题怎么办？

A: 
1. 专注于关键和高优先级问题
2. 在 `python-code-conventions.md` 中调整规则
3. 对探索性代码使用较低标准

#### Q: 如何自定义 domain-reviewer？

A: 
1. 编辑 `.claude/agents/domain-reviewer.md`
2. 添加你领域特定的 5 个审查视角
3. 包含常见错误、符号约定、方法论检查

#### Q: 评审员和修复员循环永不收敛怎么办？

A: 
- QA 循环限制为 5 轮
- 之后，Claude 会上报剩余问题供手动审查
- 考虑调整 `.claude/rules/quality-gates.md` 中的硬性门控

---

## 11. 建议的项目文件结构

### 完整的学术项目结构

```
my-academic-project/
│
├── .claude/                        # Claude Code 配置
│   ├── agents/                     # 10 个智能体定义
│   ├── rules/                      # 17 个规则文件
│   ├── skills/                     # 19 个技能
│   ├── hooks/                      # Git hooks（可选）
│   ├── settings.json               # Claude Code 设置
│   └── WORKFLOW_QUICK_REF.md       # 快速参考
│
├── Slides/                         # LaTeX/Beamer 源文件
│   ├── Lecture01_Intro.tex
│   ├── Lecture02_Methods.tex
│   └── ...
│
├── Quarto/                         # Quarto/RevealJS 源文件
│   ├── Lecture1_Intro.qmd
│   ├── Lecture2_Methods.qmd
│   ├── _quarto.yml                 # Quarto 配置
│   └── custom.scss                 # 自定义主题
│
├── Preambles/                      # LaTeX headers
│   └── header.tex                  # Beamer 主题和包
│
├── Figures/                        # 所有图表
│   ├── Lecture1/
│   │   ├── tikz_exact_00.svg
│   │   └── extract_tikz.tex
│   ├── Lecture2/
│   └── ...
│
├── scripts/                        # 实用脚本
│   ├── python/                     # 生产 Python 代码
│   │   ├── 01_data_prep.py
│   │   ├── 02_analysis.py
│   │   └── 03_figures.py
│   ├── sync_to_docs.sh            # 部署脚本
│   └── quality_score.py           # 质量评分
│
├── data/                           # 数据文件（小型）
│   ├── raw/
│   ├── processed/
│   └── README.md                   # 数据文档
│
├── output/                         # 分析输出
│   ├── figures/                    # 生成的图表
│   │   ├── figure1.pdf
│   │   ├── figure1.png
│   │   └── ...
│   ├── tables/                     # LaTeX 表格
│   │   ├── table1.tex
│   │   └── ...
│   └── results/                    # Pickle 文件
│       ├── regression_results.pkl
│       └── ...
│
├── explorations/                   # 研究沙盒
│   ├── new-method/                # 进行中
│   │   ├── README.md
│   │   ├── src/
│   │   ├── output/
│   │   └── notes.md
│   └── ARCHIVE/                   # 已完成或放弃
│       ├── completed_idea1/
│       └── abandoned_idea2/
│
├── quality_reports/               # 审查报告
│   ├── plans/                     # 批准的计划
│   │   └── 2024-03-15_lecture-creation.md
│   ├── session_logs/              # 会话记录
│   │   └── 2024-03-15_slide-polish.md
│   └── merge_reports/             # 合并时的质量报告
│
├── templates/                     # 文档模板
│   ├── session-log.md
│   ├── quality-report.md
│   ├── exploration-readme.md
│   └── archive-readme.md
│
├── docs/                          # GitHub Pages（自动生成）
│   ├── slides/                    # 渲染的 HTML
│   │   ├── Lecture1_Intro.html
│   │   └── Lecture1_Intro_files/
│   └── Figures/                   # 同步的图表
│
├── master_supporting_docs/        # 参考材料
│   ├── papers/
│   └── existing_slides/
│
├── .gitignore                     # Git 忽略规则
├── Bibliography_base.bib          # 集中式参考文献
├── CLAUDE.md                      # 项目配置
├── MEMORY.md                      # 项目记忆
├── README.md                      # 项目描述
├── USAGE.md                       # 本文件
└── requirements.txt               # Python 依赖

```

### 最小项目（仅幻灯片）

如果你只需要幻灯片而不需要数据分析：

```
minimal-slides-project/
├── .claude/                       # 完整的 Claude 配置
├── Slides/                        # Beamer 源文件
├── Quarto/                        # RevealJS 源文件
├── Preambles/                     # LaTeX headers
├── Figures/                       # 图表
├── docs/                          # GitHub Pages
├── quality_reports/               # 审查报告
├── Bibliography_base.bib
├── CLAUDE.md
└── README.md
```

### 数据分析重点项目

如果重点是数据分析而不是幻灯片：

```
data-analysis-project/
├── .claude/                       # Claude 配置
├── scripts/                       # Python 代码
│   └── python/
├── data/                          # 数据
│   ├── raw/
│   └── processed/
├── output/                        # 结果
│   ├── figures/
│   ├── tables/
│   └── results/
├── explorations/                  # 研究沙盒
├── quality_reports/               # 审查报告
├── requirements.txt
├── CLAUDE.md
├── MEMORY.md
└── README.md
```

---

## 额外资源

### 官方文档

- [Claude Code 文档](https://code.claude.com/docs/en/overview)
- [Quarto 文档](https://quarto.org/docs/guide/)
- [Beamer 文档](https://ctan.org/pkg/beamer)
- [Python 文档](https://docs.python.org/)

### 相关工具

- [pandas 文档](https://pandas.pydata.org/docs/)
- [statsmodels 文档](https://www.statsmodels.org/)
- [matplotlib 文档](https://matplotlib.org/)
- [seaborn 文档](https://seaborn.pydata.org/)

### 社区和支持

- [Claude AI 社区](https://www.anthropic.com/community)
- [GitHub Issues](https://github.com/BirdieMoon-peter/claude-code-my-workflow/issues)

---

## 下一步

1. **完成环境准备** — 安装所有必需工具
2. **运行初始化提示词** — 让 Claude 自定义你的项目
3. **创建第一个探索** — 在 `explorations/` 中尝试一个小想法
4. **运行数据分析** — 使用 `/data-analysis` 
5. **创建第一个讲座** — 使用 `/create-lecture`
6. **审查和改进** — 使用 `/slide-excellence`
7. **部署** — 使用 `/deploy` 发布到 GitHub Pages

祝你写作顺利！🎓✨

---

*最后更新：2024-02*
*版本：2.0 (中文版 + Python)*
