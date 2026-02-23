---
paths:
  - "scripts/**/*.py"
  - "Figures/**/*.py"
---

# Replication-First Protocol

**核心原则:** 在扩展之前，先将原始结果准确复制。

---

## Phase 1: Inventory & Baseline

在编写任何Python代码之前：

- [ ] 阅读论文的复制说明 (README)
- [ ] 清点复制包：编程语言、数据文件、脚本、输出
- [ ] 记录论文中的黄金标准数字：

```markdown
## Replication Targets: [Paper Author (Year)]

| Target | Table/Figure | Value | SE/CI | Notes |
|--------|-------------|-------|-------|-------|
| Main ATT | Table 2, Col 3 | -1.632 | (0.584) | Primary specification |
```

- [ ] 将目标存储在 `quality_reports/LectureNN_replication_targets.md` 或作为 pickle/JSON

---

## Phase 2: Translate & Execute

- [ ] 遵循 `python-code-conventions.md` 规范
- [ ] 逐行初步翻译 -- 复制期间不要"改进"
- [ ] 完全匹配原始规范（协变量、样本、聚类、SE计算）
- [ ] 将所有中间结果保存为 pickle/JSON

### Stata to Python Translation Pitfalls

<!-- Customize: Add pitfalls specific to your field -->

| Stata | Python | Trap |
|-------|--------|------|
| `reg y x, cluster(id)` | `from linearmodels.panel import FirstDifferenceOLS` or `statsmodels.formula.api.ols()` with `cov_type='cluster'` | Stata and Python compute clustered SE differently; verify degrees of freedom adjustment |
| `areg y x, absorb(id)` | `from linearmodels.panel import FirstDifferenceOLS` or use `pd.get_dummies()` for fixed effects | Demeaning method must match (Frisch-Waugh-Lovell vs. within transformation) |
| `probit` for PS | `from statsmodels.genmod.generalized_linear_model import GLM; GLM(..., family=sm.families.Binomial(link=sm.genmod.cov_struct.Probit()))` | Python default link != Stata default in some cases |
| `bootstrap, reps(999)` | `from scipy.stats import bootstrap` or manual resampling | Match seed, reps, and bootstrap type (percentile vs. bias-corrected) exactly |

---

## Phase 3: Verify Match

### Tolerance Thresholds

| Type | Tolerance | Rationale |
|------|-----------|-----------|
| Integers (N, counts) | Exact match | No reason for any difference |
| Point estimates | < 0.01 | Rounding in paper display |
| Standard errors | < 0.05 | Bootstrap/clustering variation |
| P-values | Same significance level | Exact p may differ slightly |
| Percentages | < 0.1pp | Display rounding |

### If Mismatch

**不要继续扩展。** 隔离引入差异的步骤，检查常见原因（样本大小、SE计算、默认选项、变量定义），并记录调查过程，即使未解决。

### Replication Report

保存到 `quality_reports/LectureNN_replication_report.md`:

```markdown
# Replication Report: [Paper Author (Year)]
**Date:** [YYYY-MM-DD]
**Original language:** [Stata/R/etc.]
**Python translation:** [script path]

## Summary
- **Targets checked / Passed / Failed:** N / M / K
- **Overall:** [REPLICATED / PARTIAL / FAILED]

## Results Comparison

| Target | Paper | Ours | Diff | Status |

## Discrepancies (if any)
- **Target:** X | **Investigation:** ... | **Resolution:** ...

## Environment
- Python version, key packages (with versions), data source
```

---

## Phase 4: Only Then Extend

复制验证后（所有目标通过）：

- [ ] 提交复制脚本: "Replicate [Paper] Table X -- all targets match"
- [ ] 现在用课程特定的修改进行扩展（不同的估计器、新图表等）
- [ ] 每个扩展都建立在已验证的基线之上
