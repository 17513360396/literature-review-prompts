# atmos_flux_inversion — 大气反演中的最优插值

- **GitHub**: https://github.com/psu-inversion/atmospheric-inverse-methods-for-flux-optimization
- **文档**: https://psu-inversion.github.io/atmospheric-inverse-methods-for-flux-optimization/
- **语言**: Python | **作者**: Penn State University
- **适用场景**: CO₂ 等大气示踪物的源汇反演

## 概述

这是一个独特的学习资源——它从**大气反演**的角度实现了最优插值。与传统气象/海洋 DA 中的 OI 不同，这里的 OI 用于**大气成分的源汇估算**（如从 CO₂ 浓度观测反推地表碳通量）。这一视角的转换能帮助你更深刻地理解 OI 的统计本质。

## OI 在大气反演中

### 问题设定

```
目标：从大气 CO₂ 浓度观测推断地表碳通量

已知：
  y = H × x + ε_obs     （大气传输方程）
  x_b ~ N(x_true, B)    （先验通量估计）

求：
  x_a（后验通量 = 最优估计）

解：
  x_a = x_b + B H^T (H B H^T + R)^{-1} (y - H x_b)
```

这与气象 OI 的公式完全相同，但物理含义不同：

| 符号 | 气象 OI 含义 | 大气反演 OI 含义 |
|:---|:---|:---|
| x | 大气状态（温度、风、湿度） | 地表通量（CO₂ 排放） |
| y | 气象观测 | 大气浓度测量 |
| H | 从格点到观测站的插值算子 | 大气输送模型（从天到周的积分） |
| B | 天气预报误差协方差 | 先验通量不确定性 |

## 三种求解器对比

代码提供了三种 OI 求解方式，这本身就是一个极佳的教学素材：

### 1. 直接矩阵求逆（simple）
```python
# 直接实现 BLUE 公式
K = B @ H.T @ np.linalg.inv(H @ B @ H.T + R)
x_a = x_b + K @ (y - H @ x_b)
```
- **适用**：小型问题（状态维度 < 1000）
- **教学价值**：映射公式最直接

### 2. Cholesky 分解（scipy_chol）
```python
# 数值更稳定
S = H @ B @ H.T + R
L = np.linalg.cholesky(S)  # S = L @ L.T
z = np.linalg.solve(L, y - H @ x_b)
x_a = x_b + B @ H.T @ np.linalg.solve(L.T, z)
```
- **适用**：中型问题
- **教学价值**：展示了为什么"不要直接求逆"（cholesky 更稳定、更快）

### 3. 公共子表达式优化（fold_common）
```python
# 利用 H^T R^{-1} H 的结构预计算
HTRinv = H.T @ np.linalg.solve(R, np.eye(n_obs))
# 然后加速后续求解
```
- **适用**：同化窗口内 H 不变化的情况
- **教学价值**：展示减少重复计算的实际技巧

## 安装与运行

```bash
pip install "git+https://github.com/psu-inversion/atmospheric-inverse-methods-for-flux-optimization.git"
```

## 学习这个仓库的独特收获

| 收获 | 说明 |
|:---|:---|
| **OI 的统计本质** | 剥去气象/海洋的物理外衣，看到纯粹的统计学框架 |
| **B 矩阵的构建** | 大气反演中 B 通常基于先验通量不确定性（~50% 误差），与气象中的模式气候态非常不同 |
| **H 矩阵的复杂性** | 大气输送模型作为观测算子——比简单的空间插值复杂得多 |
| **多种求解器的对比** | 一本"OI 数值实现"的教科书 |
| **降维技术** | 支持缩减状态空间（粗网格通量），减少矩阵求逆规模 |

## 在 OI 学习路径中的位置

```
Step 1: DA-tutorials       →  理解 OI 在气象中的标准形式
Step 2: ZC-BOM             →  看 OI 的最简 Python 实现
Step 3: atmos_flux_inversion  →  从反演/统计视角深化 OI 理解   ← 当前
Step 4: DAPPER/EnKF-C      →  对比更复杂的同化方法
```
