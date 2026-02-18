---
paths:
  - "explorations/**"
---

# 探索文件夹协议

**所有实验工作首先进入 `explorations/` 文件夹。** 永远不要直接进入生产文件夹。

## 文件夹结构

```
explorations/
├── ACTIVE_PROJECTS.md
├── [project]/
│   ├── README.md          # 目标、状态、发现
│   ├── src/               # 代码 (使用 _v1, _v2 表示迭代)
│   ├── scripts/           # 测试脚本
│   ├── output/            # 结果
│   └── SESSION_LOG.md     # 进度笔记
└── ARCHIVE/
    ├── completed_[project]/
    └── abandoned_[project]/
```

## 生命周期

1. **创建** -- `mkdir -p explorations/[name]/{src,scripts,output}` + README 来自 `templates/exploration-readme.md`
2. **开发** -- 完全在探索文件夹内工作
3. **决定:**

   - **毕业到生产** -- 复制到 `src/`、`scripts/`；需要质量 >= 80、所有测试通过、代码清晰。移动到 `ARCHIVE/completed_[project]/`
   - **继续探索** -- 在 README 中记录后续步骤
   - **放弃** -- 移动到 `ARCHIVE/abandoned_[project]/` 并附上解释 (使用 `templates/archive-readme.md`)

## 毕业检查清单

- [ ] 质量分数 >= 80
- [ ] 所有测试通过
- [ ] 结果在容差范围内复现
- [ ] 代码清晰易懂
- [ ] README 解释方法和发现
