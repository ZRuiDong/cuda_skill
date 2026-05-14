---
name: CUDA二维显存分配与间距对齐机制
description: 在GPU上分配用于二维数据结构（如图像、矩阵）的显存时，若需执行高效的跨行访问或2D内存复制操作，则使用cudaMallocPitch进行带对齐填充的显存分配。该技能适用于需要优化内存访问性能的CUDA二维数组场景，不适用于一维简单缓冲区。
---

# CUDA二维显存分配与间距对齐机制

## 何时使用
当满足以下条件时，应使用本技能：
- 需要在GPU上分配二维数据结构（如图像数据、矩阵存储、二维数组）
- 计划执行2D内存复制（如cudaMemcpy2D）或频繁进行跨行内存访问
- 要求内存访问性能优化，利用硬件对齐提升吞吐量

**注意**：若无需对齐或仅使用一维线性缓冲区，请直接使用`cudaMalloc`。

## 执行步骤
1. 调用 `cudaMallocPitch(void **devPtr, size_t *pitch, size_t widthInBytes, size_t height)`
2. 确保输入参数 `widthInBytes` 和 `height` 为正整数
3. 系统将分配至少 `widthInBytes * height` 字节的线性显存，并自动在每行末尾添加填充字节以满足硬件对齐要求
4. 通过 `*pitch` 获取实际每行占用的字节数（含填充），该值可能大于 `widthInBytes`
5. 通过 `*devPtr` 获取分配的设备内存基地址
6. 使用公式 `(T*)((char*)BaseAddress + Row * pitch) + Column` 计算二维数组中任意元素的地址
7. 检查返回值：成功时为 `cudaSuccess`，失败（如内存不足）时为 `cudaErrorMemoryAllocation`

## 关键要点
- `pitch` 是实际行宽（字节），包含对齐填充，必须用于地址计算
- 所有涉及2D内存复制的操作都推荐使用此分配方式
- 不要假设 `pitch == widthInBytes`，这通常不成立

> 详细地址计算示例、错误处理代码模板及性能影响分析请参见参考文档。