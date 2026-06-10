# The Annotated Transformer — Harvard NLP 逐行注释版

- **GitHub**: https://github.com/harvardnlp/annotated-transformer
- **中文版**: https://github.com/mcxiaoxiao/annotated-transformer-Chinese
- **作者**: Harvard NLP / Sasha Rush
- **语言**: Python (PyTorch) + Jupyter Notebook
- **对应论文**: "Attention Is All You Need" (Vaswani et al., 2017)

## 概述

这是**学习原始 Transformer 论文最经典的教程**，被誉为 Transformer 学习的"圣经"。它不是简单的代码——而是将论文中的每一个公式、每一段描述都**逐行映射到可运行的 PyTorch 代码**。

## 覆盖内容

```
Annotated Transformer 教程结构：

Part 1: 背景
  └── 模型架构总览

Part 2: 模型训练
  ├── 批次与掩码 (Batches and Masking)
  ├── 训练循环 (Training Loop)
  ├── 训练数据与批处理
  ├── 优化器 (Adam + Warmup)
  └── 正则化 (Label Smoothing)

Part 3: 模型实现 🎯 核心
  ├── 缩放点积注意力 (Scaled Dot-Product Attention)
  ├── 多头注意力 (Multi-Head Attention)
  ├── 位置前馈网络 (Position-wise FFN)
  ├── 词嵌入与 Softmax
  ├── 位置编码 (Positional Encoding)
  ├── 完整模型组装
  └── 推理 (Greedy Decoding)

Part 4: 第一个示例
  └── 复制任务 (Copy Task)

Part 5: 真实训练
  └── Multi30k 德语→英语翻译
```

## 核心代码示例

### 缩放点积注意力
```python
def attention(query, key, value, mask=None, dropout=None):
    """Compute Scaled Dot Product Attention"""
    d_k = query.size(-1)
    scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, -1e9)
    p_attn = scores.softmax(dim=-1)
    if dropout is not None:
        p_attn = dropout(p_attn)
    return torch.matmul(p_attn, value), p_attn
```

### 多头注意力
```python
class MultiHeadedAttention(nn.Module):
    def __init__(self, h, d_model, dropout=0.1):
        super().__init__()
        self.d_k = d_model // h
        self.h = h
        self.linears = clones(nn.Linear(d_model, d_model), 4)
        self.attn = None
        self.dropout = nn.Dropout(p=dropout)

    def forward(self, query, key, value, mask=None):
        # 1) 线性投影 + 分头
        query, key, value = [
            lin(x).view(nbatches, -1, self.h, self.d_k).transpose(1, 2)
            for lin, x in zip(self.linears, (query, key, value))
        ]
        # 2) 所有头上的并行注意力
        x, self.attn = attention(query, key, value, mask=mask,
                                  dropout=self.dropout)
        # 3) 拼接 + 最终投影
        x = x.transpose(1, 2).contiguous().view(nbatches, -1, self.h * self.d_k)
        return self.linears[-1](x)
```

## 论文公式 → 代码映射

| 论文公式 | 代码位置 |
|:---|:---|
| `Attention(Q,K,V) = softmax(QKᵀ/√d_k)V` | `attention()` 函数 |
| `MultiHead(Q,K,V) = Concat(head₁,...,head_h)Wᴼ` | `MultiHeadedAttention.forward()` |
| `FFN(x) = max(0, xW₁+b₁)W₂+b₂` | `PositionwiseFeedForward` |
| `PE(pos,2i) = sin(pos/10000^(2i/d))` | `PositionalEncoding` |
| `LayerNorm(x + Sublayer(x))` | `SublayerConnection` |

## 特点

1. **公式到代码的完美映射**：每个数学公式旁边就是对应的 PyTorch 实现
2. **可运行的 Notebook**：不是纯讲解，所有代码都可以在 Colab 上运行
3. **工业级代码质量**：由 Harvard NLP 团队维护，代码规范专业
4. **包含训练全流程**：从数据加载到 BLEU 评估的完整 pipeline

## 中文版

中文版由 mcxiaoxiao 翻译，对英文公式和术语做了大量注释：
```
https://github.com/mcxiaoxiao/annotated-transformer-Chinese
```

## 学习建议

1. **配合论文阅读**：打开原始论文 "Attention Is All You Need"，边看论文边看此代码
2. **配合可视化**：同时打开 Jay Alammar 的 [The Illustrated Transformer](http://jalammar.github.io/illustrated-transformer/)，三管齐下
3. **从上往下读**：先看 Part 1-2（概览+训练），再深究 Part 3（实现细节）
4. **手写一遍**：不要只看，跟着代码用 Colab 逐步运行、修改参数

## 与 minGPT/nanoGPT 的互补

| Annotated Transformer | minGPT/nanoGPT |
|:---|:---|
| Encoder-Decoder 架构 | Decoder-only 架构 |
| 论文原版实现 | GPT 系列实现 |
| 用于翻译任务 | 用于语言生成 |
| 教育+理论 | 教育+实战 |
| 两者结合学 = 全面理解 Transformer | ← 强烈推荐 |
