# EnKF-C — 业务级 EnOI 实现（澳大利亚气象局）

- **GitHub**: https://github.com/sakov/enkf-c
- **语言**: C（MPI 并行） | **Stars**: ~35
- **作者**: Pavel Sakov, 澳大利亚气象局
- **用户指南**: https://arxiv.org/abs/1410.1233
- **适用场景**: 大规模地球物理模型（海洋/大气）的生产级数据同化

## 概述

EnKF-C 是**全球唯一公开的、在生产环境中验证过的 EnOI C 代码**。它被澳大利亚气象局用于业务化全球 SST 分析系统（GSAS，0.1°×0.1° 每日 SST 产品）。理解它对"OI 在实际中如何运行"有极大帮助。

## 支持的 DA 模式

```
EnKF-C 的三种运行模式：

1. EnKF (Ensemble Kalman Filter)
   动态集合 → 流依赖协方差 → 成本最高

2. EnOI (Ensemble Optimal Interpolation)  ← 你关注的
   静态集合 → 气候态协方差 → 成本最低

3. Hybrid EnKF/EnOI
   动态 + 静态混合 → 成本与效果折中
```

## EnOI 模式详解

### 静态集合来源

EnOI 不需要在线传播集合成员，而是使用**预先生成的静态集合**：

```
离线阶段：
  历史模式长时间积分（如 OFAM3 海洋模式 10+ 年）
  → 保存每日/每月快照
  → 减去气候态 → 得到集合扰动 A'

在线阶段：
  读取预存的 A'
  → 计算 B = A' × A'^T / (N-1)
  → 执行标准 OI 分析
```

### 协方差局地化

C 代码中局地化的实现：

```c
// 概念示意
// 对每个格点 i：
for each grid_point i:
    for each observation j:
        distance = haversine(i, j);
        if (distance > localization_radius)
            influence[j] = 0;  // 这个观测不影响格点 i
        else
            influence[j] = taper(distance / localization_radius);
```

### 超级观测化（Super-Observation）

EnKF-C 的一个关键技术——对密集观测进行稀疏化：

```
原始观测（密集）：
   10000 个卫星 SST 观测
       ↓ 空间聚合（如 25km 内取平均）
  超级观测（稀疏）：
   500 个超级观测

好处：
  - 减少矩阵求逆维数
  - 减少代表性误差
  - 保持信息含量
```

## GSAS 系统的实际运行

澳大利亚气象局的全球 SST 分析系统：

| 参数 | 设置 |
|:---|:---|
| 分辨率 | 0.1° × 0.1° |
| 更新频率 | 每日 |
| 背景场 | 前一天的分析场 |
| 观测来源 | VIIRS, AMSR2, AVHRR (多卫星) |
| 静态集合 | OFAM3 海洋模式 10 年历史输出 |
| 超级观测网格 | ~10km 分辨率 |
| 代码 | EnKF-C (EnOI 模式) |

## 快速开始

```bash
# 克隆并编译
git clone https://github.com/sakov/enkf-c.git
cd enkf-c
make

# 运行示例（包含 EnKF 和 EnOI 示例）
cd examples
./run_example.sh
```

## 学习价值

即使你不需要 C 级性能，EnKF-C 也有以下学习价值：

1. **用户指南**（arXiv:1410.1233）是 EnOI 原理和调参的绝佳读物
2. **源码**展示了协方差局地化、观测质量控制等在生产中如何具体实现
3. **配置示例**展示了真实海洋同化系统的参数设置
4. **GSAS 案例**展示了 EnOI 从研究到业务的完整路径

## 与其他教程的关系

```
DA-tutorials          →  理解 OI 数学原理
ZC-BOM               →  看 OI 的 Python 教学实现
Leo-Rain             →  理解 EnOI 从 OI 的过渡
EnKF-C + User Guide  →  理解业务化 EnOI 的实际细节   ← 当前文件
```

## 关键参数（来自用户指南和 GSAS 经验）

| 参数 | 推荐范围 | 说明 |
|:---|:---|:---|
| 集合大小 N | 20-100 | 太小 → 协方差噪声大；太大 → 存储成本高 |
| 局地化半径 | 500-1500 km (SST) | 需要根据去相关尺度确定 |
| 膨胀因子 | 1.0-1.2 | 补偿集合采样误差 |
| 超级观测网格 | 10-25 km | 匹配观测密度和模式分辨率 |
