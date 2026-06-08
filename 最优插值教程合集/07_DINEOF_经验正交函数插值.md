# DINEOF-Python — 经验正交函数缺失数据插补

- **GitHub**: https://github.com/zhouweichen1992110/DINEOF-Python
- **语言**: Python
- **作者**: zhouweichen1992110
- **适用场景**: 基于经验正交函数（EOF）的时空缺失数据重建

## 概述

DINEOF（Data Interpolating Empirical Orthogonal Functions）是一种基于 EOF 分解的缺失数据插补方法。它通过迭代方式，利用数据自身的时空协方差结构来填充缺失值。

## 方法原理

### DINEOF 算法流程

```
1. 初始化：用空间均值填充所有缺失值
2. LOOP：
   a. 对当前完整数据做 EOF 分解
   b. 保留前 k 个 EOF 模态重建数据
   c. 用重建值更新缺失位置
   d. 检查收敛
3. 输出：插补后的完整数据场
```

### 核心数学

给定含缺失的数据矩阵 `X`（行=空间点, 列=时间）：
```
X ≈ U_k × Σ_k × V_k^T
```
其中保留了 k 个主导 EOF 模态。缺失位置的插补值为：
```
X_missing = (U_k × Σ_k × V_k^T)_missing
```

### 最优 EOF 模态数的确定

通过交叉验证确定：
1. 额外留出一小部分已知数据作为验证集
2. 对不同的 k 值重建
3. 选择验证集 RMSE 最小的 k

## 代码使用

```bash
git clone https://github.com/zhouweichen1992110/DINEOF-Python.git
cd DINEOF-Python
```

具体使用方法见仓库 README。

## 与 OI 的对比

| 特性 | OI/EnOI | DINEOF |
|:---|:---|:---|
| 时空维度 | 纯空间（需逐时处理） | 时空联合 |
| 协方差来源 | 预设模型或模式集合 | 数据自身 EOF |
| 背景场 | 必需（如 IAP 气候态） | 不需要 |
| 计算复杂度 | O(n³)（观测数） | O(min(n_space, n_time)³) |
| 季节性处理 | 隐式（背景场可能含气候态） | 通过 EOF 模态自然捕获 |

## 在你的场景中的应用

DINEOF 可作为无模型（model-free）方案：
1. 将研究区域内所有格点的所有时间步组织为时空矩阵
2. 缺失值 = 无观测覆盖的格点
3. 已知值 = 有观测的格点
4. 运行 DINEOF 即完成插值

这种方法不依赖 IAP 背景场（与 IAP 方法不同），但可作为对比实验。

## 引用

- **Alvera-Azcárate, A., Barth, A., Rixen, M., & Beckers, J. M. (2005)**: Reconstruction of incomplete oceanographic data sets based on EOF decomposition. *Journal of Geophysical Research*, 110, C12011.
- **Beckers, J. M., & Rixen, M. (2003)**: EOF calculations and data filling from incomplete oceanographic datasets. *Journal of Atmospheric and Oceanic Technology*, 20(12), 1839-1856.
