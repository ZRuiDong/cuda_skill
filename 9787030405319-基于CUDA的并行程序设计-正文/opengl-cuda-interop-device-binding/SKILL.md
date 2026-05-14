---
name: OpenGL-CUDA互操作设备绑定
description: 在需要将OpenGL上下文与CUDA设备进行互操作时，执行设备查询、绑定及资源注册的标准化流程。当应用程序需在GPU上共享OpenGL图形资源（如缓冲区或纹理）与CUDA计算时使用本技能。
---

# OpenGL-CUDA互操作设备绑定

## 使用时机
当你的应用程序需要在同一个NVIDIA GPU上同时使用OpenGL渲染和CUDA计算，并希望二者共享显存资源（例如将渲染结果直接作为CUDA核函数输入）时，必须在任何CUDA运行时调用前完成设备绑定。

## 执行步骤

1. **设备查询**：调用 `cudaGLGetDevices()` 获取与当前OpenGL上下文兼容的CUDA设备列表。
   - 指定 `deviceList` 参数以选择设备范围：
     - `cudaGLDeviceListAll`（值1）：所有兼容设备
     - `cudaGLDeviceListCurrentFrame`（值2）：当前帧使用的设备
     - `cudaGLDeviceListNextFrame`（值3）：下一帧将使用的设备

2. **设备绑定**：在首次调用任何CUDA运行时API之前，必须调用 `cudaGLSetGLDevice(deviceId)` 明确指定要用于互操作的CUDA设备。

3. **遵守互斥约束**：禁止同时使用 `cudaSetDevice()` 和 `cudaGLSetGLDevice()`，二者功能冲突，会导致未定义行为。

4. **注册OpenGL资源**：
   - 对于顶点/像素缓冲区，调用 `cudaGraphicsGLRegisterBuffer()`，在CUDA中以设备指针形式访问。
   - 对于纹理或渲染缓存，调用 `cudaGraphicsGLRegisterImage()`，在CUDA中以CUDA数组形式访问。

5. **验证格式支持**：确保所用OpenGL内部格式被CUDA支持，包括：
   - 所有浮点格式（如 `GL_RGBA_FLOAT32`）
   - 非归一化整数格式（如 `GL_RGBA8UI`，需OpenGL 3.0+）

> ⚠️ 注意：`GL_RGBA8UI` 等整数格式仅能由着色器写入，固定功能管线无法写入，使用前请确认渲染路径兼容性。