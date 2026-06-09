# DA-tutorials — Nansen 中心最优插值 Jupyter 教程

- **GitHub**: https://github.com/nansencenter/DA-tutorials
- **语言**: Python (Jupyter Notebook) | **Stars**: ~155
- **作者**: Nansen Environmental and Remote Sensing Center (挪威)
- **适用场景**: 从零学习数据同化（含最优插值）的最佳入门教程

## 概述

这是**学习最优插值最推荐的教程仓库**。它用渐进式 Jupyter Notebook 设计，从最优插值的基本概念出发，逐步过渡到 Bayes 定理、Kalman 滤波、集合 Kalman 滤波（EnKF）等进阶方法。每个教程约 75 分钟，包含理论讲解 + 代码实现 + 练习题。

## 教程结构

### 第1部分：基础概念（含最优插值）

| 教程 | 主题 | 内容 |
|:---|:---|:---|
| T1 | DA 导论 | 什么是数据同化，为什么需要 |
| T2 | **最优插值** 🔴 | 最小方差估计，BLUE，Kalman 增益推导 |
| T3 | Bayes 定理 | 贝叶斯视角下的 DA |
| T4 | Kalman 滤波 | 递归贝叶斯估计 |

### 第2部分：进阶方法

| 教程 | 主题 |
|:---|:---|
| T5 | 集合 Kalman 滤波（EnKF）原理 |
| T6 | EnKF 算法实现 |
| T7 | 协方差局地化与膨胀 |
| T8+ | 更多进阶主题 |

## 最优插值 Notebook 详解（T2）

这是最核心的教程，涵盖：

```
1. 问题设定
   - 状态向量 x，背景 x_b，观测 y
   - 误差定义：ε_b = x_b - x_true, ε_o = y - H(x_true)

2. BLUE 推导
   - 分析 x_a = x_b + K (y - H x_b)
   - 最小化分析误差方差 → 导出 K

3. Kalman 增益矩阵
   - K = B H^T (H B H^T + R)^{-1}
   - 各矩阵的含义和构建

4. 简单示例
   - 1D 温度同化数值示例
   - 可视化分析增量
```

## 运行方式

### 在线运行（无需安装）

- **Google Colab**: 直接打开 GitHub 上的 .ipynb 文件
- **Binder**: https://mybinder.org/ （一键运行）

### 本地运行

```bash
git clone https://github.com/nansencenter/DA-tutorials.git
cd DA-tutorials
pip install -r requirements.txt
jupyter notebook
# 进入 notebooks/ 文件夹，按编号顺序运行
```

## 与 DAPPER 的关系

| 方面 | DA-tutorials | DAPPER |
|:---|:---|:---|
| 定位 | 教学 | 研究/基准测试 |
| 复杂度 | 低（每个 Notebook 独立） | 高（模块化框架） |
| 数学推导 | 详细 | 较少 |
| 动手练习 | 有 | 无 |
| **推荐初学者** | ✅ 先看这个 | 学完再看 |

## 在学习路径中的位置

DA-tutorials 是整个 OI 学习的**第一站**：

```
DA-tutorials (理解 OI 的数学本质)
    ↓
ZC-BOM/optimal-interpolation (看 OI 的实际 Python 实现)
    ↓
DAPPER (对比 OI 与其他方法如 EnKF/4D-Var)
    ↓
EnKF-C / 具体应用 (真实数据同化)
```

## 配套理论

建议搭配以下教材阅读：
- **Kalnay (2003)**: *Atmospheric Modeling, Data Assimilation and Predictability* — 第5章
- **Daley (1991)**: *Atmospheric Data Analysis* — 第4-5章
