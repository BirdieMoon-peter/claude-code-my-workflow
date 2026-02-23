# CLAUDE.MD -- 使用Claude Code进行学术论文写作

<!-- 使用方法：将[括号中的占位符]替换为你的项目信息。
     自定义Beamer环境以适应你的主题。
     保持此文件在~150行以内 — Claude会在每个会话中加载它。
     完整工作流说明见 README.md。 -->

**项目:** [YOUR PROJECT NAME]
**机构:** [YOUR INSTITUTION]
**分支:** main

---

## 核心原则

- **优先规划** -- 在执行非平凡任务之前进入规划模式；将规划保存到 `quality_reports/plans/`
- **事后验证** -- 在每项任务结束时编译并确认输出
- **单一真实来源** -- Beamer `.tex` 和 LaTeX 论文 `.tex` 是所有内容的权威来源
- **质量门槛** -- 任何内容发布质量分数不低于80/100
- **[LEARN] 标签** -- 被修正时，将 `[LEARN:category] 错误 → 正确` 保存到 MEMORY.md

---

## 文件夹结构

```
[YOUR-PROJECT]/
├── CLAUDE.md                    # 本文件
├── .claude/                     # 规则、技能、代理、钩子
├── Bibliography_base.bib        # 中央文献库
├── Figures/                     # 图表和图像（PDF/PNG，300dpi）
├── Preambles/header.tex         # LaTeX 共用头文件
├── Papers/                      # LaTeX 学术论文 .tex 文件
├── Slides/                      # Beamer 演示文稿 .tex 文件
├── scripts/                     # Python 分析脚本
├── quality_reports/             # 计划、会话日志、审查报告
├── explorations/                # 研究沙箱（实验性工作）
├── templates/                   # 会话日志、质量报告模板
└── master_supporting_docs/      # 参考论文和现有幻灯片
```

---

## 命令

```bash
# 编译 Beamer 幻灯片（3遍，仅限XeLaTeX）
cd Slides && TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
BIBINPUTS=..:$BIBINPUTS bibtex file
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex

# 编译 LaTeX 论文（3遍，仅限XeLaTeX）
cd Papers && TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode paper.tex
BIBINPUTS=..:$BIBINPUTS bibtex paper
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode paper.tex
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode paper.tex

# 质量评分
python scripts/quality_score.py Papers/paper.tex
```

---

## 质量门槛

| 分数 | 门槛 | 含义 |
|-------|------|---------|
| 80 | 提交 | 足以保存 |
| 90 | PR | 准备部署 |
| 95 | 卓越 | 理想目标 |

---

## 技能速查参考

| 命令 | 功能描述 |
|---------|-------------|
| `/compile-latex [file]` | 3遍 XeLaTeX + bibtex |
| `/extract-tikz [file]` | TikZ → PDF → SVG |
| `/proofread [file]` | 语法/拼写/溢出审查 |
| `/visual-audit [file]` | 幻灯片版面审计 |
| `/pedagogy-review [file]` | 叙述、符号、进度审查 |
| `/review-python [file]` | Python代码质量审查 |
| `/slide-excellence [file]` | 综合多代理幻灯片审查 |
| `/validate-bib` | 交叉参考引文 |
| `/devils-advocate` | 挑战设计决策 |
| `/create-lecture` | 完整 Beamer 讲义创建 |
| `/commit [msg]` | 暂存、提交、PR、合并 |
| `/lit-review [topic]` | 文献搜索 + 综合 |
| `/research-ideation [topic]` | 研究问题 + 实证策略 |
| `/interview-me [topic]` | 交互式研究访谈 |
| `/review-paper [file]` | 论文审查（结构、识别、计量） |
| `/data-analysis [dataset]` | 端到端 Python 分析 |

---

<!-- 自定义：将下面的示例条目替换为你的 Beamer 环境。 -->

## Beamer 自定义环境

| 环境 | 效果 | 使用场景 |
|-------------------|---------------|----------------|
| `[your-env]` | [描述] | [何时使用] |

<!-- 示例条目 (删除并替换为你的):
| `keybox` | 金色背景框 | 关键点 |
| `highlightbox` | 金色左侧强调框 | 高亮内容 |
| `definitionbox[Title]` | 蓝色边框标题框 | 正式定义 |
-->

---

## 当前项目状态

| 文件 | 类型 | 关键内容 |
|------|------|---------|
| `Papers/[PaperName].tex` | 论文 | [简要描述] |
| `Slides/[Topic].tex` | Beamer | [简要描述] |
