# ZC-BOM/optimal-interpolation — OI 最优插值算法 Python 实现

- **GitHub**: https://github.com/ZC-BOM/optimal-interpolation
- **语言**: Python
- **作者**: 澳大利亚气象局（Bureau of Meteorology, BOM）
- **适用场景**: 将稀疏站点观测数据格点化为规则网格

## 概述

这是学习 Optimal Interpolation 算法的**最佳入门仓库**。代码结构极简清晰，通过配置文件即可控制算法参数。虽然原始应用是降雨数据，但 OI 算法自身完全通用——更换背景场和观测数据即可适配海洋温盐融合。

## 核心原理

### 最优插值（OI）公式

分析场 `X_a` 由背景场 `X_b` 加上观测增量得到：

```
X_a = X_b + W × (Y - H × X_b)
```

其中：
- `W` = 权重矩阵（Kalman 增益）
- `Y` = 观测向量
- `H` = 观测算子（从格点到观测位置的插值）
- `Y - H×X_b` = 观测增量（观测值减背景值）

权重矩阵的计算：
```
W = B × H^T × (H × B × H^T + R)^(-1)
```

其中：
- `B` = 背景误差协方差矩阵
- `R` = 观测误差协方差矩阵

### 关键简化假设

在 ZC-BOM 实现中：
- 背景误差协方差 `B` 建模为 `σ²_b × exp(-d² / L²)`（高斯型距离衰减）
- 观测误差协方差 `R` 假设为对角矩阵（观测间不相关）
- 仅影响半径内的观测参与计算（局部化）

## 配置文件说明 (`si.conf`)

```ini
# 背景误差方差比（相对于观测误差）
background_error_variance_ratio = 0.5

# 去相关长度尺度（度或公里，取决于输入数据）
decorrelation_length_scale = 1.5

# 总误差方差（分析场误差估计的比例因子）
total_error_variance = 1.0
```

## 运行方式

### 方式一：Unix Shell 脚本
```bash
bash run_si.bash
```

### 方式二：Jupyter Notebook
```bash
jupyter notebook
# 打开 optimal_interpolation.ipynb
```

## 数据流

```
输入：
  ├── 背景场（格点数据）     → NetCDF 格式，规则经纬度网格
  ├── 站点观测数据            → 文本/CSV 格式，包含经度、纬度、观测值
  └── 配置文件 (si.conf)      → OI 参数

       ↓ 最优插值算法 ↓

输出：
  └── 分析场（格点数据）     → NetCDF 格式，同化后的规则网格
```

## 适配海洋温盐融合

若要用于"以 IAP 数据为背景场、Argo/CTD 为观测"的场景：

1. **背景场替换**：将背景场 NetCDF 替换为 IAP 月平均温盐格点数据
2. **观测数据替换**：将站点文本替换为 Argo/CTD/XBT 等实测剖面数据（注意需要先垂向插值到标准层）
3. **参数调整**：
   - `decorrelation_length_scale`：海洋中温盐的去相关尺度一般在 200-500 km，需根据研究区域调整
   - `background_error_variance_ratio`：需根据 IAP 产品的不确定性估计设定
4. **分深度层运行**：对每个标准深度层独立运行 OI

## 局限性

- 仅支持 2D 同化（需分深度层运行）
- 协方差模型为简单的高斯距离衰减，不能反映海洋动力学的各向异性
- 没有海岸线/地形约束（海洋中需防止信息跨陆地泄漏）

## 配套学习

建议配合 `Leo-Rain/Optimal_interpolation` 的 MATLAB 代码学习，后者包含更详细的数学推导。
