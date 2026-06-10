# HuggingFace 生态教程合集

## 1. HuggingFace 官方课程

- **GitHub**: https://github.com/huggingface/course
- **在线版**: https://huggingface.co/learn/nlp-course
- **Notebook**: https://github.com/huggingface/notebooks
- **翻译**: 20+ 种语言（含中文）

### 课程章节

```
Part 1: 🤗 Transformers 基础
  ├── Chapter 1: Transformer 模型概述
  ├── Chapter 2: 使用 🤗 Transformers (pipeline)
  ├── Chapter 3: 微调预训练模型
  └── Chapter 4: 分享模型和 tokenizer

Part 2: 🤗 Datasets & Tokenizers
  ├── Chapter 5: 🤗 Datasets 库
  ├── Chapter 6: 🤗 Tokenizers 库
  └── Chapter 7: 主流的 NLP 任务

Part 3: 进阶主题
  ├── Chapter 8: 如何寻求帮助
  ├── Chapter 9: 实用技巧
  └── Chapter 10-12: 各种应用
```

---

## 2. NLP with Transformers (O'Reilly) — 配套 Notebooks

- **GitHub**: https://github.com/nlp-with-transformers/notebooks
- **书籍**: *Natural Language Processing with Transformers* (Tunstall, von Werra, Wolf)

### 章节内容

| 章节 | 主题 |
|:---|:---|
| 1 | Hello Transformers |
| 2 | 文本分类 |
| 3 | Transformer 解剖 |
| 4 | 多语言命名实体识别 |
| 5 | 文本生成 |
| 6 | 摘要 |
| 7 | 问答 |
| 8 | 高效推理 |
| 9 | 少量/零样本学习 |
| 10 | 训练 Transformer 从零开始 |
| 11 | 未来方向 |

---

## 3. HuggingFace 初学者指南 (nursenakok)

- **地址**: https://github.com/nursenakok/hugging-face-beginner-guide

三个渐进模块：
1. **Pipelines**: 一行代码搞定情感分析、NER、QA、摘要、文本生成
2. **Tokenizers**: 深入理解分词原理
3. **Attention**: 用 heatmap 可视化注意力矩阵

---

## 4. HuggingFace 核心库速览

### 🤗 Transformers
```python
from transformers import pipeline

# 一行代码搞定几乎所有 NLP 任务
classifier = pipeline("sentiment-analysis")
classifier("I love this tutorial!")
# → [{'label': 'POSITIVE', 'score': 0.999}]

generator = pipeline("text-generation", model="gpt2")
generator("The future of AI is", max_length=50)

summarizer = pipeline("summarization")
summarizer(long_text)
```

### 🤗 Datasets
```python
from datasets import load_dataset

# 一行代码加载任意公开数据集
dataset = load_dataset("imdb")
dataset = load_dataset("squad")
dataset = load_dataset("glue", "mrpc")
# 支持 10000+ 数据集
```

### 🤗 Tokenizers
```python
from tokenizers import Tokenizer
from tokenizers.models import BPE

# Rust 实现，极速分词
tokenizer = Tokenizer(BPE())
# 训练你自己的 tokenizer
```

### 🤗 PEFT (LoRA/QLoRA)
```python
from peft import LoraConfig, get_peft_model

# 只微调 0.1% 的参数，效果接近全参数微调
lora_config = LoraConfig(r=8, lora_alpha=32, target_modules=["q_proj", "v_proj"])
model = get_peft_model(base_model, lora_config)
```

---

## 5. HuggingFace 学习路径

```
第1步: huggingface/course 的 Chapter 2
        → 学会用 pipeline() 快速体验

第2步: huggingface/course 的 Chapter 3
        → 学会微调预训练模型

第3步: nlp-with-transformers/notebooks 的 Chapter 3
        → 解剖 Transformer 内部结构

第4步: nlp-with-transformers/notebooks 的 Chapter 10
        → 从零训练一个 Transformer

第5步: 回到前面 01-04 文件
        → 用从零手写代码彻底理解底层
```

## HF 官方的其他学习资源

- **HuggingFace NLP Course**: https://huggingface.co/learn/nlp-course
- **HuggingFace Audio Course**: https://huggingface.co/learn/audio-course
- **HuggingFace Diffusion Course**: https://huggingface.co/learn/diffusion-course
- **HuggingFace Deep RL Course**: https://huggingface.co/learn/deep-rl-course
- **HuggingFace Agents Course**: https://huggingface.co/learn/agents-course
