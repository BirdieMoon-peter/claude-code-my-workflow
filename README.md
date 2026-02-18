# Claude Code 学术工作流（中文版）

> **工作进行中。** 这不是为每个人准备的精美指南。它主要是我如何使用 Claude Code 进行学术工作的总结 — 创建讲座幻灯片、编写 Python 脚本、管理 Beamer 转 Quarto 工作流等。我不断学习新事物，随着学习的进行，我也不断更新这些文件。这只是我与朋友和同事分享我的心得的一种方式。

**在线网站：** [psantanna.com/claude-code-my-workflow](https://psantanna.com/claude-code-my-workflow/)

这是一个为使用 [Claude Code](https://code.claude.com/docs/en/overview) 进行学术工作的研究人员准备的即用型启动工具包，支持 **LaTeX/Beamer + Python + Quarto**。你描述你想要的内容；Claude 规划方法、运行专门的代理、修复问题、验证质量并呈现结果 — 就像一个承包商处理整个工作。该工作流从一门生产环境中的博士课程中提取（6 节课，800+ 幻灯片）。

---

## 快速开始（5 分钟）

### 1. Fork 和克隆

```bash
# 在 GitHub 上 Fork 此仓库（点击仓库页面上的 "Fork"），然后：
git clone https://github.com/YOUR_USERNAME/claude-code-my-workflow.git my-project
cd my-project
```

将 `YOUR_USERNAME` 替换为你的 GitHub 用户名。

### 2. 启动 Claude Code 并粘贴此提示

```bash
claude
```

**使用 VS Code？** 打开 Claude Code 面板即可。所有操作都一样 — 查看 [完整指南](https://psantanna.com/claude-code-my-workflow/workflow-guide.html#sec-setup) 获取详细信息。

然后粘贴以下内容，填入你的项目细节：

> 我开始在这个仓库中处理 **[项目名称]**。**[用 2-3 句话描述你的项目 — 你要构建什么、为谁构建、使用什么工具。]**
>
> 我希望我们的协作是结构化的、精确的和严谨的。创建视觉内容时，一切必须是精美的、可发表水平。
>
> 我已经设置了 Claude Code 学术工作流（从 `pedrohcgs/claude-code-my-workflow` fork）。配置文件已在此仓库中。请阅读它们、理解工作流，然后 **更新所有配置文件以适应我的项目** — 填入 `CLAUDE.md` 中的占位符、根据需要调整规则，并提出特定于我用例的任何自定义建议。
>
> 之后，对所有非平凡任务使用先规划工作流。一旦我批准计划，切换到承包商模式 — 自主协调所有内容，仅在有歧义或需要做出决定时回到我这里。
>
> 进入规划模式并开始为该项目调整工作流配置。

**这样做的作用：** Claude 读取所有配置文件、填入你的项目名称、机构和偏好，然后进入承包商模式 — 自主地规划、实现、审查和验证。你批准计划，Claude 处理其余部分。

**更喜欢手动配置？** 查看 [完整指南](https://psantanna.com/claude-code-my-workflow/workflow-guide.html#sec-setup) 获取分步手动设置说明。

---

## 工作原理

### 承包商模式

你描述一个任务。Claude 规划方法、实现它、运行专门的审查代理、修复问题、重新验证并针对质量门控评分 — 全部自主完成。当工作满足质量标准时，你会看到摘要。说"就这样做"，它也会自动提交。

### 专门化代理

而不是一个通用审查者，10 个聚焦代理各检查一个维度：

- **proofreader** — 语法/拼写错误
- **slide-auditor** — 视觉布局
- **pedagogy-reviewer** — 教学质量
- **python-reviewer** — Python 代码质量
- **domain-reviewer** — 领域特定正确性（模板 — 为你的领域自定义）

每个代理在其狭隘任务上比通才更出色。`/slide-excellence` 技能以并行方式运行它们所有。

### 对抗性 QA

两个代理相互对立工作：**批评者** 阅读 Beamer 和 Quarto 并产生严厉的发现。**修复者** 准确实现批评者发现的内容。他们循环直到批评者说"已批准"（或最多 5 轮）。这捕捉单次审查遗漏的错误。

### 质量门控

每个文件都有评分（0-100）。低于阈值的评分会阻止操作：
- **80** — 提交阈值
- **90** — PR 阈值
- **95** — 卓越（志向性目标）

---

## 指南

有关全面演练，请阅读 **[完整指南](https://psantanna.com/claude-code-my-workflow/workflow-guide.html)**（或查看 [源代码](guide/workflow-guide.qmd)）。

它涵盖：
1. **工作流存在的原因** — 问题和愿景
2. **入门** — Fork、粘贴一个提示，Claude 处理其余部分
3. **系统实际应用** — 专门代理、对抗性 QA、质量评分
4. **构建块** — CLAUDE.md、规则、技能、代理、钩子、记忆
5. **工作流模式** — 讲座创建、翻译、复制、多代理审查、研究探索
6. **为你的领域定制** — 创建你自己的审查者和知识库

---

## 包含内容

<details>
<summary><strong>10 个代理、19 个技能、17 个规则、4 个钩子</strong>（点击展开）</summary>

### 代理（`.claude/agents/`）

| 代理 | 功能 |
|-------|-------------|
| `proofreader` | 语法、拼写错误、溢出、一致性审查 |
| `slide-auditor` | 视觉布局审计（溢出、字体一致性、间距） |
| `pedagogy-reviewer` | 13 模式教学评审（叙述弧、符号密度、步调） |
| `python-reviewer` | Python 代码质量、可重现性和领域正确性 |
| `tikz-reviewer` | 严厉的 TikZ 图表视觉批评 |
| `beamer-translator` | Beamer 转 Quarto 翻译专家 |
| `quarto-critic` | 对抗性 QA，将 Quarto 与 Beamer 基准进行比较 |
| `quarto-fixer` | 实现批评者代理的修复 |
| `verifier` | 端到端任务完成验证 |
| `domain-reviewer` | **模板** 用于你的领域特定物质审查者 |

### 技能（`.claude/skills/`）

| 技能 | 功能 |
|-------|-------------|
| `/compile-latex` | 3 遍 XeLaTeX 编译及 bibtex |
| `/deploy` | 渲染 Quarto + 同步到 GitHub Pages |
| `/extract-tikz` | TikZ 图表转 PDF 再转 SVG 管道 |
| `/proofread` | 启动校对员处理文件 |
| `/visual-audit` | 启动幻灯片审计员处理文件 |
| `/pedagogy-review` | 启动教学审查者处理文件 |
| `/review-python` | 启动 Python 代码审查者 |
| `/qa-quarto` | 对抗性批评者-修复者循环（最多 5 轮） |
| `/slide-excellence` | 组合多代理审查 |
| `/translate-to-quarto` | 完整的 11 阶段 Beamer 转 Quarto 翻译 |
| `/validate-bib` | 对文献目录验证交叉引用 |
| `/devils-advocate` | 在提交前质疑设计决策 |
| `/create-lecture` | 完整讲座创建工作流 |
| `/commit` | 暂存、提交、创建 PR 并合并到 main |
| `/lit-review` | 文献搜索、综合和差距识别 |
| `/research-ideation` | 生成研究问题和实证策略 |
| `/interview-me` | 交互式采访以正式化研究思想 |
| `/review-paper` | 稿件审查：结构、计量经济学、审稿人异议 |
| `/data-analysis` | 端到端 Python 分析及出版就绪输出 |

### 研究工作流

| 特性 | 功能 |
|---------|-------------|
| 探索文件夹 | 带有研究生/存档生命周期的结构化 `explorations/` 沙箱 |
| 快速轨道工作流 | 60/100 质量阈值用于快速原型设计 |
| 简化编排器 | 实现 → 验证 → 评分 → 完成（无多轮审查） |
| 增强会话日志 | 结构化的更改、决策、验证表 |
| 仅合并报告 | 仅在合并时生成质量报告 |
| 数学行长例外 | 长行对于有记录的公式可接受 |
| 工作流快速参考 | `.claude/WORKFLOW_QUICK_REF.md` 中的单页速查表 |

### 规则（`.claude/rules/`）

规则使用路径作用域加载：**始终启用** 规则每次会话加载（约 100 行总计）；**路径作用域** 规则仅在 Claude 处理匹配文件时加载。Claude 可靠地遵循约 150 条指令，所以越少越好。

**始终启用**（无 `paths:` frontmatter — 每次会话加载）：

| 规则 | 强制执行内容 |
|------|-----------------|
| `plan-first-workflow` | 非平凡任务的规划模式 + 上下文保留 |
| `orchestrator-protocol` | 承包商模式：实现 → 验证 → 审查 → 修复 → 评分 |
| `session-logging` | 三个日志触发点：后规划、增量、会话结束 |

**路径作用域**（仅在处理匹配文件时加载）：

| 规则 | 触发于 | 强制执行内容 |
|------|------------|-----------------|
| `verification-protocol` | `.tex`, `.qmd`, `docs/` | 任务完成清单 |
| `single-source-of-truth` | `Figures/`, `.tex`, `.qmd` | 无内容重复；Beamer 是权威 |
| `quality-gates` | `.tex`, `.qmd`, `*.py` | 80/90/95 评分 + 容差阈值 |
| `python-code-conventions` | `*.py` | Python 编码标准 + 数学行长例外 |
| `tikz-visual-quality` | `.tex` | TikZ 图表视觉标准 |
| `beamer-quarto-sync` | `.tex`, `.qmd` | 自动同步 Beamer 编辑到 Quarto |
| `pdf-processing` | `master_supporting_docs/` | 安全的大 PDF 处理 |
| `proofreading-protocol` | `.tex`, `.qmd`, `quality_reports/` | 提议-首先，然后在批准后应用 |
| `no-pause-beamer` | `.tex` | Beamer 中无覆盖命令 |
| `replication-protocol` | `*.py` | 复制原始结果后再扩展 |
| `knowledge-base-template` | `.tex`, `.qmd`, `*.py` | 符号/应用注册表模板 |
| `orchestrator-research` | `*.py`, `explorations/` | 简化研究编排器（无多轮审查） |
| `exploration-folder-protocol` | `explorations/` | 实验工作的结构化沙箱 |
| `exploration-fast-track` | `explorations/` | 轻量级探索工作流（60/100 阈值） |

**模板**（`templates/`） — 会话日志、质量报告和探索 README 的参考格式。不会自动加载。

</details>

---

## 前置条件

| 工具 | 需要用于 | 安装 |
|------|-------------|---------|
| [Claude Code](https://code.claude.com/docs/en/overview) | 所有 | `npm install -g @anthropic-ai/claude-code` |
| XeLaTeX | LaTeX 编译 | [TeX Live](https://tug.org/texlive/) 或 [MacTeX](https://tug.org/mactex/) |
| [Quarto](https://quarto.org) | 网络幻灯片 | [quarto.org/docs/get-started](https://quarto.org/docs/get-started/) |
| Python | 图形和分析 | [python.org](https://www.python.org/) |
| NanoBanana | 科研绘图（可选） | `pip install nano-banana` |
| pdf2svg | TikZ 转 SVG | `brew install pdf2svg` (macOS) |
| [gh CLI](https://cli.github.com/) | PR 工作流 | `brew install gh` (macOS) |

并非所有工具都是必需的 — 仅安装你的项目使用的工具。Claude Code 是唯一的硬性要求。

---

## 为你的领域调整

1. **填写知识库**（`.claude/rules/knowledge-base-template.md`）— 添加你的符号、应用和设计原则
2. **自定义领域审查者**（`.claude/agents/domain-reviewer.md`）— 使用特定于你的领域的审查视角
3. **更新调色板** — 在你的 Quarto 主题 SCSS 文件中改变顶部的颜色变量
4. **添加领域特定的 Python 陷阱** — 到 `.claude/rules/python-code-conventions.md`
5. **填写讲座映射** — 在 `.claude/rules/beamer-quarto-sync.md` 中
6. **自定义工作流快速参考** — 使用 `.claude/WORKFLOW_QUICK_REF.md` 中的你的不可协商原则和偏好
7. **设置探索文件夹** — 用于实验工作的 (`explorations/`)

---

## 其他资源

- [Claude Code 文档](https://code.claude.com/docs/en/overview)
- [编写优秀的 CLAUDE.md](https://code.claude.com/docs/en/memory) — 关于项目记忆的官方指南

---

## 来源

该基础设施从埃默里大学的 **Econ 730：因果面板数据** 课程中提取，由 Pedro Sant'Anna 使用 Claude Code 在 6+ 个会话中开发。该课程产生了 6 个完整的博士讲座套装，包含 800+ 幻灯片、包含 plotly 图表的交互式 Quarto 版本以及完整的 Python 复制包 — 所有这些都通过此多代理工作流进行管理。

---

## 许可证

MIT 许可证。自由用于教学、研究或任何学术目的。
