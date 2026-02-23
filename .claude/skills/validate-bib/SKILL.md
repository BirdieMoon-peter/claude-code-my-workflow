---
name: validate-bib
description: 针对所有讲义文件中的引文验证参考文献条目。查找缺失的条目和未使用的参考。
disable-model-invocation: true
allowed-tools: ["Read", "Grep", "Glob"]
---

# 验证参考文献

交叉参考讲义文件中的所有引文和参考文献条目。

## 步骤

1. **读取参考文献文件**并提取所有引文密钥

2. **扫描所有讲义文件中的引文密钥：**
   - `.tex` 文件：查找 `\cite{`、`\citet{`、`\citep{`、`\citeauthor{`、`\citeyear{`
   - `.qmd` 文件：查找 `@key`、`[@key]`、`[@key1; @key2]`
   - 提取所有使用的唯一引文密钥

3. **交叉参考：**
   - **缺失条目：**讲义中使用但不在参考文献中的引文
   - **未使用条目：**参考文献中未在任何地方被引用的条目
   - **可能的拼写错误：**类似但不匹配的密钥

4. **检查每个参考文献条目的质量：**
   - 所需字段是否存在（author、title、year、journal/booktitle）
   - Author 字段格式是否正确
   - 年份是否合理
   - 是否有格式错误的字符或编码问题

5. **报告发现：**
   - 缺失的参考文献条目列表（关键）
   - 未使用条目的列表（信息性）
   - 引文密钥中可能拼写错误的列表
   - 质量问题的列表

## 要扫描的文件：
```
Slides/*.tex
Quarto/*.qmd
```

## 参考文献位置：
```
Bibliography_base.bib  (repo root)
```
