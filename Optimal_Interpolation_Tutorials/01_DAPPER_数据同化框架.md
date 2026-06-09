# DAPPER — 数据同化研究基准框架

- **GitHub**: https://github.com/nansencenter/DAPPER
- **文档**: https://nansencenter.github.io/DAPPER/
- **语言**: Python | **License**: MIT | **Stars**: ~370
- **作者**: Patrick Raanes, Nansen Environmental and Remote Sensing Center (挪威)
- **适用场景**: 数据同化方法的教学、研究、基准测试

## 概述

DAPPER（Data Assimilation with Python: a Package for Experimental Research）是目前**最全面的 Python 数据同化方法库**。它包含从经典的 OptInterp（最优插值）到前沿的粒子滤波等全套 DA 方法实现，特别适合用来理解不同方法之间的关系和进行方法间对比。

## 包含的 DA 方法

```
dapper/da_methods/
├── baseline/       # 🔴 Optimal Interpolation (OptInterp)
│                   #    使用静态气候态协方差的最简 DA 方法
├── ensemble/       # EnKF, EnKS 等集合方法
├── extended/       # EKF, RTS smoother
├── variational/    # 4D-Var, iEnKS
├── particle/       # 粒子滤波
└── hybrid/         # EnKF/EnOI 混合
```

## OptInterp 的实现

在 DAPPER 中，OptInterp（OI）被归类为 `baseline` 方法——即使用**静态/气候态先验协方差**的 Kalman 滤波分析步骤：

```python
# 概念示意（非实际 API）
from dapper.da_methods import OptInterp

# 静态协方差 — 不随时间演变
# 等价于：K = B_clim × H^T × (H × B_clim × H^T + R)^{-1}
# 分析步骤 x_a = x_b + K × (y - H × x_b)
```

与传统 OI 的唯一区别是：OI 使用预定义的气候态协方差 B_clim，而 Kalman 滤波使用通过模式动态传播的流依赖协方差。这使得 OptInterp 成为基准（baseline）方法——它比完整 Kalman 滤波简单，但计算量小得多。

## 内置测试模型

DAPPER 包含多个经典测试模型（`dapper/mods/`），方便在不接触真实数据的情况下学习 DA：

| 模型 | 维度 | 特点 |
|:---|:---|:---|
| Lorenz-63 | 3 | 混沌，教学经典 |
| Lorenz-84 | 3 | 大气环流简化 |
| Lorenz-96 | 可配置 | 环状耦合，空间结构 |
| Lotka-Volterra | 2 | 生态模型 |
| Kuramoto-Sivashinsky | 128 | 时空混沌 |

## 快速开始

```bash
git clone https://github.com/nansencenter/DAPPER.git
cd DAPPER
pip install -e .

# 运行示例实验（使用 Lorenz-63 模型 + OI）
python -m dapper.xp_launch
```

## 与你的学习需求对应

| 你想了解的 | DAPPER 中的对应 |
|:---|:---|
| OI 的数学本质 | `baseline/OptInterp` — 最简 DA 方法 |
| OI vs EnKF 的区别 | `baseline/` vs `ensemble/EnKF` |
| 协方差局地化 | 所有 ensemble 方法中的 localization |
| 观测误差协方差指定 | 所有方法中 R 矩阵的构建 |
| 方法对比框架 | `xp_launch` + `xp_process` |

## 优点

- 方法最全：OI、EnKF、4D-Var、粒子滤波全部在一个框架下
- 统一接口：方便直接对比不同方法
- 文档完善：有完整的方法参考文档
- 教学友好：内置简单模型（Lorenz 系列），无需真实数据

## 局限

- 不面向业务化/真实数据同化（没有 I/O 处理真实观测的流程）
- 依赖较多（NumPy, SciPy, matplotlib 等）
- 对初学者来说框架较复杂（建议先看 `DA-tutorials` 而非 DAPPER 本身）
