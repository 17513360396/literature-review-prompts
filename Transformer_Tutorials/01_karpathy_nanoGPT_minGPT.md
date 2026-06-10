# Andrej Karpathy 的 GPT 教学实现 — minGPT & nanoGPT

- **minGPT**: https://github.com/karpathy/minGPT
- **nanoGPT**: https://github.com/karpathy/nanoGPT
- **作者**: Andrej Karpathy (OpenAI 联合创始人, 前 Tesla AI 总监)
- **语言**: Python (PyTorch)
- **视频讲解**: "Let's build GPT: from scratch, in code, spelled out" (YouTube, 1h56min)

## 概述

minGPT 和 nanoGPT 是**学习 Transformer 不可绕过的经典**。Karpathy 的教学理念是"GPT 并没有那么复杂"——整个模型核心只需约 300 行代码。

```
minGPT (2020) → 教育优先，极简干净，约 300 行
nanoGPT (2023) → 实战优先，可复现 GPT-2 (124M)，约 600 行
nanoChat (2025) → 最新版，面向对话
```

## minGPT — 教育优先

### 核心文件结构
```
mingpt/
├── model.py    # GPT 模型定义（~300行）
├── bpe.py      # Byte-Pair Encoding 分词器
└── trainer.py  # 训练循环
```

### 关键代码片段（model.py 核心）

```python
class GPT(nn.Module):
    """GPT Language Model"""

    def __init__(self, config):
        super().__init__()
        # token embedding + position embedding
        self.tok_emb = nn.Embedding(config.vocab_size, config.n_embd)
        self.pos_emb = nn.Parameter(torch.zeros(1, config.block_size, config.n_embd))
        self.drop = nn.Dropout(config.embd_pdrop)
        # transformer blocks
        self.blocks = nn.Sequential(*[Block(config) for _ in range(config.n_layer)])
        # decoder head
        self.ln_f = nn.LayerNorm(config.n_embd)
        self.head = nn.Linear(config.n_embd, config.vocab_size, bias=False)

    def forward(self, idx, targets=None):
        b, t = idx.size()
        assert t <= self.block_size

        # token + position embeddings
        token_embeddings = self.tok_emb(idx)
        position_embeddings = self.pos_emb[:, :t, :]
        x = self.drop(token_embeddings + position_embeddings)

        # 通过所有 Transformer blocks
        x = self.blocks(x)

        # 输出层
        x = self.ln_f(x)
        logits = self.head(x)

        # 计算 loss
        loss = F.cross_entropy(logits.view(-1, logits.size(-1)),
                                targets.view(-1), ignore_index=-1)
        return logits, loss
```

### 内置 Demo
- **加法器**: 训练 GPT 做数字加法
- **字符级 LM**: 在莎士比亚文本上训练
- **排序**: 学习数列排序
- **GPT-2 微调**: 加载预训练权重生成文本

## nanoGPT — 实战优先

### 关键特性
| 特性 | 说明 |
|:---|:---|
| **可复现 GPT-2** | 在 OpenWebText 上训练，4天 8×A100 可复现 GPT-2 (124M) |
| **多 GPU** | DDP (Distributed Data Parallel) |
| **混合精度** | bfloat16 / float16 |
| **torch.compile** | PyTorch 2.0 编译加速 |
| **多平台** | GPU, CPU, Apple Silicon (MPS) |

### 训练一个莎士比亚 GPT（3分钟）
```bash
git clone https://github.com/karpathy/nanoGPT.git
cd nanoGPT
python data/shakespeare_char/prepare.py
python train.py config/train_shakespeare_char.py
# 3 分钟后，一个会写莎士比亚风格的 GPT 诞生
```

### 预训练模型复现
```bash
# 复现 GPT-2 (124M)
python train.py config/train_gpt2.py
```
| 模型 | 参数量 | Val Loss (复现) |
|:---|:---|:---|
| gpt2 | 124M | 3.12 |
| gpt2-medium | 350M | 2.84 |

## Karpathy 的核心理念

> "GPT is not a complicated model... All that's going on is that a sequence of indices feeds into a Transformer, and a probability distribution over the next index in the sequence comes out."

这句话是整个 Transformer 教学的精髓——Transformer 的本质就是：
```
输入 token 序列 → Transformer → 下一个 token 的概率分布
```

## 学习建议

1. **先看视频**（"Let's build GPT"），Karpathy 的讲解无出其右
2. **读 minGPT 的 model.py**，理解 GPT 的最简形态
3. **跑 nanoGPT 的 shakespeare_char 示例**，体验端到端训练
4. **对比 minGPT 和 nanoGPT**，理解"教学代码"和"实战代码"的差异
5. **尝试修改**：改层数、注意力头数、上下文长度，观察效果变化

## 配套分叉也值得看

- **paulxiong/minGPT-with-annotation**: 给 minGPT 加上详细注释
- **kreier/nano-gpt**: 跟随 Karpathy 视频的逐步实现
