# UMD-AOSC/DA_Tutorial — 马里兰大学数据同化教程

- **GitHub**: https://github.com/UMD-AOSC/DA_Tutorial
- **语言**: Python (Jupyter Notebook)
- **作者**: 马里兰大学大气与海洋科学系（UMD AOSC）
- **适用场景**: 从零开始学习数据同化的理论基础

## 概述

这是一个从基础到进阶的数据同化教程，使用 Python 和 Jupyter Notebook 格式。从经典的 Lorenz-63 混沌模型出发，逐步讲解最优插值、3D-Var、集合卡尔曼滤波（EnKF）等核心方法。

## 教程路线

### 1. Lorenz-63 混沌系统
- 理解"蝴蝶效应"和数据同化的必要性
- 构建最简单的预报-同化循环

### 2. 最优插值（OI）
- Kalman 增益推导
- 背景误差协方差的构建
- 观测误差协方差的指定
- 协方差局地化

### 3. 3D-Var
- 变分形式
- 与 OI 的等价性
- 代价函数和梯度下降

### 4. 扩展 Kalman 滤波（EKF）
- 非线性系统的线性化
- 切线性模式和伴随模式

### 5. 集合 Kalman 滤波（EnKF）
- 蒙特卡洛误差传播
- 集合大小的影响
- 协方差膨胀和局地化

## 快速开始

```bash
git clone https://github.com/UMD-AOSC/DA_Tutorial.git
cd DA_Tutorial
jupyter notebook
```

按照编号顺序运行 Notebook。

## 最相关的 Notebook

对于你的场景，以下 Notebook 最值得关注：

### `DA_optimal_interpolation.ipynb`
直接从 Lorenz-63 上的 OI 实现学习核心算法。虽然是低维教学模型，但 OI 的数学原理与高维海洋问题是完全一致的。

### `DA_covariance_localization.ipynb`
协方差局地化是关键——在海洋中，相隔几千公里的两点之间的温度和盐度可能几乎独立。这个 Notebook 演示了为何局地化对大规模问题至关重要。

## 从 Lorenz-63 到真实海洋

| Lorenz-63 中的概念 | 海洋对应 |
|:---|:---|
| 3 个状态变量 (x,y,z) | 10⁶-10⁸ 个格点 × 多层 × 多变量 |
| 背景误差协方差 (3×3) | B 矩阵 (10⁸×10⁸ 但从不显式构建) |
| 观测算子 H = I | 从格点到 Argo 位置的插值 |
| 噪声观测 | 真实 Argo/CTD 测量误差 |

教程的价值在于：在一个你可以完全理解的简单系统上，验证你对 OI/EnKF 的直觉。当你将方法扩展到真实海洋时，这些直觉会防止你犯根本性错误。
