# Transformer 中文教程合集

## 1. Datawhale/fun-transformer ⭐ NumPy 手写实现

- **地址**: https://github.com/datawhalechina/fun-transformer
- **语言**: Python (NumPy + SciPy)
- **作者**: Datawhale 开源组织
- **特点**: **不用 PyTorch，完全用 NumPy 手写注意力机制**，深入底层原理

### 课程章节
```
第1章: Attention 机制详解
   ├── Seq2Seq + Attention
   ├── Self-Attention 原理
   └── Multi-Head Attention

第2章: Encoder 结构
   ├── 位置编码
   ├── 前馈网络
   ├── Layer Normalization
   └── 残差连接

第3章: Decoder 结构
   ├── Masked Self-Attention
   ├── Cross-Attention
   └── 完整 Decoder 组装

第4章: 完整 Transformer
   ├── Encoder-Decoder 拼接
   └── 模型训练

第5章: 机器翻译实战
   └── 端到端翻译项目
```

### 最大亮点
用 NumPy 手写，你会被迫理解**每个矩阵运算的维度变化**——这比用 `nn.MultiheadAttention` 学到的多 10 倍。

---

## 2. mcxiaoxiao/annotated-transformer-Chinese ⭐ 哈佛教程中文版

- **地址**: https://github.com/mcxiaoxiao/annotated-transformer-Chinese
- **语言**: Python (PyTorch)
- **来源**: 翻译自 harvardnlp/annotated-transformer
- **特点**: 原汁原味的经典教程，逐行中文注释

这是学习原版 Transformer 论文的最佳中文资源。每行代码都有中文注释，解释了"为什么要这样做"。

---

## 3. haukzero/Transformer-Demo ⭐ 积木式搭建

- **地址**: https://github.com/haukzero/Transformer-Demo
- **语言**: Python (PyTorch)
- **特点**: 像搭积木一样一步一步构建 Transformer

### 项目结构
```
Transformer-Demo/
├── pe.py       # 位置编码 (Positional Encoding)
├── mha.py      # 多头注意力 (Multi-Head Attention)
├── ffn.py      # 前馈网络 (Feed-Forward Network)
├── enc.py      # 编码器 (Encoder)
├── dec.py      # 解码器 (Decoder)
├── trm.py      # 完整模型组装
├── kv_cache.py # KV-Cache 加速解码
└── moe.py      # MoE 模块（进阶）
```

每个文件独立，可以单独阅读和测试。适合"按需学习"——比如只想知道位置编码怎么实现的，直接打开 `pe.py` 即可。

---

## 4. art1524/learn-nlp-with-transformers ⭐ 中文 NLP 入门

- **地址**: https://github.com/art1524/learn-nlp-with-transformers
- **特点**: 结合生动的原理讲解 + 动手实践
- **内容**: 图解 Attention、图解 Transformer、图解 BERT、图解 GPT

为初学者量身打造，大量可视化配图。

---

## 5. yj-liuzepeng/transfomer ⭐ 零基础入门

- **地址**: https://github.com/yj-liuzepeng/transfomer
- **语言**: Python (PyTorch)
- **文档**: `transformer_theory.md` + `transformer.py`

从零开始手写，专门为零基础学习者设计。附带参数说明表和 FAQ。

---

## 6. infrasys-ai 从零实现 Transformer 训练

- **地址**: https://infrasys-ai.github.io/aiinfra-docs/06AlgoData01Basic/CODE02TransformerTrain.html
- **特点**: 完全使用 PyTorch 张量操作，不依赖 `nn.Transformer`

用手撕的方式写 Transformer，每个组件都附带原理公式。

---

## 推荐学习路径（中文学习者）

```
第1步: art1524/learn-nlp-with-transformers
        → 图解理解 Transformer 是什么（30分钟）

第2步: haukzero/Transformer-Demo
        → 逐个文件看代码实现（2小时）

第3步: mcxiaoxiao/annotated-transformer-Chinese
        → 跟着哈佛教程逐行理解（半天）

第4步: datawhalechina/fun-transformer
        → 用 NumPy 手写一遍，彻底吃透（1-2天）
```

## 其他优质中文资源

- **Bilibili**: 李沐《动手学深度学习》Transformer 章节
- **知乎**: 大量 Transformer 图解文章（搜索"图解 Transformer"）
- **微信公众号**: Datawhale、机器之心等的系列教程
