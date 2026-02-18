# 探索

此文件夹是**沙箱**，用于实验性和探索性工作。所有新想法、原型和研究实验首先进行这里——从不直接进入生产文件夹。

## 工作方式

1. **为每个探索创建子文件夹**（例如，`explorations/new-estimator/`）
2. **自由工作**——探索期间质量阈值较低（60/100）
3. **决定**：毕业到生产环境（需要 80/100）、继续探索或归档

## 规则

- 查看 `.claude/rules/exploration-folder-protocol.md` 了解完整协议
- 查看 `.claude/rules/exploration-fast-track.md` 了解轻量级工作流

## 结构

```
explorations/
├── [active-project]/       # 进行中的工作
│   ├── README.md           # 目标、假设、状态
│   ├── src/                # 实验代码
│   ├── scripts/            # 测试脚本
│   └── output/             # 结果
└── ARCHIVE/                # 已完成或已放弃
    ├── completed_[name]/   # 毕业到生产环境
    └── abandoned_[name]/   # 记录了停止的原因
```
