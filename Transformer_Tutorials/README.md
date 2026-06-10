# Transformer 教程合集 — GitHub 资源索引

> 整理时间：2026-06-09  
> 涵盖从零实现 Transformer、注意力机制、HuggingFace 生态、Vision Transformer、时序 Transformer 及 LLM 课程等全方位教程

## 核心公式

```
Attention(Q, K, V) = softmax(QKᵀ / √d_k) · V
```

---

## 文件索引

### 🔴 必看经典：从零实现 Transformer

| 文件 | 说明 | Stars |
|:---|:---|:---|
| [01_karpathy_nanoGPT_minGPT.md](01_karpathy_nanoGPT_minGPT.md) | Andrej Karpathy 的 GPT 教学实现（minGPT + nanoGPT） | 20k+/7k+ |
| [02_annotated_transformer.md](02_annotated_transformer.md) | Harvard NLP 逐行注释版 Transformer（"Attention Is All You Need"） | 经典 |
| [03_rasbt_LLMs_from_scratch.md](03_rasbt_LLMs_from_scratch.md) | Sebastian Raschka《Build a Large Language Model》配套代码 | 🔥 |
| [04_注意力机制专题教程.md](04_注意力机制专题教程.md) | Self-Attention、Multi-Head Attention、GQA/MQA/MLA 等变体集合 | — |

### 🟡 中文教程

| 文件 | 说明 |
|:---|:---|
| [05_Transformer中文教程合集.md](05_Transformer中文教程合集.md) | Datawhale、哈佛注释版中文翻译、从零手写教程 |

### 🟢 应用领域

| 文件 | 说明 |
|:---|:---|
| [06_Vision_Transformer教程.md](06_Vision_Transformer教程.md) | ViT 图像 Transformer 从零实现 + LANL 官方教程 |
| [07_Transformer时序预测.md](07_Transformer时序预测.md) | iTransformer、Informer、TFT 等时序 Transformer |
| [08_HuggingFace生态教程.md](08_HuggingFace生态教程.md) | HuggingFace 官方课程 + O'Reilly 书籍配套 Notebook |

### 🔵 LLM 课程体系

| 文件 | 说明 |
|:---|:---|
| [09_LLM课程与学习路线.md](09_LLM课程与学习路线.md) | mlabonne/llm-course、mikexcohen/LLM_course 等完整课程 |

### 📚 总结

| 文件 | 说明 |
|:---|:---|
| [10_方法对比与学习路径.md](10_方法对比与学习路径.md) | 所有资源的对比矩阵 + 5 条推荐学习路径 + 核心概念清单 |
