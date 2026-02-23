---
paths:
  - "scripts/**/*.py"
  - "explorations/**"
  - "Figures/**/*.py"
---

# Research Project Orchestrator (Simplified)

**对于Python脚本、仿真和数据分析** -- 使用这个简化的循环而不是完整的多代理编排器。

## The Simple Loop

```
Plan approved → orchestrator activates
  │
  Step 1: IMPLEMENT — Execute plan steps
  │
  Step 2: VERIFY — Run code, check outputs
  │         Python脚本: 无错误运行
  │         Simulations: 随机种子 reproducibility
  │         Plots: PDF/PNG created, correct format
  │         If verification fails → fix → re-verify
  │
  Step 3: SCORE — Apply quality-gates rubric
  │
  └── Score >= 80?
        YES → Done (commit when user signals)
        NO  → Fix blocking issues, re-verify, re-score
```

**No 5-round loops. No multi-agent reviews. Just: write, test, done.**

## Verification Checklist

- [ ] Script runs without errors
- [ ] All packages imported at top
- [ ] No hardcoded absolute paths
- [ ] `np.random.seed()` once at top if stochastic
- [ ] Output files created at expected paths
- [ ] Tolerance checks pass (if applicable)
- [ ] Quality score >= 80
