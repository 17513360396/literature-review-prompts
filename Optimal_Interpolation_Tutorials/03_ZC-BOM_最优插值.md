# ZC-BOM/optimal-interpolation — 最优插值 Python 实现

- **GitHub**: https://github.com/ZC-BOM/optimal-interpolation
- **语言**: Python
- **作者**: 澳大利亚气象局（Bureau of Meteorology）
- **适用场景**: 将稀疏站点观测格点化（降雨 → 格点场）

## 概述

这是 GitHub 上**最简洁清晰的 OI 纯 Python 教学实现**。代码量小但结构完整，通过配置文件控制算法参数，是学习 OI 实际编码的最佳起点。

## 核心代码结构

```
optimal-interpolation/
├── si.conf              # 配置文件（OI 参数）
├── optimal_interpolation.ipynb  # Jupyter Notebook 版本
├── run_si.bash          # Shell 运行脚本
├── src/
│   ├── si.py            # 核心 OI 算法
│   ├── read_data.py     # 数据读取
│   └── utils.py         # 工具函数
└── data/                # 示例数据
```

## OI 配置文件 (`si.conf`)

```ini
# 背景误差方差与总误差方差的比值
background_error_variance_ratio = 0.5

# 去相关长度尺度（单位取决于数据坐标——度或公里）
decorrelation_length_scale = 1.5

# 总误差方差（分析误差估计的比例因子）
total_error_variance = 1.0
```

### 参数含义

| 参数 | 物理含义 | 调大效果 | 调小效果 |
|:---|:---|:---|:---|
| `background_error_variance_ratio` | 背景场相对于总误差的方差占比 | 更信任观测 | 更信任背景场 |
| `decorrelation_length_scale` | 观测影响的空间范围 | 更平滑、更大范围影响 | 更贴近单点观测 |
| `total_error_variance` | 分析场误差估计的整体缩放 | 更大的误差估计 | 更小的误差估计 |

## 算法流程

```
1. 读取背景场（格点 NetCDF）
2. 读取观测数据（站点 lat/lon/value）
3. 对每个格点：
   a. 找到在 decorrelation_length_scale 范围内的所有观测
   b. 计算背景误差协方差：B_ij = σ²_b × exp(-d_ij² / L²)
   c. 计算观测误差协方差：R = σ²_o × I（对角矩阵）
   d. 构建 Kalman 增益：K = B × H^T × (H × B × H^T + R)^{-1}
   e. 分析场 = 背景场 + K × (观测 - H×背景场)
4. 输出分析场（格点 NetCDF）
```

## 观测选择策略

只选择**影响半径内**的观测参与计算——这是协方差局地化（localization）的最简形式：

```python
# 对每个格点
distances = haversine(grid_point, all_observations)
nearby_obs = observations[distances < decorrelation_length_scale]
# 只用 nearby_obs 构建 K 矩阵
```

这大大降低了矩阵求逆的开销（从 O(N_obs³) 降到 O(N_nearby³)）。

## 运行方式

### 方式一：Shell 脚本
```bash
bash run_si.bash
```

### 方式二：Jupyter Notebook
```bash
jupyter notebook optimal_interpolation.ipynb
```

## 扩展建议

### 从 2D 到 3D
```python
# 原始：2D 协方差（仅水平）
dist = sqrt((lon_i - lon_j)^2 + (lat_i - lat_j)^2)
B_ij = exp(-dist^2 / L_h^2)

# 扩展：3D 协方差（水平 + 深度）
dist_3d = sqrt(d_h^2 / L_h^2 + d_z^2 / L_z^2)
B_ij = exp(-dist_3d^2)
```

### 从高斯到更复杂的协方差模型
```python
# 高斯（原始）
B_ij = exp(-d^2 / L^2)

# 指数
B_ij = exp(-|d| / L)

# Matérn（更灵活的海洋应用）
import scipy.special
B_ij = (d/L)^nu * scipy.special.kv(nu, d/L)  # nu=0.5 退化为指数
```

## 优点

- **极简实现**：核心 OI 算法不到 200 行 Python
- **配置文件驱动**：无需改代码即可调整参数
- **Jupyter 支持**：适合交互式学习
- **公开可运行**：clone 后即可运行

## 局限

- 仅支持 2D 同化
- 协方差模型固定为高斯函数
- 不支持多变量同化（如 T 和 S 联合）
- 无海岸线/边界约束
- 适用于教学和中小规模数据，大规模需优化
