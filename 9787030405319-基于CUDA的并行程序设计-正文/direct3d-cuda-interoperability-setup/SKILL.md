---
name: Direct3D-CUDA互操作设备与资源约束配置
description: 配置Direct3D（9/10/11）与CUDA的互操作环境，包括设备绑定、资源注册及格式合规性验证。当应用程序需要在同一个GPU上共享Direct3D渲染资源与CUDA计算时使用本技能。
---

# Direct3D-CUDA互操作设备与资源约束配置

## 何时使用
当你的应用程序需要在Direct3D（版本9、10或11）和CUDA之间高效共享图形资源（如纹理或缓冲区）进行混合渲染与计算时，必须使用本技能确保设备绑定正确、资源格式合规，并避免注册不支持的资源类型。

## 执行步骤

### 1. 设备绑定
在首次调用任何CUDA运行时API之前，必须将Direct3D设备与CUDA上下文绑定：
- **Direct3D9**：调用 `cudaD3D9SetDirect3DDevice(pD3DDevice)`
- **Direct3D10**：调用 `cudaD3D10SetDirect3DDevice(pD3DDevice)`
- **Direct3D11**：调用 `cudaD3D11SetDirect3DDevice(pD3DDevice)`

> ⚠️ 必须确保Direct3D设备使用 `D3DCREATE_HARDWARE_VERTEXPROCESSING` 创建，且GPU支持CUDA。

### 2. 查询关联CUDA设备
使用以下函数获取与Direct3D设备关联的CUDA设备列表：
```cpp
size_t deviceCount;
cudaD3D10GetDevices(&deviceCount, nullptr, pD3DDevice);
```
`deviceList` 参数可指定为 `cudaD3D10DeviceListAll`, `cudaD3D10DeviceListCurrentFrame`, 或 `cudaD3D10DeviceListNextFrame`。

### 3. 注册Direct3D资源
对每个需在CUDA中访问的Direct3D资源执行注册：
```cpp
cudaGraphicsResource_t resource;
cudaGraphicsD3D10RegisterResource(&resource, pD3DResource, flags);
```
- 支持的资源类型：
  - `ID3D10Buffer` → 映射为CUDA设备指针
  - `ID3D10Texture1D/2D/3D` → 映射为CUDA数组

### 4. 验证资源格式限制
仅允许以下DXGI格式（以D3D10/11为例）：
- 单/双/四通道的8/16/32位整数或浮点格式
- 具体包括：
  - `DXGI_FORMAT_A8_UNORM`
  - `DXGI_FORMAT_B8G8R8A8_UNORM` / `X8_UNORM`
  - `R16/R32` 系列的 `FLOAT` / `SINT` / `UINT` / `UNORM` / `SNORM`
  - `R8G8B8A8` / `R8G8` / `R8` 系列的 `SINT` / `SNORM` / `UINT` / `UNORM`

### 5. 禁止注册的资源类型
以下资源**不得注册**，否则将返回错误：
- 主呈现目标（primary render target）
- 共享资源（shared resources）
- 深度/模板表面（depth/stencil surfaces）

## 错误处理
注册失败时可能返回：
- `cudaErrorInvalidResourceHandle`：资源类型不匹配
- `cudaErrorUnknown`：资源格式不受支持

确保在注册前验证资源类型与格式。