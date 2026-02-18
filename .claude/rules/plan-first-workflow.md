# 计划优先工作流

**对于任何非平凡的任务，在编写代码之前进入计划模式。**

## 协议

1. **进入计划模式** — 使用 `EnterPlanMode`
2. **检查 MEMORY.md** — 阅读任何与此任务相关的 `[LEARN]` 条目
3. **起草计划** — 哪些变更、哪些文件、顺序如何
4. **保存到磁盘** — 写入 `quality_reports/plans/YYYY-MM-DD_short-description.md`
5. **呈现给用户** — 等待批准
6. **退出计划模式** — 仅在获得批准后
7. **保存初始会话日志** — 在新鲜时捕获目标和关键背景
8. **通过协调器实现** — 见 `orchestrator-protocol.md`

## 磁盘上的计划

计划在上下文压缩后仍然存在。将每个计划保存到：

```
quality_reports/plans/YYYY-MM-DD_short-description.md
```

格式：状态（DRAFT/APPROVED/COMPLETED）、方法、要修改的文件、验证步骤。

## 上下文管理

- 更喜欢自动压缩而不是 `/clear`
- 在重要上下文丢失前将其保存到磁盘
- 仅当上下文确实被污染时才使用 `/clear`

## 会话恢复

在压缩或新会话后：
1. 阅读 `CLAUDE.md` + `quality_reports/plans/` 中最新的计划
2. 检查 `git log --oneline -10` 和 `git diff`
3. 陈述你对当前任务的理解
