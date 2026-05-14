---
name: subpixel-extremum-localization
description: 对通过初步极值检测的候选点执行亚像素级精确定位，适用于SIFT等特征提取算法中需要高精度关键点定位的场景。当subpixel_localization标志为真且候选点已通过初步极值检测时使用。
---

# 亚像素级极值点精确定位

## 何时使用
- 在SIFT或其他基于DoG（Difference of Gaussians）的关键点检测流程中
- 已完成初步极值点筛选
- 配置参数`subpixel_localization`为`true`
- 需要获得亚像素精度的关键点位置和类型

## 执行步骤

1. **验证前提条件**：
   - 确保已计算一阶偏导（fx, fy, fs）和二阶偏导（fxx, fyy, fss, fxy, fxs, fys）
   - 确认输入为通过初步极值检测的候选点

2. **构建Hessian矩阵并求解偏移量**：
   - 构造线性方程组：`[-fx, -fy, -fs]^T = Hessian × [dx, dy, ds]^T`
   - 使用带行交换的部分主元高斯消元法求解(dx, dy, ds)，避免除零（阈值设为1e-10）

3. **验证插值结果有效性**：
   - 计算插值后DoG值：`v + 0.5*(dx*fx + dy*fy + ds*fs)`
   - 检查是否满足：
     - `|插值后DoG值| > dog_threshold`
     - `|dx| < 1.0`、`|dy| < 1.0`、`|ds| < 1.0`

4. **输出结果**：
   - 若验证通过，输出四元组`(result, dx, dy, ds)`
   - 其中`result = 1.0`表示极大值，`result = -1.0`表示极小值
   - 若验证失败，丢弃该候选点

> 注意：所有偏导数计算均基于3×3×3邻域内的DoG数据，具体公式详见参考文档。