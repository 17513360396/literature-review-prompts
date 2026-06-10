# Vision Transformer (ViT) 教程合集

## 1. LANL — Vision Transformers Explained ⭐⭐⭐ 最完整

- **GitHub**: https://github.com/lanl/vision_transformers_explained
- **作者**: Los Alamos National Laboratory
- **格式**: Jupyter Notebook (4 篇系列文章)
- **语言**: Python (PyTorch)

### 教程系列（全部含可运行代码 + 可视化）

| 文章 | 内容 |
|:---|:---|
| **Vision Transformers Explained** | 完整 ViT 架构 walkthrough，用像素画示例演示 patch 分词 |
| **Attention for Vision Transformers** | 单头/多头注意力的深度解析，每步跟踪张量维度 |
| **Position Embeddings** | 位置嵌入的深入分析（可学习的 vs 正弦的） |
| **Tokens-to-Token ViT** | T2T-ViT：从头在 ImageNet 上训练 ViT |

### 最大亮点

用**像素画"山间黄昏"**来演示 patch 划分过程：

```
原始图像 (224×224×3)
    ↓ 分成 16×16 的 patches
196 个 patch，每个 16×16×3 = 768 维
    ↓ 线性投影
196 个 token，每个 768 维 → 送入 Transformer Encoder
    ↓
分类输出
```

每个步骤都有代码 + 可视化输出 + 张量维度标注。

---

## 2. lucidrains/vit-pytorch ⭐ 工业级实现

- **GitHub**: https://github.com/lucidrains/vit-pytorch
- **安装**: `pip install vit-pytorch`
- **作者**: Phil Wang (lucidrains)

### 支持的超全 ViT 变体

| 模型 | 论文 |
|:---|:---|
| **ViT** | Dosovitskiy et al. (2021) |
| **DeepViT** | Zhou et al. (2021) |
| **CaiT** | Touvron et al. (2021) |
| **Token-to-Token ViT** | Yuan et al. (2021) |
| **Cross ViT** | Chen et al. (2021) |
| **PiT** | Heo et al. (2021) |
| **LeViT** | Graham et al. (2021) |
| **CvT** | Wu et al. (2021) |
| **Distillable ViT** | Touvron et al. (2021) |
| **Masked Patch Prediction** | Atito et al. (2022) |

### 使用示例
```python
import torch
from vit_pytorch import ViT

v = ViT(
    image_size=256,
    patch_size=32,
    num_classes=1000,
    dim=1024,
    depth=6,
    heads=16,
    mlp_dim=2048,
    dropout=0.1,
    emb_dropout=0.1
)

img = torch.randn(1, 3, 256, 256)
preds = v(img)  # (1, 1000)
```

---

## 3. Kasyfil97/vision-transformer-from-scratch — 从零实现

- **地址**: https://github.com/Kasyfil97/vision-transformer-from-scratch
- **特点**: 不用高层库，一步一步手写 ViT 每个组件

---

## 4. JavierUrenaPhDProjects/ViT_Implementation_tutorial — 含训练

- **地址**: https://github.com/JavierUrenaPhDProjects/ViT_Implementation_tutorial
- **特点**: Jupyter Notebook + 完整训练脚本 + 在 CIFAR-10 上训练
- **含**: 预训练 checkpoint、注意力图可视化

---

## ViT vs CNN 的核心差异

| CNN | ViT |
|:---|:---|
| 局部感受野（卷机核 3×3, 5×5） | 全局感受野（每个 token 关注全部 token） |
| 平移等变性（translation equivariance） | 需要位置嵌入补偿 |
| 层次化特征（low→mid→high level） | 全局同质处理 |
| 归纳偏置强（适合小数据） | 归纳偏置弱（需要大数据/强正则） |
| 参数效率高 | 需要更多数据 |

## 学习建议

1. **先理解 NLP Transformer**（Self-Attention 原理），再学 ViT
2. **看 LANL 教程的第 1 篇** — 用像素画理解 patch 划分是最直观的
3. **用 lucidrains/vit-pytorch** 在你的数据上快速跑通
4. **如果想深入**，看 Kasyfil97 从零实现，每一步都自己写

## ViT 核心代码

```python
class ViT(nn.Module):
    def __init__(self, image_size, patch_size, num_classes, dim, depth, heads, mlp_dim):
        super().__init__()
        num_patches = (image_size // patch_size) ** 2
        patch_dim = 3 * patch_size ** 2

        self.patch_embed = nn.Linear(patch_dim, dim)     # patch → token
        self.pos_embed = nn.Parameter(torch.randn(1, num_patches + 1, dim))
        self.cls_token = nn.Parameter(torch.randn(1, 1, dim))

        self.transformer = nn.TransformerEncoder(
            nn.TransformerEncoderLayer(d_model=dim, nhead=heads, dim_feedforward=mlp_dim),
            num_layers=depth
        )
        self.mlp_head = nn.Sequential(nn.LayerNorm(dim), nn.Linear(dim, num_classes))

    def forward(self, img):
        # img: (B, 3, H, W) → patches → tokens
        # ... (patch 划分 + 嵌入 + 位置编码)
        x = self.transformer(x)        # Transformer Encoder
        return self.mlp_head(x[:, 0])  # 取 CLS token 输出分类
```
