---
name: SIFT特征点主方向计算
description: 为已提取的SIFT特征点计算具有旋转不变性的主方向，使用36-bin梯度方向直方图结合高斯加权和平滑处理。当需要为SIFT特征点分配主方向以实现旋转不变性描述子构建时使用本技能。
---

# SIFT特征点主方向计算

## 适用对象
- 已提取的SIFT特征点（包含位置(x, y)和尺度sigma）

## 使用前提
- 特征点位置和尺度已知
- 提供 gaussian_factor 和 sample_factor 参数
- 图像区域具有明确定义的梯度
- 特征点邻域窗口边界在 [1.5, width - 1.5] 范围内

## 执行流程

1. **初始化参数**：
   - 计算邻域半径：`win = |key.z| * sample_factor`
   - 设置距离阈值：`dist_threshold = win² + 0.5`
   - 计算高斯权重因子：`factor = -0.5 / (sigma * gsigma)`，其中 `gsigma = key.z * gaussian_factor`

2. **遍历邻域采样点**（x从xmin到xmax，y从ymin到ymax）：
   - 跳过超出距离阈值的点（`sq_dist ≥ dist_threshold`）
   - 获取该点的梯度幅值和方向
   - 应用高斯权重：`weight = gradient_magnitude * exp(sq_dist * factor)`
   - 将方向映射到0–35的bin索引：`oidx = floor(direction_degrees) mod 36`
   - 累加权重到对应bin：`vote[oidx] += weight`

3. **对直方图进行三次平滑**（共6次迭代，每次使用1/3权重）：
   - 对每个bin i：`vote[i] = (vote[i-1] + vote[i] + vote[i+1]) / 3`
   - 使用循环边界：`vote[36] = vote[0]`

4. **查找并精化主方向峰值**：
   - 找出最大响应bin `index_max`
   - 使用二次插值计算偏移量：`off = 0.5 * (next - pre) / (2*max_vote - next - pre)`
   - 输出最终主方向（弧度）：`(index_max + 0.5 + off) / 5.729577951308232`

5. **结果存储**：将计算出的主方向（弧度）存入特征点的w分量。

> 注意：此技能通常在SIFT特征描述子生成前调用，确保后续描述子具备旋转不变性。