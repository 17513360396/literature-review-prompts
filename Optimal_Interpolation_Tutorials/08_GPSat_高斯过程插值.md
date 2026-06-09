# GPSat — 高斯过程卫星数据插值

- **GitHub**: https://github.com/CPOMUCL/GPSat
- **语言**: Python (TensorFlow/GPflow) | **论文**: *Nature Communications* (2024)
- **作者**: UCL 极地观测与建模中心（CPOM）
- **适用场景**: 卫星高度计数据的可扩展高斯过程插值

## 概述

GPSat 使用**局部高斯过程专家模型**来实现卫星数据的可扩展最优插值。虽然名为"高斯过程"而非"最优插值"，但高斯过程回归在数学上与最优插值是等价的——两者都是最小方差线性无偏估计（BLUE）。GPSat 的关键创新在于**可扩展性**（通过局部专家 + GPU 加速实现 504× 加速）和**非平稳性处理**。

## 高斯过程 ≈ 最优插值

### 数学等价性

| 最优插值 | 高斯过程回归 |
|:---|:---|
| `x_a = x_b + K (y - H x_b)` | `μ_post = μ_prior + K_* (y - μ_obs_points)` |
| `K = B H^T (H B H^T + R)^{-1}` | `K_* = K(x*, X) (K(X, X) + σ²_n I)^{-1}` |
| B = 背景误差协方差 | K(X, X) = 核函数（协方差函数） |
| R = 观测误差协方差 | σ²_n I = 观测噪声方差 |

两者的数学形式完全相同。差异在于**协方差模型**：
- OI：协方差通常建模为**距离的函数**（如高斯指数衰减）
- GP：协方差通过**核函数**灵活指定，可学习非平稳结构

### 局部专家策略

```
全局 OI 的问题：
  100万 × 100万 的协方差矩阵无法求逆

GPSat 的解决方案（局部专家）：
  1. 将空间划分为 N 个重叠子区域
  2. 每个子区域独立拟合一个 GP（小矩阵可求逆）
  3. 在重叠区域对预测值进行加权平均

结果：
  O(N × m³) vs 原始的 O(M³)，其中 m << M
```

## 卫星高度计应用

### 数据流
```
原始卫星数据
├── CryoSat-2 (雷达高度计)
├── Sentinel-3A (雷达高度计)
└── Sentinel-3B (雷达高度计)
        ↓
    沿轨数据处理
        ↓
    局部专家 GP 插值
        ↓
    格点化海面高度异常 (SLA) 或冰雷达干舷
```

### 实现特点

```python
# 概念示意
from gpsat import LocalExpertGP

# 1. 数据分箱（binning）
binned_data = bin_along_track_data(raw_altimetry)

# 2. 创建局部专家模型
model = LocalExpertGP(
    bin_data=binned_data,
    n_experts=50,           # 局部专家数量
    overlap_ratio=0.5,      # 重叠比例
    kernel='matern52',      # 核函数选择
    length_scale=150e3      # 特征长度尺度（米）
)

# 3. 训练 + 预测
model.fit()
predictions, uncertainties = model.predict(grid_points)
```

## 教学资源

CPOMUCL 提供了配套的教学 Notebook（GEOL0069-AI4EO 课程）：

- [GPSat 入门与 GP 基础](https://cpomucl.github.io/GEOL0069-AI4EO/Chapter_2_Intro_to_GPSat.html)
- [沿轨插值实战](https://cpomucl.github.io/GEOL0069-AI4EO/Chapter_2_GPSat_along_track-2.html)
- [从高度计数据探测海洋中尺度涡](https://cpomucl.github.io/GEOL0069-AI4EO/Eddies_from_altimetry_part2.html)

## 与你的学习需求的关联

GPSat 与传统 OI 对比学习的价值：

| 特性 | 传统 OI (ZC-BOM) | GPSat |
|:---|:---|:---|
| 协方差来源 | 预设（高斯函数） | 数据学习（GP 核参数拟合） |
| 可扩展性 | 小数据集 | 大数据集（局部专家） |
| 非平稳性处理 | 无 | 天然支持（不同区域不同核参数） |
| GPU 加速 | 无 | 有（TensorFlow） |
| 不确定性估计 | 有（但可能不准确） | 有（自然输出后验方差） |
| 教学友好度 | 极高（代码简单） | 中等（需要 GP 背景） |

## 建议

如果你已经理解了传统 OI（ZC-BOM/DA-tutorials），GPSat 是理解"如何将 OI 现代化、可扩展化"的绝佳资源。它的局部专家 + GPU 策略代表了地球科学数据插值的前沿方向。
