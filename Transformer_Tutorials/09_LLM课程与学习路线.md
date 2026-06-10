# LLM 课程与学习路线

## 1. mlabonne/llm-course ⭐⭐⭐ 最全面的 LLM 学习路线

- **GitHub**: https://github.com/mlabonne/llm-course
- **格式**: Roadmap + Colab Notebooks
- **Stars**: 🔥🔥🔥 (顶级流量)

### 学习路线图

```
📚 LLM 基础
  ├── 1. Transformer 架构
  ├── 2. Tokenization (BPE, WordPiece, SentencePiece...)
  ├── 3. 注意力机制深入
  └── 4. 文本生成解码策略 (greedy, beam, top-k, top-p...)

🔬 LLM 科学
  ├── 1. 预训练数据
  ├── 2. 扩展定律 (Scaling Laws)
  ├── 3. 混合精度训练
  └── 4. 分布式训练 (FSDP, TP, PP)

🧪 LLM 训练
  ├── 1. 从零预训练
  ├── 2. SFT (Supervised Fine-Tuning)
  ├── 3. RLHF (Reinforcement Learning from Human Feedback)
  └── 4. DPO (Direct Preference Optimization)
```

---

## 2. mikexcohen/LLM_course ⭐ 深入底层机制

- **GitHub**: https://github.com/mikexcohen/LLM_course
- **Stars**: ~263
- **配套**: Udemy 课程（90+ 小时视频）

### 特点
- 从零构建 Transformer Block
- 深入 Self-Attention 的数学机制
- 不使用黑盒 API，全手写
- 包含完整训练 pipeline

---

## 3. fangpin/llm-from-scratch ⭐ 完全解构式实现

- **GitHub**: https://github.com/fangpin/llm-from-scratch
- **特点**: 甚至手写了 `Linear`、`Softmax`、`RMSNorm`、`SwiGLU`、`RoPE`

### 这是什么级别的"从零"
```python
# 别人说的 "从零"：用 nn.Linear
self.q_proj = nn.Linear(dim, dim)

# fangpin 的 "从零"：自己实现 Linear
class Linear:
    def __init__(self, in_features, out_features):
        self.weight = torch.randn(...)
        self.bias = torch.zeros(...)
    def forward(self, x):
        return x @ self.weight.T + self.bias  # 手动矩阵乘法
```

---

## 4. 其他值得关注的 LLM 课程

| 仓库 | 特点 |
|:---|:---|
| **Anello92/LLM-From-Scratch** | Jupyter Notebook 逐步讲解，最适零基础 |
| **malibayram/llm-from-scratch** | 8 模块完整课程（tokenization → SFT） |
| **uan-vanessa-liu/Transformer-from-scratch** | ~240 行代码训练一个微型 LLM |
| **Lo3okSky/LLM_course** | 12 模块从基础到部署的系统课程 |
| **JoeCodeswell/LLM-From-Scratch-jcw** | Anello92 的分叉，加入新的解释 |

---

## 5. 斯坦福 CS324 — 大语言模型

- **在线**: https://stanford-cs324.github.io/winter2022/
- **特点**: 全球最知名的 LLM 课程之一
- **包含**: 扩展定律、涌现能力、对齐、安全性等

## 6. 李宏毅 LLM 课程

- **Bilibili**: 搜索"李宏毅 大语言模型"
- **特点**: 中文讲解，通俗易懂
- **内容**: 从 Transformer 到 ChatGPT 的完整技术栈

---

## 推荐 LLM 学习路径

```
Phase 1: 理解 Transformer（1-2 周）
  ├── [文件 01] minGPT 代码精读
  ├── [文件 02] Annotated Transformer
  └── [文件 04] 注意力机制专题

Phase 2: 从零训练一个小语言模型（1 周）
  ├── [文件 03] rasbt/LLMs-from-scratch Ch3-5
  └── 或 nanoGPT 的 Shakespeare 示例

Phase 3: 理解完整 LLM 技术栈（2-4 周）
  ├── mlabonne/llm-course
  ├── HuggingFace 课程 (Ch5-7)
  └── SFT + DPO 实战

Phase 4: 紧跟前沿（持续）
  └── 论文 + GitHub trending + 中文社区
```

## 如果不确定从哪里开始

1. **纯新手** → Anello92/LLM-From-Scratch 的 Jupyter Notebook
2. **有 ML 基础但不熟 NLP** → rasbt/LLMs-from-scratch
3. **想快速上手训练** → nanoGPT 的 shakespeare_char
4. **想理解完整技术栈** → mlabonne/llm-course
