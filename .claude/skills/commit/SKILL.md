---
name: commit
description: 暂存、提交、创建 PR 并合并到 main。用于标准的提交-PR-合并周期。
disable-model-invocation: true
argument-hint: "[可选：提交消息]"
allowed-tools: ["Bash", "Read", "Glob"]
---

# 提交、PR 和合并

暂存更改、使用描述性消息提交、创建 PR 并合并到 main。

## 步骤

1. **检查当前状态：**

```bash
git status
git diff --stat
git log --oneline -5
```

2. **从当前状态创建一个分支：**

```bash
git checkout -b <short-descriptive-branch-name>
```

3. **暂存文件** — 添加特定文件（永远不要使用 `git add -A`）：

```bash
git add <file1> <file2> ...
```

不要暂存 `.claude/settings.local.json` 或包含机密的任何文件。

4. **使用描述性消息提交：**

如果提供了 `$ARGUMENTS`，将其用作提交消息。否则，分析暂存的更改并写一条解释*为什么*而不仅仅是*什么*的消息。

```bash
git commit -m "$(cat <<'EOF'
<commit message here>
EOF
)"
```

5. **推送并创建 PR：**

```bash
git push -u origin <branch-name>
gh pr create --title "<short title>" --body "$(cat <<'EOF'
## 摘要
<1-3 要点>

## 测试计划
<清单>

🤖 使用 [Claude Code](https://claude.com/claude-code) 生成
EOF
)"
```

6. **合并并清理：**

```bash
gh pr merge <pr-number> --merge --delete-branch
git checkout main
git pull
```

7. **报告** PR URL 和合并的内容。

## 重要

- 始终创建一个新分支 — 永远不要直接提交到 main
- 从暂存中排除 `settings.local.json` 和敏感文件
- 使用 `--merge`（不是 `--squash` 或 `--rebase`），除非另有要求
- 如果从 `$ARGUMENTS` 提供了提交消息，请按原样使用它
