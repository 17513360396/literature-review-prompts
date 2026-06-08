# IAP 数据集方法论文献精要

## 背景

IAP（Institute of Atmospheric Physics）海洋格点数据集由中科院大气物理研究所**成里京、朱江**团队开发，是全球海洋次表层温度和盐度的主要格点产品之一。

## 核心方法：EnOI-DE（集合最优插值 + 动态集合）

### 方法论文

**Cheng, L. & Zhu, J. (2016)**
- **标题**: Benefits of CMIP5 Multimodel Ensemble in Reconstructing Historical Ocean Subsurface Temperature Variation
- **期刊**: *Journal of Climate*, 29(15), 5393–5416
- **DOI**: 10.1175/JCLI-D-15-0730.1

#### 核心创新

传统 OI 的背景误差协方差基于**统计假设**（如高斯距离衰减函数），而 IAP 的 EnOI-DE 使用 **CMIP5 多模式历史模拟**构建**物理驱动的动态集合**。

#### 方法步骤

```
第1步：收集 CMIP5 多模式历史模拟的海洋温度输出
第2步：对每个模式计算其相对于自身气候态的异常
第3步：所有模式的异常场组合成一个大集合
第4步：从集合中估计背景误差协方差 B
第5步：用标准 OI 公式计算分析场
        X_a = X_b + B × H^T × (H × B × H^T + R)^{-1} × (Y - H × X_b)
```

#### 为什么用 CMIP 模式？

1. 海洋模式包含完整的海洋动力学（平流、混合、温跃层等），其协方差能反映物理真实
2. 多模式集合能减少单个模式的系统偏差
3. 模式覆盖了广泛的时间变率（季节到年代际），提供了丰富的协方差信息
4. 相比纯统计协方差，能更好地处理以下情况：
   - 不同海盆之间的遥相关
   - 温跃层深度的经向变化
   - ENSO 等气候模态的三维结构
   - 赤道波导、西边界流等动力学结构

### 最新版 IAPv4

**Cheng, L., Pan, Y., Tan, Z., et al. (2024)**
- **标题**: IAPv4 Ocean Temperature and Ocean Heat Content Gridded Dataset
- **期刊**: *Earth System Science Data*, 16, 3517–3546
- **DOI**: 10.5194/essd-16-3517-2024

#### IAPv4 完整处理流程

```
步骤1：数据收集
  └── WOD（世界海洋数据库）+ GTSPP（近实时更新）
       包含：XBT, CTD, Argo, MBT, Bottle, Glider, Mooring, 海洋哺乳动物传感器

步骤2：质量控制 (CODC-QC)
  └── 自动质控 + 人工审核
       剔除：重复剖面、位置错误、温盐范围异常、密度翻转等

步骤3：偏差校正
  ├── XBT: CH14 方案（Cheng et al. 2014）
  ├── MBT: 热惯性校正
  ├── Nansen Bottle: 系统偏差校正
  └── 海洋哺乳动物 TDR/SRDL: 传感器偏差校正

步骤4：垂直插值
  └── 每个剖面插值到标准深度层（0-2000m 共 41 层）

步骤5：气候态构建
  └── 更新气候态场（IAP 的气候态本身也是用 EnOI-DE 构建的）

步骤6：EnOI-DE 映射
  ├── 背景场 = 气候态
  ├── 背景误差协方差 = CMIP5/CMIP6 多模式动态集合
  ├── 观测 = 第4步处理后的剖面数据
  └── 输出 = 月平均 1°×1° 格点场（0-6000m）
```

## 数据获取

- **IAP 数据服务页**: http://www.ocean.iap.ac.cn/pages/dataService/dataService.html?navAnchor
- **说明 PDF**: http://www.ocean.iap.ac.cn/ftp/cheng/IAP_Ocean_heat_content_anomaly_0_2000m/IAP_Global_ocean_gridded_product.pdf

## 与其他主流产品的对比

| 产品 | 方法 | 协方差来源 | 深度覆盖 |
|:---|:---|:---|:---|
| **IAP** | EnOI-DE | CMIP 多模式动态集合 | 0-6000m |
| **EN4 (UK Met Office)** | OI | 基于距离的统计协方差 | 0-5500m |
| **Ishii (JMA)** | OI | 基于距离的统计协方差 | 0-3000m |
| **WOA (NOAA)** | OI/Kriging | 基于距离的统计协方差 | 0-5500m |

IAP 的关键优势：
- **物理驱动的协方差**（不是简单的距离函数）
- **子采样测试**验证历史重建的无偏性（这是 IAP 独有的做法）
- 追溯到 1957 年的长期一致性

## 实现建议

如果你要复现/简化 IAP 方法：

### 简化方案 A：传统 OI + IAP 背景场
- 背景场 = IAP 气候态
- 协方差 = 高斯距离衰减
- 工具 = `ZC-BOM/optimal-interpolation` 或 `DIVAnd.jl`
- 优点：实现简单
- 缺点：丢失 EnOI-DE 的物理协方差优势

### 完整方案 B：EnOI-DE
- 背景场 = IAP 气候态
- 协方差 = CMIP5/6 多模式集合
- 工具 = `Leo-Rain/Optimal_interpolation` + 自建集合
- 优点：接近 IAP 原始方法
- 缺点：需要下载和处理 CMIP 数据

### 混合方案 C：滑动历史快照集合
- 背景场 = IAP 月平均数据
- 协方差 = 同一月份过去 N 年的 IAP 产品快照
- 工具 = 任一 OI/EnOI 代码
- 优点：不需要 CMIP 输出
- 缺点：集合大小有限（~N 年）

## 关键注意事项

1. **偏差校正至关重要**：不同仪器（XBT vs Argo）之间必须进行偏差校正，否则"同化"实际上是在混合有偏的数据
2. **子采样测试**：用 Argo 时代的密集观测验证历史稀疏观测时期的重建能力
3. **信号衰减**：历史时期（1950s-1960s）的观测稀疏导致变率信号衰减，这在 IAP 方法中通过 CMIP 集合提供了部分补偿
