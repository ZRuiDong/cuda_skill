---
name: CUDA页锁定内存与异步执行优化
description: 利用页锁定内存和CUDA流实现主机-设备数据传输与计算的重叠执行，隐藏数据传输延迟。当需要提升吞吐量且存在可并行的数据传输与计算任务时使用此技能。
---

# CUDA页锁定内存与异步执行优化

## 何时使用
- 需要隐藏主机与设备间的数据传输延迟
- GPU设备支持页锁定内存（`canMapHostMemory == 1`）
- 存在可拆分的数据块和独立计算任务

## 执行步骤
1. **启用主机内存映射**：
   ```c
   cudaSetDeviceFlags(cudaDeviceMapHost);
   ```
   - 必须在任何CUDA调用前设置

2. **分配页锁定内存**：
   ```c
   cudaHostAlloc((void**)&h_buffer, size, cudaHostAllocMapped);
   ```

3. **获取设备指针（零复制）**：
   ```c
   cudaHostGetDevicePointer((void**)&d_buffer, h_buffer, 0);
   ```
   - Kernel中直接使用`d_buffer`访问主机内存
   - 必须通过流或事件同步避免数据竞争

4. **配置异步执行流水线**：
   a) 创建非默认流：`cudaStreamCreate(&stream)`
   b) 异步传输：`cudaMemcpyAsync(a_d, a_h, size, cudaMemcpyHostToDevice, stream)`
   c) 流中启动Kernel：`kernel<<<grid, block, 0, stream>>>(a_d)`

5. **关键限制**：
   - 默认流（0号）是同步的，无法与其他流并发
   - 同步与异步传输不能重叠
   - 实际并发能力受GPU的`asyncEngineCount`限制

> **优化策略**：将大数组分为`nStreams`块，当计算时间Tc > 传输时间Tr时，并行执行时间≈Tc + Tr/nStreams。