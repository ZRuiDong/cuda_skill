---
name: CUDA灰度图像对比度增强（imadjust加速）
description: 使用CUDA并行实现MATLAB imadjust函数的灰度对比度增强功能，适用于256级灰度图像且已预计算LUT映射表的场景。当需要在GPU上高效处理大批量uint8灰度图像以提升对比度时使用此技能。
---

# CUDA灰度图像对比度增强（imadjust加速）

## 适用条件
- 输入为256色（uint8）灰度图像
- 已根据目标对比度参数（low_in, high_in, low_out, high_out）计算好256元素的查找表（LUT）
- GPU支持共享内存
- 不适用于彩色图像或需gamma非线性变换的场景

## 执行步骤
1. **准备LUT**：确保已按公式构建256元素的LUT数组，用于像素值映射。
2. **启动CUDA内核**：
   - 每个block配置256个线程（block_size=256）
   - 每个线程将全局内存中的LUT[threadIdx.x]复制到共享内存 `_ucLut2[threadIdx.x]`
   - 调用 `__syncthreads()` 同步所有线程
3. **并行图像处理**：
   - 每个线程计算全局像素索引 `i = threadIdx.x + blockIdx.x * blockDim.x`
   - 循环步进 `i += blockDim.x * gridDim.x` 直至覆盖整图
   - 对每个像素 `d_ucIn[i]`，执行查表赋值：`d_ucIn[i] = _ucLut2[d_ucIn[i]]`
4. **MEX接口调用**：
   - 接收单个uint8输入矩阵
   - 创建同尺寸输出矩阵
   - 调用上述CUDA内核完成增强

## 输出结果
返回与MATLAB `imadjust` 函数结果一致的uint8灰度图像矩阵，已完成对比度增强。

> **提示**：若需处理彩色图像，请先转换为灰度；若需gamma校正，请在本流程前后单独处理。