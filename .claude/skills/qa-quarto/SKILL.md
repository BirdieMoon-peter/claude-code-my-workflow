---
name: qa-quarto
description: 对抗性QA工作流，将Quarto HTML与Beamer PDF基准进行比较。在评审员（发现问题）和修复员（应用修复）之间迭代，直到批准或达到最大迭代次数。
disable-model-invocation: true
argument-hint: "[LectureN, 例如 Lecture1]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Edit", "Bash", "Task"]
---

# 对抗性 Quarto vs Beamer QA 工作流

使用迭代式评审/修复循环将 Quarto HTML 幻灯片与其 Beamer PDF 基准进行比较。

**理念:** Beamer PDF 是黄金标准。Quarto 译本在所有维度上都必须至少一样好。

---

## 工作流

```
阶段0：预检 → 阶段1：评审员审核 → 阶段2：修复员 → 阶段3：重新审核 → 循环直到批准（最多5轮）
```

## 硬性门控（不可协商）

| 门控 | 条件 |
|------|-----------|
| **溢出** | 没有内容被截断 |
| **图表质量** | 交互式图表 >= 静态图表 |
| **内容对等** | 没有缺失的幻灯片/公式/文本 |
| **视觉回归** | Quarto 在所有维度上 >= Beamer |
| **幻灯片居中** | 内容居中，没有跳跃 |
| **符号保真度** | 所有数学公式逐字来自 Beamer |

## 阶段 0: 预检

1. 定位 Beamer (.tex/.pdf) 和 Quarto (.qmd/.html) 文件
2. 检查新鲜度（如果 QMD 比 HTML 新，则重新渲染）
3. 如适用，验证 TikZ SVG

## 阶段 1: 初始审核

启动 `quarto-critic` 智能体全面比较 Beamer vs Quarto。报告保存到 `quality_reports/[Lecture]_qa_critic_round1.md`。

## 阶段 2: 修复循环

如果未批准，启动 `quarto-fixer` 智能体应用修复（关键 → 主要 → 次要），重新渲染并验证。

## 阶段 3: 重新审核

重新启动评审员验证修复。如需要，循环回阶段 2。

## 迭代限制

最多 5 轮修复。之后，将剩余问题上报给用户。

## 最终报告

保存到 `quality_reports/[Lecture]_qa_final.md`，包含硬性门控状态、迭代摘要和剩余问题。
