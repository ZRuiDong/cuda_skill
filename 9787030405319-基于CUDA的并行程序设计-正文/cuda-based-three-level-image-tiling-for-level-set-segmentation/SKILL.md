---
name: CUDA-Based Three-Level Image Tiling for Level Set Segmentation
description: 将大尺寸遥感图像或水平集分割任务按三级分块策略划分，以适配CPU-GPU协同处理架构。当处理大尺寸图像且系统具备CUDA支持时使用此技能，确保显存高效利用并避免内存溢出。
---

# CUDA-Based Three-Level Image Tiling for Level Set Segmentation

## 何时使用

- 输入为大尺寸遥感图像或需执行水平集分割的图像数据
- 系统具备CUDA支持设备
- 图像尺寸足够大（小图像无法体现分块优势）
- 需在有限GPU显存下完成图像处理任务

## 执行步骤

1. **确认前提条件**：获取图像宽高（`lWidth`, `lHeight`）、预设第3级分块大小（`PieceSize`）、GPU全局显存（`DeviceMemorySize`）及共享内存大小（`SharedMemorySize`）。

2. **计算第3级分块参数**：
   - 计算横向与纵向分块数：`ImgWp = lWidth / PieceSize + 1`，`ImgHp = lHeight / PieceSize + 1`
   - 计算实际每块宽高（考虑余数）：`ImgPiecewidth = lWidth / ImgWp + 1`，`ImgPieceheight = lHeight / ImgHp + 1`

3. **估算GPU单次可处理的最大第3级分块数**：
   - 使用公式：`ImgBlockNum_Max = (int)(DeviceMemorySize * MemoryUseRate) / (ImgPiecewidth * ImgPieceheight * 11)`
   - 其中 `11` 为每个像素所需字节数的保守估计（含多个中间变量）

4. **构建第1级（CPU级）分块网格**：
   - 设定网格行列数：`ImgRow = ImgCol = (int)sqrt((float)ImgBlockNum_Max)`
   - 计算每块实际覆盖尺寸：`ImgWidth = ImgCol * ImgPiecewidth`，`ImgHeight = ImgRow * ImgPieceheight`

5. **确定第1级分块总数**：
   - `ImgBlockCol = (lWidth + ImgWidth - 1) / ImgWidth`
   - `ImgBlockRow = (lHeight + ImgHeight - 1) / ImgHeight`

6. **遍历每个第1级分块**：
   - 计算行/列偏移量（`offset_h`, `offset_w`），处理边界余数
   - 确定当前块实际宽高（最后一块需特殊处理）
   - 计算该块内包含的第3级分块数量（`wp`, `hp`）
   - 构建精确分块尺寸数组 `psize`，使用模运算分配每块宽度和高度

7. **调度至GPU**：将主机数据拷贝到设备，并启动GPU核函数执行水平集分割。

> 注意：若图像尺寸过小或系统无CUDA支持，不应使用本技能。