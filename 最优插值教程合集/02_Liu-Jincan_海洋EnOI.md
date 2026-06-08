# Liu-Jincan/Data-Assimilation-for-Ocean-Current-Forecasts — 海洋 EnOI 数据同化

- **GitHub**: https://github.com/Liu-Jincan/Data-Assimilation-for-Ocean-Current-Forecasts
- **语言**: MATLAB (44.1%), Shell (45.6%), Fortran (7.1%)
- **作者**: Liu-Jincan（博士后项目）
- **适用场景**: 海洋流场预报的 EnOI 数据同化

## 概述

这是目前 GitHub 上**最直接匹配"海洋 EnOI 数据同化"的仓库**。项目构建了一个完整的 EnOI（Ensemble Optimal Interpolation）数据同化模型，用于海洋流场预报。更重要的是，它包含了教程目录 `TUTORIAL_GRIDGEN/`，对学习者非常友好。

## 仓库结构

```
项目根目录/
├── DA-Code/              # 核心数据同化代码
│   ├── EnOI/             # Ensemble Optimal Interpolation 实现
│   └── ...               # 其他数据同化方法
├── TUTORIAL_GRIDGEN/     # 🎓 网格生成教程
├── TrendTendency_EVA/    # 趋势/倾向评估
├── ndbc/                 # NDBC 浮标观测数据处理
├── work_eastUSA.sh       # 美国东部海域工作流
├── work_eastUSA_2.sh     # 同上（不同配置）
└── ...                   # 其他脚本
```

## 核心方法：Ensemble Optimal Interpolation

### 与传统 OI 的区别

| 方面 | 传统 OI | EnOI |
|:---|:---|:---|
| 背景误差协方差来源 | 预设统计模型（如高斯函数） | 模式集合统计 |
| 协方差物理性 | 仅基于距离 | 反映海洋动力学关系 |
| 计算成本 | 低 | 中等（需维护集合） |
| 流依赖（flow-dependent） | 无 | 有 |
| 多变量相关（如 T-S 协变） | 难以建模 | 自然包含 |

### EnOI 算法核心

EnOI 使用**静态模式集合**（如历史长时间序列的模式快照）来估计背景误差协方差：

```
B ≈ 1/(N-1) × A' × A'^T
```

其中 `A'` 是包含 N 个集合成员的集合扰动矩阵。

每个集合成员代表一个可能的背景状态：
- 可以是同一模式不同时间的快照
- 可以是多模式的历史模拟（IAP 方法的核心）
- 可以是不同参数方案的扰动

对于分析增量：
```
X_a = X_b + K × (Y - H × X_b)
K = B × H^T × (H × B × H^T + R)^(-1)
```

## Shell 脚本工作流

该仓库使用 Shell 脚本串联完整流程，以 `work_eastUSA.sh` 为例：

1. 准备网格和地形数据
2. 运行海洋模式（Fortran）
3. 读取观测数据（NDBC 浮标）
4. 执行 EnOI 同化（MATLAB）
5. 输出分析场
6. 评估同化效果

## TUTORIAL_GRIDGEN 教程模块

该目录包含网格生成的教程代码，可用于学习：
- 如何为海洋区域创建计算网格
- 如何生成地形文件
- 网格质量控制

## 适配海洋温盐融合

该仓库的 EnOI 实现框架可直接适配你的场景：

1. **背景场**：将模式预报场替换为 IAP 月平均格点数据
2. **观测数据**：将 NDBC 浮标替换为 Argo/CTD/XBT 等实测剖面
3. **状态变量**：将海流替换为温度、盐度（或联合同化 T+S）
4. **集合生成**：参考 IAP 方法，使用 CMIP 多模式历史模拟构建静态集合

## 关键参考论文

- Fu et al. (2011): 北大西洋/波罗的海 EnOI 温盐同化
- 2025 Applied Ocean Research: 东南亚/南海 EnOI 在 NEMO 中的集成
- Cheng & Zhu (2016): IAP 的 EnOI-DE 方法
