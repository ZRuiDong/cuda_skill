---
name: CUDA纹理存储器绑定与访问
description: 提供纹理存储器的完整使用流程，包括绑定线性内存或CUDA数组、设备端访问及解绑操作。当需要对只读数据利用纹理缓存的硬件优化特性（如滤波、归一化坐标）时使用此技能。
---

# CUDA纹理存储器绑定与访问

## 何时使用
当满足以下条件时应用本技能：
- 需要对只读数据使用纹理存储器
- 已分配设备内存（线性内存或CUDA数组）
- 纹理参考已声明为全局变量

## 执行步骤
1. **主机端纹理建立**：
   a) 分配存储空间：
      - 线性内存：`cudaMalloc()`
      - CUDA数组：`cudaMallocArray()` + `cudaChannelFormatDesc`
   b) 声明纹理参考：`texture<Type, Dim, ReadMode> texRef;`
   c) 绑定数据：
      - 线性内存：`cudaBindTexture(offset, texRef, devPtr, size)`
      - CUDA数组：`cudaBindTextureToArray(texRef, cuArray)`

2. **设备端纹理访问**：
   a) 线性内存绑定：使用 `tex1Dfetch(texRef, int x)`
   b) CUDA数组绑定：
      - 1D：`tex1D(texRef, float x)`
      - 2D：`tex2D(texRef, float x, float y)`
      - 3D：`tex3D(texRef, float x, float y, float z)`

3. **清理资源**：
   - 调用 `cudaUnbindTexture(texRef)` 解除绑定

> 注意：线性内存仅支持1D、整型坐标、无滤波；CUDA数组支持多维、浮点坐标、滤波及归一化坐标。