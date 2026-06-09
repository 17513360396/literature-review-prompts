# pyinterpolate — Python 空间 Kriging 插值库

- **GitHub**: https://github.com/DataverseLabs/pyinterpolate
- **PyPI**: `pip install pyinterpolate` | `conda install -c conda-forge pyinterpolate`
- **语言**: Python | **Stars**: ~92 | **License**: BSD-3-Clause
- **JOSS 论文**: doi:10.21105/joss.02869
- **作者**: Szymon Moliński
- **适用场景**: 点观测数据的空间统计插值（地统计学）

## 与最优插值的区别

Kriging 与最优插值在**数学结构上等价**，但术语体系和实现范式不同：

| 概念 | 气象 OI | 地统计 Kriging |
|:---|:---|:---|
| 协方差结构 | 背景误差协方差 B | Variogram / Covariogram |
| 评估指标 | 分析 RMSE | Kriging variance |
| "背景场" | 来自数值天气预报模式 | 常为空间常数（Ordinary Kriging） |
| "观测算子" | 三维空间的插值算子 | 通常是二维（空间） |
| 参数确定 | 基于已知的误差统计 | Variogram 拟合 = 从数据学习 |

## 支持的插值方法

```python
from pyinterpolate import (
    ordinary_kriging,      # 普通 Kriging（假设均值未知但常数）
    simple_kriging,        # 简单 Kriging（已知均值）
    universal_kriging,     # 泛 Kriging（趋势+残差）
    poisson_kriging,       # Poisson Kriging（面域数据）
    indicator_kriging,     # 指示 Kriging（概率估计）
    inverse_distance_weighting  # IDW（基准对比方法）
)
```

## 完整工作流

### 1. 计算实验 Variogram
```python
from pyinterpolate import ExperimentalVariogram

exp_vario = ExperimentalVariogram(
    input_array=point_data,  # [x, y, value]
    step_size=500,           # 滞后距步长
    max_range=40000          # 最大搜索范围
)
```

### 2. 拟合理论 Variogram 模型
```python
from pyinterpolate import build_theoretical_variogram

semivar = build_theoretical_variogram(
    experimental_variogram=exp_vario,
    model_name='spherical',  # 或 'exponential', 'gaussian', 'linear'
    nugget=0.0,
    sill=exp_vario.variance,
    rang=8000
)
```

### 3. 执行 Kriging 插值
```python
from pyinterpolate import ordinary_kriging

predictions = []
for grid_point in grid:
    pred = ordinary_kriging(
        theoretical_model=semivar,
        known_locations=point_data,
        unknown_location=grid_point,
        no_neighbors=32          # 仅使用最近的 32 个点
    )
    # pred = [value, error_variance, lon, lat]
    predictions.append(pred)
```

## 从 Variogram 理解协方差

这是 Kriging 最有教学价值的部分：**从数据估计空间协方差结构**。

### 实验 Variogram
```
γ(h) = 1/(2N(h)) × Σ (z(x_i) - z(x_i + h))²

其中：
  h  = 滞后距（两点的空间距离）
  N(h) = 距离为 h 的点对数量
```

### Variogram 三参数

```
γ(h)
↑
│         .--sill────────────
│       .'
│     .'
│   .'  range → 相关变为零的距离
│ .'
│'─────────────→ h
│ nugget → h=0 处的不连续性（测量误差 + 微尺度变异）
```

这三个参数与 OI 中协方差参数的对应：

| Variogram 参数 | OI 对应 | 物理意义 |
|:---|:---|:---|
| `sill` | `σ²_b`（背景误差方差） | 空间变率的总幅度 |
| `range` | 局地化半径 | 空间相关的最大距离 |
| `nugget` | `σ²_o`（观测误差方差的一部分） | 两次重复测量不会完全一致 |

## 为什么学习 Kriging 有助于理解 OI

1. **协方差/变差函数的直观构建**：Variogram 的可视化（实验半方差随距离的变化）比抽象描述"背景误差协方差"要直观得多
2. **参数自动学习**：OI 的 `decorrelation_length_scale` 需要手动调参；Kriging 通过 Variogram 拟合自动确定
3. **术语对照**：通过对比理解 OI 中"背景误差协方差 B"与 Kriging 中"sill - semi-variance"的对应关系

## 安装

```bash
pip install pyinterpolate
# 或
conda install -c conda-forge pyinterpolate
```

需要 Python ≥ 3.10，依赖 geopandas, numpy, scipy, matplotlib, pydantic。

## 局限

- 不像 OI 那样支持明确的物理"背景场"（背景场在 Kriging 中被简化为常数或趋势）
- 不支持多变量联合分析（co-Kriging 尚未实现）
- 纯 Python 实现，大数据集可能较慢
