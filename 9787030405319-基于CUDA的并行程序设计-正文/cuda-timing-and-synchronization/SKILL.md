---
name: CUDA测时与同步方法
description: 精确测量CUDA内核函数、异步API调用或主机-设备数据传输的执行时间。当需要对CUDA操作进行性能分析且要求时间测量准确可靠时使用本技能，特别注意必须正确处理GPU异步执行与流同步机制。
---

# CUDA测时与同步方法

## 何时使用
- 需要精确测量CUDA内核执行时间
- 需要评估异步内存拷贝（如 cudaMemcpyAsync）耗时
- 对CUDA程序进行性能剖析或优化
- 测量涉及默认流（0号流）的操作

## 执行步骤

### 方法一：CPU计时器（仅限0号流）
1. 在CUDA操作前记录CPU时间戳。
2. 启动CUDA内核或调用异步API。
3. **必须**调用 `cudaDeviceSynchronize()`（或旧版 `cudaThreadSynchronize()`）确保所有先前提交的操作完成。
4. 在同步后记录结束时间戳，计算差值得到耗时。
5. 注意：此方法仅对0号流（默认流）可靠，因其他流并发执行会导致计时不准确。

### 方法二：GPU事件测时（推荐）
1. 声明两个 `cudaEvent_t` 变量：`start` 和 `stop`。
2. 调用 `cudaEventCreate(&start)` 和 `cudaEventCreate(&stop)` 创建事件。
3. 在0号流中记录开始事件：`cudaEventRecord(start, 0)`。
4. 执行目标CUDA操作（内核启动或数据传输）。
5. 在0号流中记录结束事件：`cudaEventRecord(stop, 0)`。
6. 调用 `cudaEventSynchronize(stop)` 等待事件完成。
7. 使用 `cudaEventElapsedTime(&elapsed_time_ms, start, stop)` 获取毫秒级耗时（精度约0.5ms）。
8. 清理资源：调用 `cudaEventDestroy(start)` 和 `cudaEventDestroy(stop)`。

## 关键约束
- **仅0号流测时可靠**：默认流与其他流同步执行，因此其测时结果不包含其他流干扰。
- **避免在非0号流使用CPU计时器**：因异步并发，会导致测量值偏大或不可复现。
- **必须显式同步**：无论使用哪种方法，若未正确同步，测得的时间可能仅为启动开销而非实际执行时间。