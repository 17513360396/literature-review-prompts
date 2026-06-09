# R 语言空间插值生态 — gstat & automap

- **gstat GitHub**: https://github.com/r-spatial/gstat
- **automap GitHub**: https://github.com/cran/automap (CRAN 镜像)
- **语言**: R
- **作者**: gstat — Edzer Pebesma; automap — Paul Hiemstra
- **适用场景**: R 环境下的空间统计与 Kriging

## 概述

R 在地统计学（geostatistics）领域拥有最成熟的生态系统。`gstat` 是该生态的核心包——它实现了 Kriging 的几乎全部变体（普通 Kriging、通用 Kriging、Co-Kriging、时空 Kriging），而 `automap` 在其基础上提供了全自动化的插值流程。这些工具对理解 OI 的 Variogram/协方差建模部分特别有价值。

## gstat 核心功能

### 单变量 Kriging
```r
library(gstat)
library(sp)

# 1. 计算实验 Variogram
vario <- variogram(temperature ~ 1, data = obs_points)

# 2. 拟合理论模型
vario_fit <- fit.variogram(vario, model = vgm("Sph"))

# 3. 执行 Kriging
krige_result <- krige(temperature ~ 1,
                      locations = obs_points,
                      newdata = grid_points,
                      model = vario_fit)
# 输出：predictions + kriging variance
```

### Co-Kriging（多变量联合插值）
```r
# 同时使用温度和盐度的协方差结构
# 这在海洋学中特别有用——T 和 S 有已知的物理协变关系
g <- gstat(NULL, "temp", temperature ~ 1, obs_temp)
g <- gstat(g, "salt", salinity ~ 1, obs_salt)
g <- gstat(g, "temp_salt", temperature ~ salinity, obs_combined)

co_krige <- predict(g, grid_points)
```

### 时空 Kriging (krigeST)
```r
# 3D：lon × lat × time
library(spacetime)
st_vario <- variogramST(temperature ~ 1, data = st_obs)
st_fit <- fit.StVariogram(st_vario, model = vgmST("metric",
                           space = vgm("Sph"),
                           time = vgm("Sph")))
```

## automap 全自动流程

```r
library(automap)

# 一行代码完成 OI/Kriging！
result <- autoKrige(temperature ~ 1,
                    input_data = obs_points,
                    new_data = grid_points)
# autoKrige 自动完成：
#   1. 实验 variogram 计算
#   2. 理论模型选择 + 拟合
#   3. Kriging 插值
#   4. 交叉验证
```

这就是"一键 OI"——`autoKrige` 自动完成了所有需要专家知识的参数选择。

## R 与 Python OI 生态的对比

| 能力 | R (gstat/automap) | Python (ZC-BOM + pyinterpolate) |
|:---|:---|:---|
| Co-Kriging（多变量联合） | ✅ 原生支持 | ❌ 尚未实现 |
| 时空 Kriging | ✅ 支持 | ❌ 尚未实现 |
| Variogram 自动拟合 | ✅ automap | ✅ pyinterpolate |
| "背景场"概念 | ❌ 不是地统计学的核心概念 | ✅ OI 中有 B 矩阵 |
| 数据同化（DA）集成 | ❌ 不涉及 | ✅ 多种方法 |
| 教学资源丰富度 | ✅ Tutorial 极多 (rspatial.org 等) | 中等 |

## 为什么 OI 学习者应该了解 R 生态

R 的地统计工具箱对 OI 学习者的核心价值在于 **Variogram 分析**：

1. **可视化协方差结构**：在 R 中绘制实验 semi-variogram 是理解"空间相关性如何随距离衰减"的最佳方式
2. **多种理论模型对比**：gstat 支持 spherical, exponential, gaussian, Matern, Bessel 等模型——在 OI 中你要为 B 矩阵选择协方差模型，这些模型在 R 中可以先测试拟合效果
3. **Co-Kriging** 对应于 OI 中的多变量同化（如 T+S 联合），R 的实现最容易理解
4. **Edzer Pebesma 的教程** (rspatial.org) 是理解空间统计的经典参考

## 快速开始

```r
# 安装
install.packages(c("gstat", "automap", "sp"))
library(gstat)
library(automap)

# 加载示例数据
data(meuse)  # gstat 自带的重金属污染数据

# 计算 variogram
v <- variogram(zinc ~ 1, meuse)
plot(v)

# 自动 Kriging
k <- autoKrige(zinc ~ 1, meuse, meuse.grid)
plot(k)

# 看看自动选择了什么模型
summary(k$var_model)
```

## 推荐学习资源

- **rspatial.org** (Edzer Pebesma 的线上教材) — 空间统计经典
- **Geocomputation with R** (Robin Lovelace 等) — 第12-13章
- **Applied Spatial Data Analysis with R** (Bivand, Pebesma, Gomez-Rubio) — 学术标准参考
