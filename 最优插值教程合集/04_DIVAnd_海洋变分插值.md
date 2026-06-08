# DIVAnd.jl — n 维海洋数据变分分析

- **GitHub**: https://github.com/gher-uliege/DIVAnd.jl
- **语言**: Julia
- **Stars**: ~79 | **License**: GPL-2.0
- **作者**: 列日大学 GHER（GeoHydrodynamics and Environment Research）组
- **适用场景**: 海洋/大气观测数据的任意维度格点化

## 概述

**DIVAnd 是目前海洋学界最成熟的观测格点化工具**。它在数学上等价于最优插值（OI）和 Kriging，但通过有限元方法实现了更强的适应能力和更好的边界处理。被 EMODnet、SeaDataCloud、BlueCloud 等欧洲海洋数据基础设施用于**业务化**生产格点产品。

## 与最优插值（OI）的关系

OI、Kriging、变分分析在数学上是等价的，最优解形式相同：

```
X_a = X_b + (H^T R^{-1} H + B^{-1})^{-1} H^T R^{-1} (Y - H X_b)
```

| 方法 | 数学形式 | 实现方式 |
|:---|:---|:---|
| OI | 矩阵显式求逆 | Kalman 增益矩阵 |
| Kriging | 统计插值 | Variogram 拟合 |
| DIVAnd | 变分优化 | 有限元 PDE 求解 |

DIVAnd 的优势在于通过有限元离散，天然处理复杂几何边界。

## 海洋特色功能

### 1. 海岸线/地形约束
```julia
# 加载陆海掩码
mask = DIVAnd.load_mask("mask.nc")

# 分析自动尊重陆海边界
# 信息不会从太平洋"穿透"巴拿马地峡传播到大西洋
fi, s = DIVAndrun(mask, (pm, pn), (lon, lat), (obslon, obslat), obsval,
                  len, epsilon2)
```

### 2. n 维分析（含深度和时间）
```julia
# 4D 分析：lon × lat × depth × time
fi, s = DIVAndrun(mask, (pm, pn, pdepth, ptime),
                  (lon, lat, depth, time),
                  (obslon, obslat, obsdepth, obstime),
                  obsval,
                  (len_lon, len_lat, len_depth, len_time),
                  epsilon2)
```

### 3. 物理约束
```julia
# 平流约束（适合海洋锋面区域）
vel_u, vel_v = load_velocity_field()
fi = DIVAndrun(..., advection_velocity=(vel_u, vel_v), ...)
```

### 4. 误差估计
```julia
# 分析误差方差
error_variance = DIVAnd_aexerr(fi, s, ...)
```

## 核心函数

| 函数 | 用途 |
|:---|:---|
| `DIVAndrun` | 核心 n 维分析 |
| `DIVAndgo` | 大规模问题的自动分域+迭代求解 |
| `diva3D` | 3D 地球气候态高级接口 |
| `DIVAnd_cv` | 交叉验证参数优化 |
| `DIVAnd_aexerr` | "近似精确"误差计算 |
| `DIVAndfit` | 自动拟合信号/噪声比和相关长度 |

## 关键参数调优

### 相关长度 `len`
- 物理含义：信息传播的特征距离
- 调优方法：可以通过 `DIVAnd_cv`（交叉验证）或 `DIVAndfit`（自动拟合）
- 海洋建议值：
  - 中纬度 SST：100-300 km
  - 次表层温度：200-500 km
  - 盐度：与温度类似但通常略小

### 信噪比倒数 `epsilon2`
- `epsilon2 = σ_obs² / σ_background²`
- 值大 → 更信任背景场（平滑）
- 值小 → 更信任观测（贴近观测）
- 建议：可以从 1.0 开始，用交叉验证优化

## 安装

```julia
using Pkg
Pkg.add("DIVAnd")
```

## 适配海洋温盐融合

```julia
using DIVAnd
using NCDatasets

# 1. 加载 IAP 背景场
iap_temp = ncread("IAPv4_temperature.nc", "temperature")
lon_bg = ncread("IAPv4_temperature.nc", "lon")
lat_bg = ncread("IAPv4_temperature.nc", "lat")

# 2. 加载观测（Argo/CTD/XBT 剖面已垂向插值到标准层）
obs_lon = [120.5, 121.3, ...]  # 观测经度
obs_lat = [20.3, 21.1, ...]   # 观测纬度
obs_val = [25.6, 24.8, ...]   # 观测温度值

# 3. 设置参数
len = 300e3  # 300 km 相关长度（需转为度）
epsilon2 = 1.0

# 4. 执行分析（逐深度层或 3D 联合）
fi, s = DIVAndrun(mask, (pm, pn), (lon_bg, lat_bg),
                  (obs_lon, obs_lat), obs_val, len, epsilon2)

# 5. 输出分析场
# fi 即为融合 IAP 背景场和观测的分析场
```

## 引用

> Barth, A., Beckers, J.-M., Troupin, C., Alvera-Azcárate, A., and Vandenbulcke, L.: DIVAnd-1.0: n-dimensional variational data analysis for ocean observations, Geosci. Model Dev., 7, 225–241, 2014. doi:10.5194/gmd-7-225-2014
