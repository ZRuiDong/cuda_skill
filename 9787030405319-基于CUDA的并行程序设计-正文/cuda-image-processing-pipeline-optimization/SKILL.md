---
name: CUDA图像处理流水线优化
description: 该技能用于在GPU上优化连续的图像处理操作（如imadjust和imerode），通过合并多个CUDA内核调用为单一内核，避免中间结果在显存与主机内存之间反复拷贝。当用户处理灰度图像、已实现CUDA版本的imadjust和imerode函数、且流程中存在连续无数据依赖中断的操作时，应使用此技能。
---

# CUDA图像处理流水线优化

## 何时使用

- 当前图像处理流程包含连续的`imadjust`（对比度调整）和`imerode`（形态学腐蚀）操作；
- 两个操作均已实现CUDA版本；
- 输入为灰度图像数据；
- 运行环境为Windows平台并可使用`QueryPerformanceCounter`进行计时；
- 所有操作在同一GPU设备上执行，且中间结果无需返回CPU。

> **注意**：本技能不适用于纯CPU环境，或处理步骤间存在必须返回主机内存的数据依赖。

## 优化核心原则

保持图像数据全程驻留在GPU显存中，避免频繁分配/释放显存及多次主机-设备间数据传输。

## 执行步骤

1. **分析原始流程瓶颈**：确认显存分配（而非内核计算）是主要耗时来源。
2. **设计合并内核函数**：创建单一CUDA函数（如`imadjust_imerode`），依次执行对比度调整与腐蚀。
3. **一次性分配GPU资源**：
   - 使用`cudaMallocPitch`分配输入缓冲区；
   - 使用`cudaMalloc`分配输出缓冲区和LUT映射表。
4. **单次拷贝输入数据**：通过`cudaMemcpy2D`将主机图像数据一次性传入GPU显存。
5. **执行imadjust阶段**：
   - 计算`stretchlim`获取`fLowIn`/`fHighIn`；
   - 生成256级查找表（ucLut）；
   - 启动`adjust2`核函数完成直方图拉伸。
6. **配置纹理引用**：
   - 设置`normalized=0`；
   - `filterMode=CUDA_FILTER_MODE_POINT`；
   - `addressMode=[cudaAddressModeWrap, cudaAddressModeWrap]`。
7. **绑定纹理内存**：使用`cudaBindTexture2D`供后续腐蚀操作高效读取。
8. **执行imerode阶段**：启动`erode`核函数直接处理显存中的调整后图像。
9. **单次回传结果**：仅一次调用`cudaMemcpy`（设备→主机）返回最终图像。
10. **统一释放资源**：调用`cudaFree`释放所有GPU内存。

## 预期效果

- 显存分配与数据拷贝次数显著减少；
- 实测处理速度提升约7倍（例如从1.12秒降至0.02秒）；
- 整体吞吐量大幅提升，尤其适用于显存受限的预处理场景。

> 关键提示：“显存分配非常耗时”、“不用把图片内容从显存复制回内存”、“合并imadjust和imerode两个处理过程”。