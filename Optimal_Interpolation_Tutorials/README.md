# Optimal Interpolation（最优插值）教程合集 — GitHub 资源索引

> 整理时间：2026-06-09  
> 涵盖数据同化中的最优插值（OI）、集合最优插值（EnOI）及相关空间插值方法的 GitHub 教程与代码

## 什么是 Optimal Interpolation

最优插值（OI）是一种最小方差线性无偏估计（Best Linear Unbiased Estimation, BLUE），广泛应用于气象学、海洋学、大气化学等地球科学领域的数据同化。其核心公式：

```
X_a = X_b + K × (Y - H × X_b)
K = B × H^T × (H × B × H^T + R)^{-1}
```

其中 X_b 为背景场，Y 为观测，B 为背景误差协方差，R 为观测误差协方差，H 为观测算子。

---

## 文件索引

### 🔴 OI 数据同化核心教程

| 文件 | 说明 | 语言 | Stars |
|:---|:---|:---|:---|
| [01_DAPPER_数据同化框架.md](01_DAPPER_数据同化框架.md) | 挪威 Nansen 中心 DA 研究与教学框架 | Python | ~370 |
| [02_DA-tutorials_Nansen教程.md](02_DA-tutorials_Nansen教程.md) | OI→EnKF 渐进式 Jupyter 教程 | Python/Jupyter | ~155 |
| [03_ZC-BOM_最优插值.md](03_ZC-BOM_最优插值.md) | 澳大利亚气象局 OI 纯 Python 实现 | Python | — |
| [04_UMD_数据同化教程.md](04_UMD_数据同化教程.md) | RISDA 2018 国际数据同化学校教程 | Python/Jupyter | — |
| [05_Leo-Rain_EnOI教学.md](05_Leo-Rain_EnOI教学.md) | EnOI MATLAB 教学实现 + Ridge 回归 | MATLAB | — |
| [06_EnKF-C_业务化OI.md](06_EnKF-C_业务化OI.md) | 业务级 EnOI C 语言实现（澳大利亚气象局） | C | ~35 |

### 🟡 大气反演与地球科学中的 OI

| 文件 | 说明 | 语言 | Stars |
|:---|:---|:---|:---|
| [07_atmos_flux_inversion.md](07_atmos_flux_inversion.md) | Penn State 大气通量反演 OI 求解器 | Python | — |
| [08_GPSat_高斯过程插值.md](08_GPSat_高斯过程插值.md) | UCL 卫星测高数据的可扩展 GP 插值 | Python | — |

### 🟢 通用空间插值（Kriging 等方法）

| 文件 | 说明 | 语言 | Stars |
|:---|:---|:---|:---|
| [09_pyinterpolate_Kriging.md](09_pyinterpolate_Kriging.md) | 地统计 Kriging Python 库（JOSS 论文） | Python | ~92 |
| [10_R_gstat_automap.md](10_R_gstat_automap.md) | R 语言 geostatistics 生态（gstat/automap） | R | — |

### 📚 总结对比

| 文件 | 说明 |
|:---|:---|
| [11_方法对比与学习路径.md](11_方法对比与学习路径.md) | 各方法/工具对比表 + 推荐学习路径 |
