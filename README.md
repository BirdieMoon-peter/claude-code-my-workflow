# Claude Code 学术论文写作工作流

一套基于 Claude Code 的完整学术工作流，专为**金融、量化金融、计算机科学、经济学**等领域的论文写作、实验设计和 Beamer 演示文稿制作而设计。

---

## 目录

1. [5 分钟快速开始](#快速开始)
2. [工作流全貌](#工作流全貌)
3. [文件夹结构](#文件夹结构)
4. [技能完整指南](#技能完整指南)
5. [代理系统](#代理系统)
6. [规则系统](#规则系统)
7. [核心工作模式](#核心工作模式)
8. [为你的领域定制](#为你的领域定制)
9. [前置条件](#前置条件)

---

## 快速开始

### 1. Fork 和克隆

```bash
git clone https://github.com/YOUR_USERNAME/claude-code-my-workflow.git my-project
cd my-project
```

### 2. 填写项目信息

编辑 `CLAUDE.md`，替换所有 `[占位符]`：

```
项目: 你的论文/项目名称
机构: 你的大学/研究机构
```

### 3. 启动 Claude Code，粘贴初始化提示

```bash
claude
```

粘贴以下内容：

> 我开始在这个仓库中处理 **[项目名称]**。这是一个关于 **[主题，例如：高频交易中的做市策略 / 深度学习在信用评分中的应用 / 双重差分法在政策评估中的应用]** 的研究项目。
>
> 我已经设置了 Claude Code 学术工作流。请阅读 `CLAUDE.md` 和 `README.md`，理解工作流，然后**更新所有配置文件以适应我的项目** — 填入 `CLAUDE.md` 中的占位符、根据我的领域调整规则，提出特定于我用例的自定义建议。
>
> 之后，对所有非平凡任务使用先规划工作流。一旦我批准计划，切换到承包商模式 — 自主协调所有内容，仅在有歧义或需要做出决定时回到我这里。

**这样做的作用：** Claude 读取配置文件、填入项目信息，然后进入承包商模式。你批准计划，Claude 处理其余部分。

---

## 工作流全貌

### 承包商模式

你描述一个任务。Claude 规划方法、实现它、运行专门的审查代理、修复问题、重新验证并对质量门控评分 — **全部自主完成**。

```
你的任务描述
    ↓
Claude 进入规划模式
    ↓
展示计划 → 你批准
    ↓
执行：实现 → 验证 → 审查 → 修复 → 评分
    ↓
得分 >= 80？是 → 呈现摘要
           否 → 继续修复（最多 5 轮）
```

### 质量门控

| 分数 | 操作 |
|------|------|
| 95+ | 卓越 — 理想目标 |
| 90+ | 准备提交/发布 |
| 80+ | 可以保存 |
| < 80 | 阻止提交，必须修复 |

---

## 文件夹结构

```
my-project/
├── CLAUDE.md                    # 项目配置（每次会话加载）
├── .claude/                     # 规则、技能、代理、钩子
│   ├── agents/                  # 专门化审查代理
│   ├── skills/                  # 可调用工作流（/command 形式）
│   ├── rules/                   # 自动加载的行为规则
│   ├── hooks/                   # 自动触发的钩子
│   └── WORKFLOW_QUICK_REF.md   # 一页速查表
├── Bibliography_base.bib        # 中央文献库（所有 .bib 引用）
├── Figures/                     # 图表（Python 生成的 PDF/PNG）
├── Preambles/                   # LaTeX 共用头文件
│   └── header.tex               # 包含所有 \usepackage 声明
├── Papers/                      # LaTeX 学术论文
│   └── MyPaper.tex              # 你的论文
├── Slides/                      # Beamer 演示文稿
│   └── Talk_Topic.tex           # 你的幻灯片
├── scripts/                     # Python 分析脚本
│   ├── python/                  # 分析、图表生成
│   └── quality_score.py         # 质量评分工具
├── quality_reports/             # 计划和审查报告
│   ├── plans/                   # 任务规划（用户批准）
│   ├── session_logs/            # 会话记录
│   └── merges/                  # 合并时质量报告
├── explorations/                # 实验性研究沙箱
│   └── ARCHIVE/                 # 存档的探索
├── templates/                   # 日志/报告模板
└── master_supporting_docs/      # 参考资料
    ├── supporting_papers/       # 参考论文 PDF
    └── supporting_slides/       # 参考幻灯片
```

---

## 技能完整指南

技能通过 `/command [arguments]` 调用。每个技能都是一个完整的多阶段工作流。

### 论文写作

#### `/review-paper [file]`
全面的手稿审查，生成类似顶级期刊审稿人的报告。

```
/review-paper Papers/MyPaper.tex
/review-paper master_supporting_docs/supporting_papers/some_paper.pdf
```

**输出：** `quality_reports/paper_review_[name].md`，包含：
- 总体建议（Strong Accept / R&R / Reject）
- 论证结构评估
- **识别策略**分析（因果主张的可信度）
- 计量经济学规范检查
- 文献定位分析
- 3-5 个"审稿人异议"（致命问题）

**适用场景：** 提交前自审、审查合作者论文、学习论文写作范式。

---

#### `/lit-review [topic]`
结构化文献搜索和综合，识别研究空白。

```
/lit-review "高频交易的价格影响"
/lit-review "transformer 在时间序列预测中的应用"
/lit-review "最低工资政策的就业效应 DiD 研究"
```

**输出：** `quality_reports/lit_review_[topic].md`，包含：
- 关键论文摘要（贡献、方法、发现）
- 主题组织（理论/实证/方法论）
- **研究空白与机会**
- BibTeX 引用条目

**提示：** 将感兴趣的论文放入 `master_supporting_docs/supporting_papers/`，让 lit-review 可以搜索它们。

---

#### `/research-ideation [topic]`
从主题或数据集生成结构化的研究问题和实证策略。

```
/research-ideation "中国股市的散户行为"
/research-ideation "使用 CRSP 日度数据：动量策略的风险因子分解"
/research-ideation "大语言模型在财务报告分析中的信息提取"
```

**输出：** `quality_reports/research_ideation_[topic].md`，包含：
- 3-5 个研究问题（描述性 → 因果性）
- 每个问题的识别策略、数据需求、关键假设
- 按可行性和贡献度的优先级排序

---

#### `/interview-me [topic]`
交互式采访，帮助正式化你的研究思路。

```
/interview-me "我想研究央行政策对债券市场的影响"
```

Claude 会追问问题，帮你厘清：研究问题、识别策略、数据来源、贡献点。

---

#### `/validate-bib`
验证 `Bibliography_base.bib` 中的所有引用与 `.tex` 文件的交叉引用完整性。

```
/validate-bib
```

检查：缺失条目、键名不一致、格式问题、重复引用。

---

### 数据分析

#### `/data-analysis [dataset]`
端到端 Python 数据分析，从数据加载到发布级表格和图表。

```
/data-analysis data/stock_returns.csv
/data-analysis "使用 Compustat 数据分析企业杠杆与投资的关系，控制行业和年份固定效应"
/data-analysis "蒙特卡洛模拟：Black-Scholes 期权定价 vs 二叉树模型"
```

**输出（保存到 `output/`）：**
- Python 脚本（`scripts/python/analysis_name.py`）
- 图表（`.pdf` 和 `.png`，300 dpi）
- LaTeX 表格（`.tex`，可直接 `\input{}` 引用）
- 摘要统计

**工作流：** 设置 → EDA → 主要分析 → 可视化 → 发布级输出 → python-reviewer 代码审查

**支持方法：**
- 计量经济学：OLS、面板数据（FE/RE）、IV、DID、RDD、合成控制
- 金融：资产定价因子、时间序列、事件研究、期权定价
- ML：sklearn、pytorch、statsmodels、linearmodels

---

#### `/review-python [file]`
Python 代码质量审查，检查可重现性、风格、领域正确性。

```
/review-python scripts/python/analysis.py
```

---

### Beamer 幻灯片

#### `/create-lecture [topic]`
从头创建一份完整的 Beamer 演示文稿，适用于学术讲座、论文展示、研讨会。

```
/create-lecture "期权定价：Black-Scholes 模型及其局限"
/create-lecture "双重差分法：假设、实现与稳健性检验"
/create-lecture "Transformer 架构详解"
```

**协作流程：**
1. 分析提供的材料（论文、数据结果、已有幻灯片）
2. 提议大纲（你批准）
3. **分批创建幻灯片**（5-10 张一批，你给反馈）
4. 生成配套 TikZ 图表和 Python 图形
5. 完整编译并审查

---

#### `/compile-latex [file]`
3 遍 XeLaTeX + bibtex 编译，检查错误并报告。

```
/compile-latex Slides/Talk_Topic.tex
/compile-latex Papers/MyPaper.tex
```

---

#### `/extract-tikz [file]`
从 `.tex` 文件提取 TikZ 图表，编译为独立 PDF，再转换为 SVG。

```
/extract-tikz Slides/Talk_Topic.tex
```

---

#### `/proofread [file]`
语法、拼写、符号一致性、学术质量全面审查。

```
/proofread Papers/MyPaper.tex
/proofread Slides/Talk_Topic.tex
```

**输出：** `quality_reports/[file]_report.md`

---

#### `/visual-audit [file]`
Beamer 幻灯片视觉布局审计：溢出、字体一致性、盒子疲劳、间距。

```
/visual-audit Slides/Talk_Topic.tex
```

---

#### `/pedagogy-review [file]`
叙事弧、教学模式、符号密度、节奏审查（适用于教学讲义）。

```
/pedagogy-review Slides/Lecture_Topic.tex
```

---

#### `/slide-excellence [file]`
综合多代理审查：同时运行视觉审核 + 教学审查 + 校对 + TikZ 审查 + 实质审查。

```
/slide-excellence Slides/Talk_Topic.tex
```

**适用场景：** 重要演讲前的全面检查。

---

#### `/devils-advocate [file]`
挑战你的设计决策，提出批判性问题。

```
/devils-advocate Slides/Talk_Topic.tex
/devils-advocate Papers/MyPaper.tex
```

---

### 版本控制

#### `/commit [message]`
暂存所有更改、提交、创建 PR 并合并到 main。

```
/commit "添加期权定价章节"
/commit "修复回归表中的标准误"
```

---

## 代理系统

代理是专门化的子 AI，在狭窄任务上比通才更出色。技能会在需要时自动调用它们。

| 代理 | 功能 | 调用方式 |
|------|------|----------|
| `proofreader` | 语法、拼写、一致性 | `/proofread` |
| `slide-auditor` | 视觉布局（溢出、字体、间距） | `/visual-audit` |
| `pedagogy-reviewer` | 叙事弧、13种教学模式 | `/pedagogy-review` |
| `python-reviewer` | Python 代码质量、可重现性 | `/review-python` |
| `tikz-reviewer` | TikZ 图表视觉批评 | `/slide-excellence` |
| `domain-reviewer` | 领域实质正确性（金融/量化/CS/经济） | `/slide-excellence` |
| `verifier` | 端到端任务完成验证 | 自动（任务结束时） |

### `domain-reviewer` 覆盖的检查

**金融/量化：**
- 无套利假设是否明确
- Ito 引理应用是否正确
- Greeks 推导（delta, gamma, vega, theta）
- 风险度量（VaR, CVaR）计算
- 前视偏差防范

**计量经济学/实证：**
- 识别假设（平行趋势、排除限制）
- 聚类标准误级别是否正确
- 多重检验校正
- 稳健性检验完整性

**CS/算法：**
- 时间/空间复杂度分析
- 算法正确性证明
- 收敛条件

---

## 规则系统

规则在 Claude 处理匹配文件时自动加载，无需手动调用。

### 始终加载的规则

| 规则 | 作用 |
|------|------|
| `plan-first-workflow` | 非平凡任务必须先规划，计划保存到 `quality_reports/plans/` |
| `orchestrator-protocol` | 承包商模式：实现 → 验证 → 审查 → 修复 → 评分 |
| `session-logging` | 计划后、增量进度、会话结束时记录日志 |

### 路径触发的规则

| 规则 | 触发条件 | 作用 |
|------|----------|------|
| `verification-protocol` | `.tex` 文件 | 编译验证清单 |
| `single-source-of-truth` | `.tex`, `Figures/` | LaTeX 是权威源，不独立编辑派生文件 |
| `quality-gates` | `.tex`, `.py` | 80/90/95 分数门槛 |
| `python-code-conventions` | `.py` | Python 编码标准、类型提示、文档字符串 |
| `tikz-visual-quality` | `.tex` | TikZ 图表视觉标准 |
| `no-pause-beamer` | `.tex` | Beamer 中禁用 `\pause` 覆盖命令 |
| `replication-protocol` | `.py` | 先复制原始结果，再扩展 |
| `pdf-processing` | `master_supporting_docs/` | 安全处理大 PDF |
| `exploration-folder-protocol` | `explorations/` | 实验性工作的结构化沙箱 |
| `exploration-fast-track` | `explorations/` | 快速原型（60/100 质量阈值） |

---

## 核心工作模式

### 模式 1：从零写一篇论文

```
# 第一步：文献调研
/lit-review "你的研究主题"

# 第二步：研究设计
/research-ideation "基于文献调研的具体问题"
# 或
/interview-me "我想研究..."

# 第三步：数据分析
/data-analysis "数据集路径或分析描述"

# 第四步：写作（Claude 直接辅助）
> 帮我起草论文引言，基于以下研究问题和文献：...

# 第五步：迭代完善
/proofread Papers/MyPaper.tex

# 第六步：最终审查
/review-paper Papers/MyPaper.tex
```

---

### 模式 2：制作论文展示幻灯片

```
# 第一步：创建幻灯片（会问你批准大纲）
/create-lecture "论文标题或主题"

# 第二步：编译验证
/compile-latex Slides/MyTalk.tex

# 第三步：全面审查
/slide-excellence Slides/MyTalk.tex

# 第四步：最终检查
/devils-advocate Slides/MyTalk.tex
```

---

### 模式 3：实验性探索（快速通道）

在 `explorations/` 文件夹中工作时，工作流更轻量：
- **质量阈值降低：** 60/100（vs 正式工作的 80/100）
- **无需完整规划：** 只需 2 分钟研究价值检查
- **快速迭代：** 先运行，后分析

```
# 在 explorations/ 中探索想法
> 在 explorations/ 中测试这个想法：[描述实验]

# 确认值得推进后，移到正式目录
> 将 explorations/my_idea/ 的结果整合到 Papers/MyPaper.tex
```

---

### 模式 4：复制已有论文的结果

```
# 第一步：将论文放入 master_supporting_docs/supporting_papers/
# 第二步：开始复制
> 复制 [论文名] 的表格 2 第 3 列结果。
> 论文在 master_supporting_docs/supporting_papers/paper.pdf

# Claude 会：
# 1. 读取 replication-protocol 规则
# 2. 建立黄金标准目标（与论文数字完全匹配）
# 3. 实现分析代码
# 4. 验证是否与目标匹配（在容差范围内）
# 5. 再扩展或修改
```

---

### 模式 5：审查合作者论文

```
/review-paper path/to/collaborator_paper.pdf
```

获得类似期刊审稿人的详细报告：论证结构、识别策略、计量经济学规范、文献定位、审稿人异议。

---

## 为你的领域定制

### 第 1 步：填写知识库（`.claude/rules/knowledge-base-template.md`）

添加你的领域符号约定、核心公式、常用数据集：

```markdown
## 符号约定
- $r_t$：对数收益率（不是简单收益率）
- $P_t$：资产价格
- $\Sigma$：协方差矩阵（不用 $V$ 或 $S$）

## 核心公式
- BS 公式：$C = S_0 N(d_1) - K e^{-rT} N(d_2)$

## 数据源
- CRSP：股票日度收益，1926-至今
- Compustat：上市公司财务报表
```

### 第 2 步：自定义领域审查者（`.claude/agents/domain-reviewer.md`）

在镜头 4 中添加你的领域已知陷阱：

```markdown
## 镜头 4：代码理论对齐
<!-- 自定义：你的领域已知陷阱 -->
- [ ] 高频数据：确认使用 bid-ask midpoint 而非 last price
- [ ] 事件研究：确认正确的估计窗口和事件窗口
- [ ] 面板数据：Driscoll-Kraay SE 用于时间序列相关
```

### 第 3 步：自定义 Beamer 环境（`CLAUDE.md`）

填写你项目中使用的自定义 LaTeX 环境：

```markdown
## Beamer 自定义环境
| `keybox` | 金色背景框 | 关键结论 |
| `definitionbox[Title]` | 蓝色标题框 | 正式定义 |
| `assumptionbox` | 灰色框 | 假设陈述 |
```

### 第 4 步：设置调色板（`scripts/python/` 中的代码）

在 `.claude/rules/python-code-conventions.md` 中设置项目颜色：

```python
PRIMARY_COLOR = "#012169"   # 机构蓝
ACCENT_COLOR = "#f2a900"    # 强调金
```

### 第 5 步：填写容差阈值（`.claude/rules/quality-gates.md`）

```markdown
| 点估计 | 1e-6 | 数值精度 |
| 标准误差 | 1e-4 | MC 变异性 |
```

---

## 前置条件

| 工具 | 用途 | 安装 |
|------|------|------|
| [Claude Code](https://docs.anthropic.com/claude-code) | **必须** — 整套工作流的基础 | `npm install -g @anthropic-ai/claude-code` |
| XeLaTeX | LaTeX 编译 | [TeX Live](https://tug.org/texlive/) 或 [MacTeX](https://tug.org/mactex/) |
| Python 3.8+ | 数据分析和图表 | [python.org](https://www.python.org/) |
| pdf2svg | TikZ → SVG 转换 | `brew install pdf2svg` (macOS) / `apt install pdf2svg` (Linux) |
| [gh CLI](https://cli.github.com/) | PR 工作流（可选） | `brew install gh` (macOS) |

**Python 依赖：**
```bash
pip install numpy pandas matplotlib seaborn scipy scikit-learn statsmodels linearmodels stargazer
```

---

## 许可证

MIT 许可证。自由用于教学、研究或任何学术目的。
