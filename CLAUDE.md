# CLAUDE.MD -- 使用Claude Code进行学术项目开发

<!-- 使用方法：将[括号中的占位符]替换为你的项目信息。
     自定义Beamer环境和CSS类以适应你的主题。
     保持此文件在~150行以内 — Claude会在每个会话中加载它。
     完整文档见 docs/workflow-guide.html。 -->

**项目:** [YOUR PROJECT NAME]
**机构:** [YOUR INSTITUTION]
**分支:** main

---

## 核心原则

- **优先规划** -- 在执行非平凡任务之前进入规划模式；将规划保存到 `quality_reports/plans/`
- **事后验证** -- 在每项任务结束时编译/渲染并确认输出
- **单一真实来源** -- Beamer `.tex` 是权威来源；Quarto `.qmd` 从其派生
- **质量门槛** -- 任何内容发布质量分数不低于80/100
- **[LEARN] 标签** -- 被修正时，将 `[LEARN:category] 错误 → 正确` 保存到 MEMORY.md

---

## 文件夹结构

```
[YOUR-PROJECT]/
├── CLAUDE.MD                    # 本文件
├── .claude/                     # 规则、技能、代理、钩子
├── Bibliography_base.bib        # 中央文献库
├── Figures/                     # 图表和图像
├── Preambles/header.tex         # LaTeX 头文件
├── Slides/                      # Beamer .tex 文件
├── Quarto/                      # RevealJS .qmd 文件 + 主题
├── docs/                        # GitHub Pages (自动生成)
├── scripts/                     # 实用脚本 + Python代码
├── quality_reports/             # 计划、会话日志、合并报告
├── explorations/                # 研究沙箱 (参见规则)
├── templates/                   # 会话日志、质量报告模板
└── master_supporting_docs/      # 论文和现有幻灯片
```

---

## 命令

```bash
# LaTeX (3遍，仅限XeLaTeX)
cd Slides && TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
BIBINPUTS=..:$BIBINPUTS bibtex file
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex
TEXINPUTS=../Preambles:$TEXINPUTS xelatex -interaction=nonstopmode file.tex

# 将Quarto部署到GitHub Pages
./scripts/sync_to_docs.sh LectureN

# 质量分数
python scripts/quality_score.py Quarto/file.qmd
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
| `/deploy [LectureN]` | 渲染 Quarto + 同步到 docs/ |
| `/extract-tikz [LectureN]` | TikZ → PDF → SVG |
| `/proofread [file]` | 语法/拼写/溢出审查 |
| `/visual-audit [file]` | 幻灯片版面审计 |
| `/pedagogy-review [file]` | 叙述、符号、进度审查 |
| `/review-python [file]` | Python代码质量审查 |
| `/qa-quarto [LectureN]` | 对抗性 Quarto vs Beamer 质量保证 |
| `/slide-excellence [file]` | 综合多代理审查 |
| `/translate-to-quarto [file]` | Beamer → Quarto 翻译 |
| `/validate-bib` | 交叉参考引文 |
| `/devils-advocate` | 挑战幻灯片设计 |
| `/create-lecture` | 完整讲座创建 |
| `/commit [msg]` | 暂存、提交、PR、合并 |
| `/lit-review [topic]` | 文献搜索 + 综合 |
| `/research-ideation [topic]` | 研究问题 + 策略 |
| `/interview-me [topic]` | 交互式研究访谈 |
| `/review-paper [file]` | 论文审查 |
| `/data-analysis [dataset]` | 端到端 Python 分析 |

---

<!-- 自定义：将下面的示例条目替换为你的
     Beamer环境和Quarto CSS类。这些是
     原始项目中的示例 — 删除它们并添加你的。 -->

## Beamer 自定义环境

| 环境 | 效果 | 使用场景 |
|-------------------|---------------|----------------|
| `[your-env]` | [描述] | [何时使用] |

<!-- 示例条目 (删除并替换为你的):
| `keybox` | 金色背景框 | 关键点 |
| `highlightbox` | 金色左侧强调框 | 高亮内容 |
| `definitionbox[Title]` | 蓝色边框标题框 | 正式定义 |
-->

## Quarto CSS 类

| 类 | 效果 | 使用场景 |
|--------------------|---------------|----------------|
| `[.your-class]` | [描述] | [何时使用] |

<!-- 示例条目 (删除并替换为你的):
| `.smaller` | 85% 字体 | 密集内容幻灯片 |
| `.positive` | 绿色加粗 | 正面注释 |
-->

---

## 当前项目状态

| 讲座 | Beamer | Quarto | 关键内容 |
|---------|--------|--------|-------------|
| 1: [主题] | `Lecture01_Topic.tex` | `Lecture1_Topic.qmd` | [简要描述] |
| 2: [主题] | `Lecture02_Topic.tex` | -- | [简要描述] |
