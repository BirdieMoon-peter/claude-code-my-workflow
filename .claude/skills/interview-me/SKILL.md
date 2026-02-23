---
name: interview-me
description: 通过互动访谈将研究想法形式化为包含假设和实证策略的结构化规范
disable-model-invocation: true
argument-hint: "[简短主题或'从头开始']"
allowed-tools: ["Read", "Write"]
---

# 研究访谈

进行结构化访谈，帮助将研究想法形式化为具体规范。

**输入：** `$ARGUMENTS` — 简要的主题描述或 "start fresh" 进行开放式探索。

---

## 工作原理

这是一个**对话式**技能。你不会立即生成报告，而是通过逐个提问的方式进行访谈，根据回答深入探究，并逐步构建结构化的研究规范。

**不要使用 AskUserQuestion。** 在文本回复中直接提问，一次一到两个问题。等待用户回复后再继续。

---

## 访谈结构

### 阶段 1：全局视角（1-2 个问题）
- "你试图理解什么现象或困惑？"
- "为什么这很重要？谁应该关心答案？"

### 阶段 2：理论动机（1-2 个问题）
- "你对 X 为什么发生 / 什么驱动 Y 的直觉是什么？"
- "标准理论会预测什么？你期望不同的结果吗？"

### 阶段 3：数据和背景（1-2 个问题）
- "你可以访问什么数据，或者理想情况下你想要什么数据？"
- "你关注的是特定的背景、时间段还是制度环境？"

### 阶段 4：识别策略（1-2 个问题）
- "是否存在你可以利用的自然实验、政策变化或变异来源？"
- "对因果解释的最大威胁是什么？"

### 阶段 5：预期结果（1-2 个问题）
- "你期望发现什么？什么会让你感到惊讶？"
- "结果对政策或理论意味着什么？"

### 阶段 6：贡献（1 个问题）
- "这与已完成的工作有何不同？你要填补什么空白？"

---

## 访谈之后

一旦你有足够的信息（通常 5-8 次交流），生成一个**研究规范文档**：

```markdown
# Research Specification: [Title]

**Date:** [YYYY-MM-DD]
**Researcher:** [from conversation context]

## Research Question

[Clear, specific question in one sentence]

## Motivation

[2-3 paragraphs: why this matters, theoretical context, policy relevance]

## Hypothesis

[Testable prediction with expected direction]

## Empirical Strategy

- **Method:** [e.g., Difference-in-Differences with staggered adoption]
- **Treatment:** [What varies]
- **Control:** [Comparison group]
- **Key identifying assumption:** [What must hold]
- **Robustness checks:** [Pre-trends, placebo tests, etc.]

## Data

- **Primary dataset:** [Name, source, coverage]
- **Key variables:** [Treatment, outcome, controls]
- **Sample:** [Unit of observation, time period, N]

## Expected Results

[What the researcher expects to find and why]

## Contribution

[How this advances the literature — 2-3 sentences]

## Open Questions

[Issues raised during the interview that need further thought]
```

**保存到：** `quality_reports/research_spec_[sanitized_topic].md`

---

## 访谈风格

- **保持好奇，而非指导。** 你的工作是引出研究者的思考，而不是强加自己的想法。
- **温和地探查薄弱环节。** 如果识别策略听起来很脆弱，问"怀疑者会怎么说...？"而不是"这不会起作用，因为..."
- **基于回答进行构建。** 每个问题都应该跟随前一个回复。
- **知道何时停止。** 如果研究者在 4-5 次交流后有了清晰的愿景，就转向规范。不要过度访谈。
