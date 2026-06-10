# Transformer 时序预测教程合集

## 1. iTransformer ⭐ SOTA 倒置注意力

- **GitHub**: https://github.com/thuml/iTransformer
- **论文**: ICLR 2024, 清华/Ant Group
- **核心创新**: 把注意力**从时间维倒置到变量维**

### 倒置注意力的思想

```
传统 Transformer:
  对每个变量，让时间步之间互相关注
  问题：同一时刻的不同变量（如温度、湿度、风速）之间的关联被忽略

iTransformer:
  对每个时间步，让变量之间互相关注
  优势：天然捕获多变量之间的协变关系
```

### 使用
```bash
pip install iTransformer
```

---

## 2. pytorch-forecasting — 含 TFT 的高层库

- **GitHub**: https://github.com/jdb78/pytorch-forecasting
- **核心模型**: Temporal Fusion Transformer (TFT)
- **特点**: 生产级时序预测库，含完整的数据处理、训练、评估流程

### TFT 架构亮点
- **Variable Selection Networks**: 自动选择哪些变量有用
- **Interpretable Multi-Head Attention**: 可解释的注意力——能看懂模型在关注什么
- **分位数输出**: 不只点预测，还有不确定性区间

---

## 3. Time-Series-Transformer (Ruhul-Quddus-Tamim)

- **地址**: https://github.com/Ruhul-Quddus-Tamim/Time-Series-Transformer
- **特点**: Encoder-Decoder 完整 Transformer，结合 CNN + Attention
- **应用领域**: 能源消费、IoT 传感器、金融市场

---

## 4. rezaAdinepour/Time-Series-Forecasting

- **地址**: https://github.com/rezaAdinepour/Time-Series-Forecasting
- **特点**: 干净的教学式实现，含完整训练 pipeline 和评估 (MAE, MSE, RMSE)
- **数据**: 股票数据

---

## 5. Gateformer (NYU)

- **地址**: https://github.com/nyuolab/Gateformer
- **核心创新**: 同时使用时间维度和变量维度的注意力，用门控机制融合
- **效果**: 比基线提升最多 20.7%

---

## 6. timesfm_torch — Google TimesFM 的 PyTorch 复现

- **地址**: https://github.com/ziraax/timesfm_torch
- **论文**: "A Decoder-Only Foundation Model for Time-Series Forecasting" (Google Research)
- **特点**: Decoder-only 的时序基础模型，无需重新训练即可零样本预测

---

## 时序 Transformer 技术要点

### 位置编码的特殊性

时序任务中位置编码比 NLP 更关键：

```python
# 标准正弦位置编码 — 对时序也有效
# 但有时需要时间特定的编码

class TimeFeatureEncoding(nn.Module):
    """将时间戳编码为特征向量"""
    def forward(self, timestamps):
        # 提取：小时、星期几、月份、是否为节假日等
        hour = timestamps.hour / 23.0
        day_of_week = timestamps.dayofweek / 6.0
        month = timestamps.month / 12.0
        # ... 更多时间特征
        return torch.stack([hour, day_of_week, month, ...], dim=-1)
```

### 因果掩码 vs 双向注意力

| 任务 | 掩码方式 |
|:---|:---|
| 实时预测（streaming） | 因果掩码（不能看未来） |
| 离线插补（填补缺失值） | 双向注意力（可以看前后） |
| 多步预测 | 因果掩码（防止信息泄漏） |

### 长时间序列的挑战

```
O(n²) 的注意力复杂度对长序列是致命问题。

解决方案：
  ├── Informer: ProbSparse Attention, O(L log L)
  ├── Autoformer: 自相关机制替代注意力
  ├── FEDformer: 频域增强
  ├── PatchTST: 将序列分成 patch（类似 ViT 的思路）
  │   └── 把 seq_len=512 分成 42 个 patch，复杂度骤降
  └── iTransformer: 倒置——在变量维做注意力
```

---

## 时序 Transformer 选择指南

| 场景 | 推荐 |
|:---|:---|
| 快速上手、生产环境 | `pytorch-forecasting` (TFT) |
| 多变量、多维时间序列 | `iTransformer` |
| 极长序列（>1000 步） | `PatchTST` 或 `Informer` |
| 零样本/基础模型 | `TimesFM` |
| 需要可解释性 | `TFT`（含 Interpretable Attention） |
