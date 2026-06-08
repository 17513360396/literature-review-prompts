# 最优插值（OI）海洋温盐数据融合 — GitHub 资源合集

> 整理时间：2026-06-08  
> 目标：搜集基于最优插值法进行海洋温、盐数据融合的教程与代码（背景场 → IAP 温盐数据集；待融合数据 → 多源实测站点温盐剖面）。  
> 若无完全匹配的教程，则收录最接近的 OI/EnOI 最优插值实现。

---

## 目录

1. [核心最优插值（OI/EnOI）代码仓库](#1-核心最优插值oienoi代码仓库)
2. [变分插值方法（OI 的泛化等价形式）](#2-变分插值方法oi-的泛化等价形式)
3. [海洋插值实战教程与 Workshop](#3-海洋插值实战教程与-workshop)
4. [神经网络/深度学习海洋插值](#4-神经网络深度学习海洋插值)
5. [海洋数据处理与辅助工具](#5-海洋数据处理与辅助工具)
6. [数据同化通用教程](#6-数据同化通用教程)
7. [中文资源](#7-中文资源)
8. [IAP 数据集核心方法论文献](#8-iap-数据集核心方法论文献)
9. [关键论文推荐](#9-关键论文推荐)
10. [总结与推荐路径](#10-总结与推荐路径)

---

## 1. 核心最优插值（OI/EnOI）代码仓库

### 1.1 ZC-BOM/optimal-interpolation ⭐ 强烈推荐

- **地址**: https://github.com/ZC-BOM/optimal-interpolation
- **语言**: Python
- **简介**: 澳大利亚气象局（BOM）的 Optimal Interpolation 实现。可配置背景误差方差比、去相关长度尺度、总误差方差等参数。支持 Unix shell 脚本和 Jupyter Notebook 两种运行方式。
- **适用性**: 虽然原用于降雨数据，但 OI 算法核心与海洋温盐融合完全通用。代码简洁清晰，是学习 OI 算法的最佳起点。
- **关键特点**:
  - 配置文件驱动（`si.conf`），参数可调
  - 读取站点观测数据 → 输出格点化同化结果
  - Python + Shell 双语支持

### 1.2 Liu-Jincan/Data-Assimilation-for-Ocean-Current-Forecasts ⭐ 高度相关

- **地址**: https://github.com/Liu-Jincan/Data-Assimilation-for-Ocean-Current-Forecasts
- **语言**: MATLAB (44.1%), Shell (45.6%), Fortran (7.1%)
- **简介**: 博士后项目，构建 EnOI（集合最优插值）数据同化模型用于海洋流场预报。**这是目前找到的最直接匹配海洋 EnOI 的仓库**。
- **目录结构**:
  - `DA-Code/` — 核心数据同化代码
  - `TUTORIAL_GRIDGEN/` — 网格生成教程
  - `TrendTendency_EVA/` — 趋势/倾向评估
  - `ndbc/` — NDBC 浮标数据处理
  - `work_eastUSA.sh` 等 — 工作流脚本
- **适用性**: 可直接参考其 EnOI 实现框架，替换背景场为 IAP 数据，观测为 Argo/CTD 等实测站点数据。

### 1.3 Leo-Rain/Optimal_interpolation ⭐ MATLAB EnOI 教学实现

- **地址**: https://gist.github.com/Leo-Rain/Optimal_interpolation
- **语言**: MATLAB
- **简介**: 原始最优插值 + Ensemble Optimal Interpolation 的纯 MATLAB 实现。包含 Kalman 增益的详细数学推导。虽然应用于北极风场同化，但算法核心完全通用。
- **关键特点**:
  - 基础 EnOI 实现（Kalman 增益公式）
  - 引入 Ridge 回归增强，减少非平稳、空间非均匀协方差造成的伪影
  - 有配套发表论文
  - 代码结构清晰，适合教学用途

### 1.4 ChakoChen/Data-Assimilation-for-Ocean-Current-Forecasts

- **地址**: https://github.com/ChakoChen/Data-Assimilation-for-Ocean-Current-Forecasts
- **语言**: MATLAB + Fortran
- **简介**: 与 Liu-Jincan 类似，也是 EnOI 数据同化模型用于海洋流场预报的博士后项目。可以对比参考。

---

## 2. 变分插值方法（OI 的泛化等价形式）

> 注：最优插值（OI）在数学上等价于变分分析（Variational Analysis）和 Kriging。以下工具本质上实现了 OI 的泛化版本。

### 2.1 gher-uliege/DIVAnd.jl ⭐ 海洋插值工业级工具

- **地址**: https://github.com/gher-uliege/DIVAnd.jl
- **语言**: Julia
- **Stars**: ~79 | **License**: GPL-2.0
- **简介**: n 维变分分析（Data-Interpolating Variational Analysis in n dimensions），是海洋学界最成熟的格点化工具之一。被 EMODnet、SeaDataCloud、BlueCloud 等欧洲海洋数据基础设施使用。
- **海洋特色功能**:
  - 海岸线/地形感知：陆海掩码防止信息跨陆地"泄漏"
  - 拓扑约束：自然处理半岛、岛屿、子海盆
  - 物理约束：支持平流约束、不等式约束
  - 误差场输出：生成分析误差方差图
  - 支持 4D 分析（lon × lat × depth × time）
- **核心函数**: `DIVAndrun`, `DIVAndgo`, `diva3D`, `DIVAnd_cv`
- **可调参数**: `len`（相关长度）、`epsilon2`（信噪比倒数）
- **引用**: Barth et al. (2014), GMD, doi:10.5194/gmd-7-225-2014

### 2.2 gher-uliege/DIVA (Fortran 原版)

- **地址**: https://github.com/gher-uliege/DIVA
- **语言**: Fortran
- **简介**: 原始 2D/3D/4D 有限元变分插值工具，已被 DIVAnd.jl 取代。仅用于理解历史方法。

---

## 3. 海洋插值实战教程与 Workshop

### 3.1 gher-uliege/Diva-Workshops ⭐⭐ 最佳教程资源

- **地址**: https://github.com/gher-uliege/Diva-Workshops
- **语言**: Jupyter Notebook (Julia)
- **简介**: DIVA/DIVAnd 的官方培训工作坊。包含 4 个递进式模块，是学习海洋数据格点化最完整的教程。
- **模块结构**:
  - `1-Intro/` — Julia 基础、netCDF 读写、可视化
  - `2-Preprocessing/` — 数据读取、水深地形、掩码
  - `3-Analysis/` — 参数优化、误差图、完整分析示例（⭐ 推荐）
  - `4-AdvancedTopics/` — 平流约束、多变量分析、不等式约束
- **重点 Notebook**: `3-07-example-analysis.ipynb`（完整分析流程）、`3-09-full-analysis.ipynb`（项目模板）
- **如何使用**:
  ```bash
  git clone https://github.com/gher-uliege/Diva-Workshops.git
  ```
  **推荐先看 `3-Analysis` 模块**，它展示了从观测到格点场的完整 OI 类分析流程。将 IAP 数据作为背景场、Argo/CTD 数据作为观测，即可直接适配你的场景。

### 3.2 gher-uliege/DIVAnd-ME4OH (实际应用示例)

- **地址**: https://github.com/gher-uliege/divand-me4oh
- **语言**: Julia
- **简介**: 海洋热含量密度的格点化实际应用案例。包含逐层分析和时间相关 3D 分析两种方案。

---

## 4. 神经网络/深度学习海洋插值

> 虽然不是传统 OI，但这些方法代表了海洋数据插值的前沿方向，可作为对比参考。

### 4.1 gher-uliege/DINCAE (Python, 已停维)

- **地址**: https://github.com/gher-uliege/DINCAE
- **语言**: Python (TensorFlow 1.15)
- **Stars**: 47
- **简介**: 使用卷积自编码器（Convolutional Auto-Encoder）重建卫星海洋观测中缺失的数据（SST、叶绿素等）。
- **状态**: 不再维护，被 Julia 版取代。
- **引用**: Barth et al. (2020), GMD, doi:10.5194/gmd-13-1609-2020

### 4.2 gher-uliege/DINCAE.jl (Julia, 活跃维护)

- **地址**: https://github.com/gher-uliege/DINCAE.jl
- **语言**: Julia (CUDA GPU 支持)
- **简介**: DINCAE 2.0 的 Julia 重写版，支持 GPU 加速和多变量重建（如联合 SST + 高度计）。
- **引用**: Barth et al. (2022), GMD, doi:10.5194/gmd-15-2183-2022

### 4.3 zhouweichen1992110/DINEOF-Python

- **地址**: https://github.com/zhouweichen1992110/DINEOF-Python
- **语言**: Python
- **简介**: DINEOF（Data Interpolating Empirical Orthogonal Functions）的 Python 实现，基于 EOF 的缺失数据重建方法。

---

## 5. 海洋数据处理与辅助工具

### 5.1 MaceKuaily/seaduck

- **地址**: https://github.com/MaceKuaily/seaduck
- **语言**: Python
- **简介**: 在复杂海洋模型网格（如 MITgcm、LLC4320）上进行插值的 Python 包。支持 Eulerian（固定点）和 Lagrangian（粒子平流）插值。
- **发布**: JOSS (2023)

### 5.2 ChanJeunlam/oceansdb

- **地址**: https://github.com/ChanJeunlam/oceansdb
- **语言**: Python
- **简介**: 海洋参考数据数据库，包含气候态和地形数据。可作为背景场/气候态数据的获取工具。

### 5.3 EMODnet/EMODnet-Biology-Phytoplankton-Interpolated-Maps

- **地址**: https://github.com/EMODnet/EMODnet-Biology-Phytoplankton-Interpolated-Maps
- **语言**: Python
- **简介**: EMODnet 生物数据插值制图实例，展示了从观测到格点化产品的完整生产流程。

---

## 6. 数据同化通用教程

### 6.1 UMD-AOSC/DA_Tutorial

- **地址**: https://github.com/UMD-AOSC/DA_Tutorial
- **语言**: Python (Jupyter Notebook)
- **简介**: 马里兰大学数据同化教程。从 Lorenz-63 模型出发，逐步讲解最优插值、3D-Var、EnKF 等方法。

### 6.2 RISDA 2018 Data Assimilation Tutorial

- **地址**: https://github.com/topics/ensemble-prediction (主题页)
- **简介**: 理化学研究所（RIKEN）国际数据同化学校的动手教程，Python/Jupyter Notebook 格式。

---

## 7. 中文资源

### 7.1 MATLAB 对海洋数据的最优插值

- **地址**: https://www.matlabcode.cn/shows/2/38727.html
- **简介**: 中文 MATLAB 最优插值海洋数据教程代码。

### 7.2 MATLAB 海洋最优插值（第二来源）

- **地址**: https://www.matlabol.com/article/6382.html
- **简介**: 另一份中文 MATLAB 最优插值海洋数据实现。

---

## 8. IAP 数据集核心方法论文献

> IAP 数据集使用的方法学名为 **"Improved Ensemble Optimal Interpolation with Dynamical Ensemble"**（EnOI-DE）。理解这些文献对实现至关重要。

### 8.1 Cheng & Zhu (2016) — 方法论文

- **标题**: Benefits of CMIP5 Multimodel Ensemble in Reconstructing Historical Ocean Subsurface Temperature Variation
- **期刊**: *Journal of Climate*, 29(15), 5393–5416
- **DOI**: 10.1175/JCLI-D-15-0730.1
- **核心内容**: 使用 CMIP5 多模式历史模拟构建动态集合，提供基于物理的背景误差协方差（而非仅基于距离假设的统计协方差）。这是 IAP 方法区别于传统 OI 的核心创新。

### 8.2 Cheng et al. (2017) — 应用与验证

- **标题**: Improved Estimates of Ocean Heat Content from 1960 to 2015
- **期刊**: *Science Advances*, 3, e1601545
- **DOI**: 10.1126/sciadv.1601545

### 8.3 Cheng et al. (2024) — IAPv4 最新描述

- **标题**: IAPv4 Ocean Temperature and Ocean Heat Content Gridded Dataset
- **期刊**: *Earth System Science Data*, 16, 3517–3546
- **DOI**: 10.5194/essd-16-3517-2024
- **核心内容**: IAPv4 所有方法论更新，包括 CODC-QC 质控系统、CH14 XBT 偏差校正方案、更新后的气候态场、EnOI-DE 映射方法。

### 8.4 IAP 数据获取

- **IAP 海洋数据服务页**: http://www.ocean.iap.ac.cn/pages/dataService/dataService.html?navAnchor
- **IAP 全球海洋格点产品 PDF**: http://www.ocean.iap.ac.cn/ftp/cheng/IAP_Ocean_heat_content_anomaly_0_2000m/IAP_Global_ocean_gridded_product.pdf

---

## 9. 关键论文推荐

### 9.1 EnOI 海洋温盐同化

- **Fu et al. (2011)**: Application of an Ensemble Optimal Interpolation in a North/Baltic Sea Model: Assimilating Temperature and Salinity Profiles. *Ocean Modelling*.
  - 直接在北大西洋/波罗的海模型中用 EnOI 同化温盐剖面数据，检验多变量特性。

### 9.2 EnOI 在东南亚/南海的应用

- **2025, Applied Ocean Research**: Development and Application of Ensemble Optimal Interpolation Data Assimilation on the Southeast Asia Region and Southwestern South China Sea.
  - 在 NEMO 海洋模型中集成 EnOI 代码，温盐 RMSE 持续降低，SST 偏差 < ±0.5°C。

### 9.3 EnOI 偏差校正

- **J. Mar. Sci. Eng. (2023)**: A Simple Bias Correction Scheme in Ocean Data Assimilation.
  - 使用 EN4.2.2 数据集的 EnOI 同化，分析了偏差校正对热含量和盐含量趋势的影响。

### 9.4 快速 OI 海洋映射

- **适应快速最优插值算法到海洋数据制图** (PDF): 关于将快速 OI 算法用于海洋格点化的技术报告。

---

## 10. 总结与推荐路径

### 现状分析

目前 GitHub 上**没有完全匹配"以 IAP 数据为背景场 + 多源实测站点温盐观测为待融合数据 + 最优插值法"的现成教程仓库**。但是，有大量可组合使用的高质量资源：

| 你的需求环节 | 最匹配资源 |
|:---|:---|
| **理解 OI 算法原理** | `ZC-BOM/optimal-interpolation` (Python), `Leo-Rain/Optimal_interpolation` (MATLAB) |
| **海洋 EnOI 实战代码** | `Liu-Jincan/Data-Assimilation-for-Ocean-Current-Forecasts` (MATLAB) |
| **海洋格点化完整教程** | `gher-uliege/Diva-Workshops` (Jupyter/Julia) |
| **工业级海洋插值工具** | `gher-uliege/DIVAnd.jl` (Julia) |
| **IAP 方法论理解** | Cheng & Zhu (2016) + Cheng et al. (2024) 文献 |
| **背景场数据获取** | IAP 数据服务页 + `ChanJeunlam/oceansdb` |

### 推荐实施路径

1. **学习 OI 原理** → 阅读 `Leo-Rain/Optimal_interpolation` MATLAB 代码（含 Kalman 增益推导）
2. **理解海洋 OI 实践** → 完成 `Diva-Workshops` 的 `3-Analysis` 模块
3. **理解 IAP 特有方法** → 精读 Cheng & Zhu (2016) 了解 EnOI-DE 中 CMIP 动态集合的构建方式
4. **构建你自己的方案** → 以 `DIVAnd.jl` 或 `ZC-BOM/optimal-interpolation` 为基础，将背景场替换为 IAP 气候态/格点数据，观测替换为 Argo/CTD/XBT 等实测剖面数据
5. **参考海洋 EnOI** → 参考 `Liu-Jincan` 的 MATLAB EnOI 代码了解海洋同化特有的处理（如协方差局地化、深度相关结构）

### IAP 方法的关键差异

注意：传统 OI 使用**基于距离的统计协方差模型**（如高斯函数），而 IAP 的 EnOI-DE 使用 **CMIP5/CMIP6 多模式历史模拟**构建动态集合来提供背景误差协方差。这意味着：
- 如果你想严格复现 IAP 方法，需要额外获取 CMIP 多模式输出
- 如果你想做一个简化版（用传统 OI + IAP 气候态背景场），可直接使用上面列出的工具
- EnOI 的核心优势在于协方差能反映海洋动力学关系（如不同深度、不同海盆之间的遥相关），这是纯统计 OI 无法做到的

---

> **备注**: 本合集将持续更新。如有新的相关仓库发现，欢迎补充。
