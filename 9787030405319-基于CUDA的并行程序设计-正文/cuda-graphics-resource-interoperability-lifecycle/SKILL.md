---
name: CUDA图形资源互操作生命周期管理
description: 管理OpenGL或Direct3D图形资源在CUDA中的注册、映射、访问与注销完整流程。当应用程序需要在CUDA内核中直接读写图形API（如OpenGL缓冲区/纹理或Direct3D资源）时使用本技能，确保资源同步、避免未定义行为并遵循设备兼容性约束。
---

# CUDA图形资源互操作生命周期管理

## 何时使用
- 需要在CUDA内核中访问由OpenGL或Direct3D创建的缓冲区、纹理或渲染表面
- 图形与计算管线需共享同一块GPU内存
- 应用程序已初始化CUDA上下文并确认设备支持图形互操作

## 执行步骤

1. **注册资源**：调用对应API函数注册图形资源，获取`cudaGraphicsResource_t`句柄
   - OpenGL缓冲区：`cudaGraphicsGLRegisterBuffer`
   - OpenGL纹理/渲染缓存：`cudaGraphicsGLRegisterImage`
   - Direct3D资源：`cudaGraphicsD3D10RegisterResource`等

2. **（可选）设置访问标志**：调用`cudaGraphicsResourceSetMapFlags()`指定访问模式（ReadOnly / WriteDiscard / None）以优化驱动行为

3. **映射资源**：调用`cudaGraphicsMapResources()`将资源映射到CUDA地址空间，该操作隐式同步图形API命令队列

4. **获取设备指针或数组**：
   - 缓冲区：使用`cudaGraphicsResourceGetMappedPointer()`获取`void*`设备指针
   - 纹理/子资源：使用`cudaGraphicsSubResourceGetMappedArray()`或`cudaGraphicsResourceGetMappedMipmappedArray()`获取`cudaArray_t`

5. **在CUDA内核中访问数据**：通过返回的指针或数组进行读写

6. **解映射资源**：调用`cudaGraphicsUnmapResources()`解除映射，该操作隐式同步CUDA内核执行

7. **注销资源**：在资源不再需要互操作时调用`cudaGraphicsUnregisterResource()`释放关联

## 关键约束
- 禁止重复注册同一资源
- 映射状态下不得注销资源
- 映射期间图形API不得访问同一资源
- 注册是高开销操作，应避免每帧执行
- 不支持跨GPU设备互操作