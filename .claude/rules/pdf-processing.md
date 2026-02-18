---
paths:
  - "master_supporting_docs/**"
---

# 稳健的 PDF 处理

## 安全处理工作流

**步骤 1: 接收 PDF 上传**
- 用户上传 PDF 到 `master_supporting_docs/supporting_papers/` 或 `supporting_slides/`
- Claude 不会尝试直接读取

**步骤 2: 检查 PDF 属性**
```bash
pdfinfo paper_name.pdf | grep "Pages:"
ls -lh paper_name.pdf
```

**步骤 3: 创建子文件夹并分割**
```bash
mkdir -p paper_name/

for i in {0..9}; do
  start=$((i*5 + 1))
  end=$(((i+1)*5))
  gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER \
     -dFirstPage=$start -dLastPage=$end \
     -sOutputFile="paper_name/paper_name_p$(printf '%03d' $start)-$(printf '%03d' $end).pdf" \
     paper_name.pdf 2>/dev/null
done
```

**步骤 4: 智能处理块**
- 一次使用 Read 工具读取一个块
- 从每个块中提取关键信息
- 逐步建立理解
- 不要试图在工作内存中保留所有块

**步骤 5: 选择性深度阅读**
- 扫描所有块后,识别最相关的部分
- 只详细读取这些部分以进行幻灯片开发
- 跳过附录、参考文献或不太相关的部分,除非需要

## 错误处理协议

**如果块处理失败:**
1. 注意有问题的块 (例如,"块 p021-025 失败")
2. 尝试分割成 1-2 页块
3. 如果仍然失败,跳过并记录间隙

**如果分割失败:**
1. 检查 Ghostscript 是否已安装: `gs --version`
2. 尝试替代方法: `pdftk paper.pdf burst output paper_%03d.pdf`
3. 如果一切都失败,要求用户手动上传特定页面范围

**如果内存/token 问题仍然存在:**
1. 每个 session 只处理 2-3 个块
2. 关注用户确定的最重要的特定部分

