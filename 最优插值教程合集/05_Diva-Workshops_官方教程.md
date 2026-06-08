# Diva-Workshops — DIVA/DIVAnd 官方培训教程

- **GitHub**: https://github.com/gher-uliege/Diva-Workshops
- **语言**: Jupyter Notebook (Julia)
- **作者**: 列日大学 GHER 组
- **适用场景**: 从零开始学习海洋数据格点化的完整流程

## 概述

这是 **DIVA/DIVAnd 的官方培训工作坊**，也是目前 GitHub 上**最完整的海洋数据格点化教程**。从 Julia 基础、数据预处理、参数优化到高级物理约束，4 个递进模块覆盖了用变分方法（等价于 OI）进行海洋数据格点化的全过程。

## 模块结构

### 1-Intro：入门基础
```
1-02-julia-basics.ipynb      → Julia 语言入门
1-03-netcdf.ipynb            → netCDF 文件读写
1-04-plotting.ipynb          → 数据可视化
```
**目标**：环境搭建，基本数据操作能力

### 2-Preprocessing：数据预处理
```
2-01-data-reading.ipynb      → 读取各种格式的海洋观测数据
2-02-bathymetry.ipynb        → 加载和处理水深地形数据
2-03-masks.ipynb             → 创建陆海掩码
```
**目标**：准备分析所需的网格、掩码和观测数据

### 3-Analysis：核心分析 ⭐ 重点
```
3-01-simple-analysis.ipynb   → 最简单的分析示例
3-02-parameter-optimization.ipynb → 参数（len, epsilon2）优化
3-03-error-fields.ipynb      → 生成分析误差场
3-04-masked-analysis.ipynb   → 带陆海掩码的分析
3-05-bathymetry-constraint.ipynb → 海底地形约束
3-07-example-analysis.ipynb  → 🔥 完整分析流程（强烈推荐）
3-09-full-analysis.ipynb     → 🔥 可作为你项目的模板
```
**目标**：从观测到格点化产品的完整分析流程

### 4-AdvancedTopics：高级主题
```
4-01-advection.ipynb         → 平流约束（适合锋面、洋流区域）
4-02-multivariate.ipynb      → 多变量分析（如 T、S 联合同化）
4-03-inequalities.ipynb      → 不等式约束（如非负约束）
4-04-domain-decomposition.ipynb → 大域分解（大规模数据处理）
```
**目标**：高级应用场景

## 快速开始

```bash
# 克隆仓库
git clone https://github.com/gher-uliege/Diva-Workshops.git
cd Diva-Workshops

# 启动 Jupyter
jupyter notebook
```

## 推荐学习路径

对于你的场景（IAP 背景场 + 多源实测站点温盐数据）：

1. **快速浏览** 1-Introduction（如不熟悉 Julia）
2. **重点完成** 3-07 和 3-09 这两个完整的分析 Notebook
3. **根据需要**学习 4-02（多变量分析，用于联合 T+S 同化）
4. **可选**学习 2-Preprocessing（了解数据准备流程）

## 与你场景的对应关系

| Workshop 概念 | 你的场景 |
|:---|:---|
| Background field | IAP 月平均温盐格点数据 |
| Observations | Argo/CTD/XBT 等实测站点剖面 |
| Mask | 研究区域陆海掩码 |
| `len`（相关长度） | 海洋中 T/S 的去相关尺度 (~200-500 km) |
| `epsilon2`（信噪比倒数） | IAP 不确定性 vs 观测误差的比值 |
| Analysis field | 融合后的温盐格点产品 |

## 重点 Notebook 详解

### 3-07-example-analysis.ipynb
完整分析示例，包含：
1. 读取观测数据
2. 设置网格和掩码
3. 选择相关长度和信噪比
4. 执行 DIVAnd 分析
5. 可视化分析场和误差场
6. 输出 NetCDF 文件

### 3-09-full-analysis.ipynb
可作为项目模板，比 3-07 更完整，包含：
- 多参数配置对比
- 交叉验证
- 批量输出
