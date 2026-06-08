# DINCAE — 卷积自编码器海洋数据插值

- **GitHub (Python, 已停维)**: https://github.com/gher-uliege/DINCAE
- **GitHub (Julia, 活跃)**: https://github.com/gher-uliege/DINCAE.jl
- **语言**: Julia (推荐) / Python (已停维，TensorFlow 1.x)
- **作者**: 列日大学 GHER 组
- **适用场景**: 卫星海洋观测（SST、叶绿素等）缺失数据重建

## 概述

DINCAE（Data-Interpolating Convolutional Auto-Encoder）使用卷积自编码器神经网络来重建海洋观测中的缺失数据。它不是传统 OI 方法，但代表了海洋数据插值的前沿方向，可作为你方法的对比基准。

## 方法原理

### 核心思想

DINCAE 将"有云遮挡的不完整卫星图像 → 完整图像"视为一个图像修复（inpainting）问题：

```
输入（含缺失值的 SST 场）→ [编码器 → 潜在表示 → 解码器] → 完整 SST 场
```

神经网络从大量训练样本中学习海洋 SST 的统计结构，自动发现比固定协方差模型更丰富的空间模式。

### 与传统 OI 的对比

| 特性 | OI/变分方法 | DINCAE |
|:---|:---|:---|
| 物理约束 | 显式（高斯协方差） | 隐式（从数据学习） |
| 计算速度（推理） | 慢（矩阵求逆） | 快（前向传播） |
| 训练时间 | 零 | 长（需 GPUs, 数小时到数天） |
| 不确定性估计 | 有 | 有（输出方差图） |
| 小尺度特征 | 受限于协方差假设 | 可学习复杂模式 |
| 泛化能力 | 好（物理约束） | 受训练集代表性限制 |

## 版本对比

### DINCAE 1.0 (Python)
- **引用**: Barth et al. (2020), GMD, doi:10.5194/gmd-13-1609-2020
- 单变量重建（SST-only）
- TensorFlow 1.15 → 已停维，不推荐新项目使用

### DINCAE 2.0 (Julia)
- **引用**: Barth et al. (2022), GMD, doi:10.5194/gmd-15-2183-2022
- **多变量联合重建**（如 SST + 高度计数据）
- GPU 加速（CUDA）
- 输出包含预期误差方差

## Julia 版快速开始

```julia
using DINCAE

# 加载含缺失值的 SST 数据（NetCDF）
sst_missing = ncread("sst_with_clouds.nc", "sst")

# 训练模型
model = DINCAE.train(sst_missing; epochs=1000)

# 重建完整 SST 场
sst_reconstructed = DINCAE.reconstruct(model, sst_missing)

# 获取重建不确定性
sst_error = DINCAE.error(model, sst_missing)
```

## 在你的场景中的应用

DINCAE 可作为"纯数据驱动"方案与传统 OI/EnOI 方案对比：

1. **训练数据**：使用 IAP 月平均数据作为训练集（完整全覆盖）
2. **输入**：在 IAP 数据中人工制造缺失（模拟 Argo 观测覆盖模式）
3. **目标**：学习缺失→完整的映射
4. **推理**：输入包含真实 Argo 观测和 IAP 背景场的数据，输出融合场

## 已知应用案例

- **Han et al. (2020)**: 南海叶绿素-a 填补
- **Ji et al. (2021)**: 台风海洋表面响应
- **Jung et al. (2022)**: 黑潮延伸体高分辨率日 SST 融合
- **Luo et al. (2022)**: 渤黄海叶绿素-a 重建
