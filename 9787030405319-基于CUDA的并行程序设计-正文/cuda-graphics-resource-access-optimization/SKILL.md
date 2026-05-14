---
name: CUDA图形资源访问模式优化
description: 为已注册的CUDA图形资源设置最优访问标志（只读、只写丢弃或默认），以减少同步开销和数据迁移延迟。当应用程序明确知道CUDA对图形资源的访问模式（如只读渲染结果或完全覆盖写入）时使用此技能。
---

# CUDA图形资源访问模式优化

## 何时使用
- 图形资源（如OpenGL纹理或Direct3D缓冲区）已通过`cudaGraphics*Register*`注册
- 资源当前未映射
- 应用程序能预知CUDA对该资源的访问行为（只读、只写覆盖或读写）

## 执行步骤
1. **确认前提条件**：
   - 资源已注册且未映射
   - 设备支持图形互操作

2. **选择合适的访问标志**：
   - `cudaGraphicsMapFlagsNone`：默认，适用于读写场景
   - `cudaGraphicsMapFlagsReadOnly`：CUDA仅读取资源，驱动可跳过写同步
   - `cudaGraphicsMapFlagsWriteDiscard`：CUDA完全覆盖写入，驱动可丢弃原数据避免复制

3. **调用设置函数**：
   ```c
   cudaError_t err = cudaGraphicsResourceSetMapFlags(resource, flags);
   ```
   - 必须在首次映射前调用
   - 修改后的标志在下次映射时生效

4. **错误处理**：
   - 若资源已映射，返回`cudaErrorUnknown`
   - 若`flags`无效，返回`cudaErrorInvalidValue`
   - 使用`cudaGetErrorString()`解析错误

5. **跨API注意事项**：
   - OpenGL/Direct3D注册函数也接受`flags`参数
   - 映射时设置的`flags`优先级高于注册时的`flags`

> **性能提示**：对大纹理使用`WriteDiscard`可显著减少PCIe传输；`ReadOnly`可避免不必要的GPU缓存刷新。