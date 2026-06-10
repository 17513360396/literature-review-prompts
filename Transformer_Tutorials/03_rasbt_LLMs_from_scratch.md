# rasbt/LLMs-from-scratch —《Build a Large Language Model》配套教程

- **GitHub**: https://github.com/rasbt/LLMs-from-scratch
- **作者**: Sebastian Raschka (ML 研究员, Lightning AI)
- **语言**: Python (PyTorch) + Jupyter Notebook
- **配套书籍**: *Build a Large Language Model (From Scratch)*, Manning 2024
- **中文译版**: https://github.com/rickiepark/llm-from-scratch (韩语)
- **Stars**: 🔥 极高（2024年最热门的 LLM 教育仓库之一）

## 概述

这是 2024 年**最具影响力的 LLM 从零实现教程**。Raschka 的书和配套代码带领读者从空白的 Python 文件开始，一步一步构建一个类似 ChatGPT 的模型。与 Karpathy 的 nanoGPT 互补——Raschka 更偏向"系统性教学"，而 Karpathy 更偏向"极简代码"。

## 教程路线

```
Chapter 1: 理解大语言模型
  └── Transformer 架构概述、GPT 系列发展

Chapter 2: 文本数据处理
  ├── Tokenization (BPE)
  ├── 词嵌入 (Embedding)
  └── 数据加载器

Chapter 3: 注意力机制 🎯
  ├── 自注意力 (Self-Attention) 的原理和实现
  ├── 缩放点积注意力 (Scaled Dot-Product)
  ├── 因果注意力掩码 (Causal Mask)
  └── 多头注意力 (Multi-Head Attention)

Chapter 4: 从零实现 GPT 架构
  ├── Transformer Block (LayerNorm + Attention + FFN)
  ├── 位置嵌入 (Positional Embedding)
  ├── 完整的 GPT 模型组装
  └── 参数统计与初始化

Chapter 5: 预训练
  ├── 训练循环
  ├── 学习率调度 (Cosine Scheduler)
  ├── 梯度裁剪
  └── 在真实文本上训练

Chapter 6: 文本分类微调
  └── 情感分析等任务

Chapter 7: 指令微调 (Instruction Fine-tuning)
  ├── Alpaca 格式数据集
  ├── SFT 训练流程
  └── 使模型"遵循指令"

Chapter 8: 人类偏好对齐 (RLHF/DPO)
```

## 关键代码结构

```python
# 核心 Transformer Block (Chapter 4)
class TransformerBlock(nn.Module):
    def __init__(self, cfg):
        super().__init__()
        self.att = MultiHeadAttention(
            d_in=cfg["emb_dim"],
            d_out=cfg["emb_dim"],
            context_length=cfg["context_length"],
            num_heads=cfg["n_heads"],
            dropout=cfg["drop_rate"],
            qkv_bias=cfg["qkv_bias"]
        )
        self.ff = FeedForward(cfg)
        self.norm1 = LayerNorm(cfg["emb_dim"])
        self.norm2 = LayerNorm(cfg["emb_dim"])
        self.drop_shortcut = nn.Dropout(cfg["drop_rate"])

    def forward(self, x):
        # Pre-LayerNorm (更稳定的训练)
        shortcut = x
        x = self.norm1(x)
        x = self.att(x)
        x = self.drop_shortcut(x)
        x = x + shortcut  # 残差连接

        shortcut = x
        x = self.norm2(x)
        x = self.ff(x)
        x = self.drop_shortcut(x)
        x = x + shortcut
        return x
```

## 与其他教程的区别

| 特性 | rasbt/LLMs | nanoGPT | annotated-transformer |
|:---|:---|:---|:---|
| **教学方式** | 书 + 代码，系统性 | 视频 + 极简代码 | 代码注释 |
| **覆盖深度** | 从 tokenization 到 RLHF | 专注模型+训练 | 专注论文实现 |
| **配套素材** | Manning 出版级书籍 | YouTube 视频 | 博文 |
| **适合人群** | 想要完整体系的学习者 | 想要快速跑通的学习者 | 想要理解论文的学习者 |
| **后训练** | ✅ SFT + RLHF/DPO | ❌ | ❌ |

## 快速开始

```bash
git clone https://github.com/rasbt/LLMs-from-scratch.git
cd LLMs-from-scratch

# 按章节运行 Notebook
jupyter notebook ch03/ # 注意力机制
jupyter notebook ch04/ # GPT 架构
jupyter notebook ch05/ # 预训练
```

## 学习建议

1. **有书的话跟着书看**，没书的话 GitHub 上的 README 和 Jupyter Notebook 也很完整
2. **从 Chapter 3 开始**（注意力机制），Chapter 1-2 可以快速浏览
3. **重点关注 Chapter 4**——从零组装一个 GPT 模型
4. **对比 nanoGPT 的 model.py**——同一个模型的不同实现风格

## 中文资源

- 书籍中文版：图灵社区引进中
- 韩国语译本已出版（rickiepark/llm-from-scratch）
- Bilibili 上有大量国内创作者基于该书的讲解视频
