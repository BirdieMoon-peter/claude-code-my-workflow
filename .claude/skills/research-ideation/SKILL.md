---
name: research-ideation
description: 从主题或数据集生成结构化的研究问题、可检验的假设和实证策略
disable-model-invocation: true
argument-hint: "[主题、现象或数据集描述]"
allowed-tools: ["Read", "Grep", "Glob", "Write"]
---

# 研究构想

从主题、现象或数据集生成结构化的研究问题、可检验的假设和实证策略。

**输入：** `$ARGUMENTS` — 一个主题（例如："最低工资对就业的影响"）、一个现象（例如："为什么企业会地理集聚？"）或数据集描述（例如："2000-2020年美国县级污染和健康结果面板数据"）。

---

## 步骤

1. **理解输入。** 阅读 `$ARGUMENTS` 和任何引用的文件。检查 `master_supporting_docs/` 查找相关论文。检查 `.claude/rules/` 了解领域惯例。

2. **生成3-5个研究问题**，从描述性到因果性排序：
   - **描述性：** 有什么模式？（例如："X如何随时间演变？"）
   - **相关性：** 哪些因素相关联？（例如："在控制Z后，X与Y是否相关？"）
   - **因果性：** 效应是什么？（例如："X对Y的因果效应是什么？"）
   - **机制：** 为什么存在这种效应？（例如："X通过什么渠道影响Y？"）
   - **政策：** 有什么含义？（例如："政策X会改善结果Y吗？"）

3. **对每个研究问题，发展：**
   - **假设：** 具有预期符号/幅度的可检验预测
   - **识别策略：** 如何建立因果关系（DiD、IV、RDD、synthetic control等）
   - **数据需求：** 需要什么数据？是否可获得？
   - **关键假设：** 策略有效需要满足什么条件？
   - **潜在陷阱：** 识别的常见威胁
   - **相关文献：** 使用类似方法的2-3篇论文

4. **对问题进行排序**，按可行性和贡献度。

5. **保存输出** 到 `quality_reports/research_ideation_[sanitized_topic].md`

---

## 输出格式

```markdown
# 研究构想: [Topic]

**日期：** [YYYY-MM-DD]
**输入：** [Original input]

## 概述

[1-2段文字说明主题定位及其重要性]

## 研究问题

### RQ1: [Question] (可行性: High/Medium/Low)

**类型：** Descriptive / Correlational / Causal / Mechanism / Policy

**假设：** [Testable prediction]

**识别策略：**
- **方法：** [e.g., Difference-in-Differences]
- **处理：** [What varies and when]
- **控制组：** [Comparison units]
- **关键假设：** [e.g., Parallel trends]

**数据需求：**
- [Dataset 1 — what it provides]
- [Dataset 2 — what it provides]

**潜在陷阱：**
1. [Threat 1 and possible mitigation]
2. [Threat 2 and possible mitigation]

**相关研究：** [Author (Year)], [Author (Year)]

---

[对RQ2-RQ5重复]

## 排序

| RQ | 可行性 | 贡献度 | 优先级 |
|----|-------------|-------------|----------|
| 1  | High        | Medium      | ...      |
| 2  | Medium      | High        | ...      |

## 建议的下一步

1. [最有希望的方向和即时行动]
2. [要获取的数据]
3. [需要深入审查的文献]
```

---

## 原则

- **具有创造性但要有依据。** 超越显而易见的问题，但每个建议都必须在实证上可行。
- **像审稿人一样思考。** 对于每个因果问题，立即识别识别挑战。
- **考虑数据可获得性。** 一个没有可用数据的绝妙问题是不可操作的。
- **建议具体的数据集**，在可能的情况下（FRED、Census、PSID、行政数据等）。
