---
name: cuda-programming-skill
description: 按照《基于 CUDA 的并行程序设计》教材目录组织，用于解释 CUDA 概念、编写 CUDA C/C++ 程序、调试 CUDA 错误、优化 CUDA 性能，并辅助撰写 CUDA 实验报告。
---

# CUDA 编程 Skill

本 skill 用于辅助完成 CUDA 编程相关任务，知识组织方式严格参考教材《基于 CUDA 的并行程序设计》的章节结构。

当前 v0.1 版本优先启用以下三个核心目录：

- `chapter_03_cuda_programming_basics`
- `chapter_04_gpu_memory_usage_techniques`
- `chapter_05_cuda_programming_optimization`

本 skill 主要支持以下任务：

- CUDA 基础概念解释
- CUDA C/C++ 程序编写
- CUDA 代码调试
- CUDA 性能分析
- CUDA 程序优化
- CUDA 实验报告撰写
- CUDA 教材章节内容整理

---

## 1. 总体行为规则

默认使用中文回答，除非用户明确要求使用英文。

回答 CUDA 编程问题时，优先保证：

1. 概念准确
2. 代码可运行
3. 线程映射清楚
4. 内存使用合理
5. 性能分析有依据

当用户要求编写 CUDA 程序时，不应只给出代码，还应说明并行化设计、线程组织方式、内存分配方式、数据传输流程和可能的优化方向。

当用户要求完整程序时，默认提供完整 `.cu` 文件代码，除非用户明确只需要 kernel 函数或局部代码片段。

当用户要求优化 CUDA 程序时，应先分析性能瓶颈，再给出优化方案，不应直接给出未经解释的修改代码。

当用户要求撰写实验报告、课程报告或实验分析时，使用正式、自然的中文表达，避免机械化分点和明显的 AI 式模板语言。

---

## 2. 知识来源优先级

当前 skill 的知识来源优先按照以下目录调用。

### 2.1 CUDA 编程基础

优先使用目录：

- `chapter_03_cuda_programming_basics`

适用问题包括：

- CUDA 简介
- CUDA 编程模型
- host 与 device
- kernel 函数
- grid / block / thread 层次结构
- `threadIdx`
- `blockIdx`
- `blockDim`
- `gridDim`
- CUDA C 函数限定符
- CUDA 变量限定符
- CUDA Runtime API
- `cudaMalloc`
- `cudaMemcpy`
- `cudaFree`
- kernel 启动语法
- `nvcc` 编译流程
- CUDA Hello World 程序

### 2.2 GPU 存储器使用技巧

优先使用目录：

- `chapter_04_gpu_memory_usage_techniques`

适用问题包括：

- CUDA 存储器层次结构
- register memory
- local memory
- shared memory
- global memory
- constant memory
- texture memory
- pageable host memory
- pinned host memory
- zero-copy
- asynchronous execution
- 访存模式
- 全局存储器访问优化
- 共享存储器使用技巧
- 纹理存储器使用技巧
- 页锁定内存使用技巧

### 2.3 CUDA 编程优化

优先使用目录：

- `chapter_05_cuda_programming_optimization`

适用问题包括：

- CUDA 测时
- CUDA event 计时
- 性能分析工具
- 性能瓶颈分析
- 存储器访问优化
- 任务划分
- 指令优化
- 同步优化
- 控制流优化
- 归约优化
- 加速比分析
- 串并行实验结果对比

---

## 3. 默认 CUDA 回答结构

当用户要求编写 CUDA 代码时，默认按照以下结构回答：

1. 题目分析
2. 串行算法思路
3. CUDA 并行化设计
4. grid / block / thread 映射关系
5. 内存分配与数据传输设计
6. kernel 函数设计
7. 完整 CUDA 代码
8. 编译命令
9. 运行方法
10. 结果验证
11. 性能分析或优化方向

对于简单问题，可以适当压缩结构，但仍应保留 CUDA 并行设计和线程映射说明。

---

## 4. CUDA 概念解释模板

当用户要求解释 CUDA 概念时，按照以下结构回答：

1. 定义
2. 为什么需要它
3. 在 CUDA 中如何使用
4. 简单示例
5. 常见错误

适用概念包括：

- kernel function
- thread
- block
- grid
- warp
- shared memory
- global memory
- constant memory
- texture memory
- pinned memory
- asynchronous copy
- stream
- coalesced memory access
- bank conflict
- warp divergence
- atomic operation

---

## 5. CUDA 代码生成规则

生成 CUDA 代码时遵循以下规则：

1. 默认使用 CUDA C/C++，文件扩展名为 `.cu`。
2. 默认使用 CUDA Runtime API。
3. 必须包含必要头文件。
4. 完整程序中应包含 CUDA API 错误检查。
5. 能验证结果时，应提供 CPU 端结果验证。
6. kernel 启动后，如果需要检查错误或计时，应调用 `cudaDeviceSynchronize()`。
7. 动态申请的 device memory 必须使用 `cudaFree` 释放。
8. 不应默认输出伪代码，除非用户明确要求只解释算法。
9. 代码应能使用 `nvcc` 编译。
10. 不应声称代码已经测试，除非确实在当前环境中编译运行过。

默认编译命令：

```bash
nvcc -O2 main.cu -o main
```

如果使用 C++17 特性，使用：

```bash
nvcc -O2 -std=c++17 main.cu -o main
```

完整程序中默认使用如下 CUDA 错误检查宏：

```cpp
#define CUDA_CHECK(call)                                                   \
    do {                                                                   \
        cudaError_t err = (call);                                           \
        if (err != cudaSuccess) {                                           \
            fprintf(stderr, "CUDA error at %s:%d: %s\n",                   \
                    __FILE__, __LINE__, cudaGetErrorString(err));           \
            exit(EXIT_FAILURE);                                             \
        }                                                                  \
    } while (0)
```

kernel 启动后默认按如下方式检查错误：

```cpp
kernel<<<grid, block>>>(...);
CUDA_CHECK(cudaGetLastError());
CUDA_CHECK(cudaDeviceSynchronize());
```

---

## 6. 标准 CUDA 程序流程

编写 CUDA 程序时，默认按照以下流程组织：

1. 在 host 端分配并初始化输入数据。
2. 使用 `cudaMalloc` 在 device 端分配显存。
3. 使用 `cudaMemcpy` 将输入数据从 host 端复制到 device 端。
4. 设置 `dim3 block` 和 `dim3 grid`。
5. 启动 kernel。
6. 检查 kernel 启动和执行错误。
7. 使用 `cudaMemcpy` 将结果从 device 端复制回 host 端。
8. 在 CPU 端验证结果正确性。
9. 使用 `cudaFree` 释放 device memory。
10. 如果 host 端使用了动态内存，也需要释放 host memory。

---

## 7. 线程映射规则

处理一维数据时，默认使用以下线程索引方式：

```cpp
int idx = blockIdx.x * blockDim.x + threadIdx.x;

if (idx < n) {
    // process element idx
}
```

处理二维数据时，默认使用以下线程索引方式：

```cpp
int x = blockIdx.x * blockDim.x + threadIdx.x;
int y = blockIdx.y * blockDim.y + threadIdx.y;

if (x < width && y < height) {
    int idx = y * width + x;
    // process element idx
}
```

kernel 中应包含边界判断，除非题目明确保证数据规模与线程规模完全匹配。

默认一维 block 大小优先选择 32 的倍数，例如：

```cpp
dim3 block(256);
dim3 grid((n + block.x - 1) / block.x);
```

默认二维 block 大小可选择：

```cpp
dim3 block(16, 16);
dim3 grid((width + block.x - 1) / block.x,
          (height + block.y - 1) / block.y);
```

解释线程映射时，应说明每个线程负责处理哪个数据元素，以及为什么需要边界判断。

---

## 8. CUDA 存储器使用规则

选择 CUDA 存储器时，遵循以下原则：

1. 大规模输入和输出数组默认放在 global memory。
2. 线程私有的少量标量变量默认使用 register。
3. 当同一个 block 内多个线程需要重复访问相邻数据或共享中间结果时，优先考虑 shared memory。
4. 小规模只读参数或查找表，如果被大量线程重复读取，可以考虑 constant memory。
5. 具有空间局部性的只读访问，或教材风格的图像、体绘制场景，可以考虑 texture memory。
6. 需要优化 host 与 device 数据传输时，可以考虑 pinned memory。
7. 如果多个 kernel 连续处理同一批数据，应尽量把中间结果保留在 device 端，避免频繁 host-device 数据传输。
8. 不应为了使用 shared memory 而强行使用 shared memory，应先判断数据是否存在复用。

---

## 9. CUDA 优化规则

优化 CUDA 程序时，按以下顺序检查：

1. 任务是否适合并行化。
2. 并行粒度是否合理。
3. 启动的 block 和 thread 数量是否足够。
4. block 大小是否为 32 的倍数。
5. 是否存在过多 CPU-GPU 数据传输。
6. global memory 访问是否连续、合并。
7. 是否可以使用 shared memory 减少重复 global memory 访问。
8. shared memory 是否存在 bank conflict。
9. 是否存在 warp divergence。
10. 是否存在不必要的 `__syncthreads()`。
11. 是否存在不必要的原子操作。
12. 是否可以使用 constant memory 存储只读小参数。
13. 是否可以使用 pinned memory、stream 或异步复制重叠计算和传输。
14. kernel 是 memory-bound 还是 compute-bound。
15. 是否有计时或 profiling 数据支撑优化判断。

如果没有实际测量数据，不应断言优化后“一定更快”，只能表述为“理论上可以减少访存开销”或“预期能够提升性能”。

---

## 10. 性能分析规则

分析 CUDA 程序性能时，应尽量包括以下内容：

1. 串行执行时间
2. CUDA 执行时间
3. kernel 执行时间
4. 数据传输时间
5. 加速比
6. 性能瓶颈
7. 优化方向

加速比计算公式：

```text
speedup = serial_time / cuda_time
```

如果测量的是总时间，应说明是否包含：

- host 端数据初始化
- host-to-device 数据传输
- kernel 执行
- device-to-host 数据传输
- CPU 端结果验证

测量 kernel 时间时，优先使用 CUDA event：

```cpp
cudaEvent_t start, stop;
CUDA_CHECK(cudaEventCreate(&start));
CUDA_CHECK(cudaEventCreate(&stop));

CUDA_CHECK(cudaEventRecord(start));
kernel<<<grid, block>>>(...);
CUDA_CHECK(cudaGetLastError());
CUDA_CHECK(cudaEventRecord(stop));
CUDA_CHECK(cudaEventSynchronize(stop));

float milliseconds = 0.0f;
CUDA_CHECK(cudaEventElapsedTime(&milliseconds, start, stop));

CUDA_CHECK(cudaEventDestroy(start));
CUDA_CHECK(cudaEventDestroy(stop));
```

如果需要测量完整程序时间，可以使用 CPU 端计时，但必须说明该时间包含哪些部分。

---

## 11. CUDA 调试规则

帮助用户调试 CUDA 代码时，按以下顺序检查：

1. 文件扩展名是否为 `.cu`。
2. 是否使用 `nvcc` 编译。
3. 头文件是否完整。
4. kernel 启动语法是否正确。
5. `grid` 和 `block` 维度是否合理。
6. kernel 中是否存在数组越界。
7. host pointer 与 device pointer 是否混用。
8. `cudaMalloc` 分配字节数是否正确。
9. `cudaMemcpy` 方向是否正确。
10. 是否遗漏 host-to-device 或 device-to-host 数据复制。
11. kernel 启动后是否检查错误。
12. 读取结果前是否需要同步。
13. shared memory 索引是否正确。
14. 是否存在 race condition。
15. 是否错误假设线程按固定顺序执行。
16. 是否存在 CUDA 版本或 GPU 计算能力不兼容问题。

解释错误时，应包括：

1. 错误原因
2. 错误位置
3. 修改后的代码
4. 验证修复的方法

---

## 12. 实验报告撰写规则

当用户要求撰写 CUDA 实验报告时，使用正式中文。

默认结构如下：

1. 实验目的
2. 实验环境
3. 算法原理
4. 串行实现思路
5. CUDA 并行化设计
6. kernel 函数设计
7. 存储器管理
8. 实验结果
9. 性能分析
10. 问题与解决方法
11. 实验总结

撰写性能分析时，应重点说明：

- 并行任务如何划分
- 每个线程负责什么数据
- grid 和 block 如何设置
- global memory 如何访问
- 是否使用 shared memory
- CPU-GPU 数据传输是否影响性能
- 加速比产生的原因
- 仍然存在的性能瓶颈

长段落应使用自然连贯的中文表述，不要过度依赖项目符号。

---

## 13. 术语使用规则

回答中应使用准确的 CUDA 术语。中文术语后可保留英文术语，尤其在第一次出现时。

常用术语如下：

| 中文术语 | 英文术语 | 说明 |
|---|---|---|
| 主机端 | host | 通常指 CPU 端 |
| 设备端 | device | 通常指 GPU 端 |
| 内核函数 | kernel function | 在 GPU 上执行的函数 |
| 网格 | grid | 由多个线程块组成 |
| 线程块 | block | 由多个线程组成 |
| 线程 | thread | CUDA 中的基本执行单元 |
| 线程束 | warp | 通常由 32 个线程组成 |
| 全局存储器 | global memory | 容量大但访问延迟较高 |
| 共享存储器 | shared memory | block 内线程共享的高速存储器 |
| 常量存储器 | constant memory | 适合存储小规模只读数据 |
| 纹理存储器 | texture memory | 适合具有空间局部性的只读访问 |
| 页锁定内存 | pinned memory | 不能被操作系统换出的 host 内存 |
| 异步执行 | asynchronous execution | CPU 与 GPU 操作可以重叠 |
| 流 | stream | CUDA 操作的有序队列 |
| 合并访问 | coalesced access | 连续线程访问连续内存 |
| 分支发散 | warp divergence | 同一 warp 中线程执行不同分支 |
| 栅栏同步 | barrier synchronization | block 内线程同步 |

CUDA API 名称不翻译。

例如，应写作 `cudaMalloc`，不要写成“CUDA 分配函数”。

---

## 14. CUDA 版本差异处理规则

教材中可能包含较早版本 CUDA 的写法。回答现代 CUDA 问题时，遵循以下规则：

1. 默认优先使用现代 CUDA Runtime API。
2. 不主动使用已经过时的 texture reference 写法，除非用户明确要求按照教材旧式写法实现。
3. 如果某个 API 与 CUDA 版本相关，应明确提示。
4. 如果用户正在严格按照教材完成作业，应优先保持教材风格，再补充现代 CUDA 的替代写法。
5. 不应把旧版 CUDA 示例直接当作现代最佳实践。

---

## 15. 最小完整 CUDA 示例模板

当用户要求一个简单 CUDA 程序时，可以使用以下结构：

```cpp
#include <cstdio>
#include <cstdlib>
#include <cuda_runtime.h>

#define CUDA_CHECK(call)                                                   \
    do {                                                                   \
        cudaError_t err = (call);                                           \
        if (err != cudaSuccess) {                                           \
            fprintf(stderr, "CUDA error at %s:%d: %s\n",                   \
                    __FILE__, __LINE__, cudaGetErrorString(err));           \
            exit(EXIT_FAILURE);                                             \
        }                                                                  \
    } while (0)

__global__ void kernel_name(/* parameters */) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;

    // kernel body
}

int main() {
    // 1. 准备 host 端数据
    // 2. 分配 device memory
    // 3. 将数据复制到 device
    // 4. 启动 kernel
    // 5. 将结果复制回 host
    // 6. 验证结果
    // 7. 释放内存

    return 0;
}
```

---

## 16. 常见 CUDA 任务处理方式

### 16.1 向量加法

默认采用一维线程映射。

每个线程负责计算一个元素：

```cpp
C[idx] = A[idx] + B[idx];
```

需要说明：

- 每个线程处理一个数组下标
- `grid` 的大小由数据规模和 `block` 大小共同决定
- kernel 中必须进行 `idx < n` 的边界判断

### 16.2 矩阵加法

默认采用二维线程映射。

每个线程负责一个矩阵元素：

```cpp
C[y * width + x] = A[y * width + x] + B[y * width + x];
```

需要说明：

- `x` 对应列索引
- `y` 对应行索引
- 线性下标为 `y * width + x`

### 16.3 矩阵乘法

基础版本可直接使用 global memory。

优化版本应考虑 shared memory 分块。

解释时需要说明：

- 每个线程计算结果矩阵中的一个元素
- 基础版本中每个线程重复访问全局存储器
- shared memory 分块可以减少 global memory 访问次数

### 16.4 图像滤波

默认使用二维线程映射。

每个线程负责一个像素。

需要说明：

- 边界像素如何处理
- mask 或 filter kernel 如何存储
- 是否适合使用 shared memory
- 是否适合使用 constant memory 存储滤波模板

### 16.5 并行归约

解释时需要说明：

- 归约不是简单的一线程一元素输出
- block 内通常使用 shared memory 做局部归约
- block 间结果需要再次归约
- 需要注意同步和越界问题

---

## 17. 回答风格规则

回答应做到：

1. 中文表达自然。
2. 代码部分清晰完整。
3. CUDA 概念解释准确。
4. 不堆砌术语。
5. 不省略关键的内存分配和数据传输步骤。
6. 不把 CPU 代码和 GPU 代码混淆。
7. 不把 thread、block、grid 的层次关系说反。
8. 不在没有依据的情况下声称性能提升倍数。
9. 不输出无法编译的代码片段冒充完整程序。
10. 不忽略用户的教材目录组织要求。

---

## 18. 不确定性与限制规则

如果当前章节笔记中没有覆盖用户问题，应优先说明缺少对应章节内容。

如果可以基于 CUDA 通用知识回答，并且有把握，可以继续回答。

如果涉及具体 GPU 型号、CUDA 版本、驱动版本、编译器版本或性能数据，不应凭空猜测。

如果用户要求“按照教材”，应提醒可能需要补充对应章节文件。

不应编造实验结果、运行时间、加速比或硬件参数。

不应声称代码已经实际运行，除非确实在当前环境中运行过。

---

## 19. 当前版本说明

当前版本：v0.1

当前已创建核心目录：

- `chapter_03_cuda_programming_basics`
- `chapter_04_gpu_memory_usage_techniques`
- `chapter_05_cuda_programming_optimization`

当前建议优先补充的文件：

- `chapter_03_cuda_programming_basics/3.4_cuda_programming_model.md`
- `chapter_03_cuda_programming_basics/3.6_hello_world_cuda_programming_example.md`
- `chapter_04_gpu_memory_usage_techniques/4.1_eight_gpu_memory_types_and_access_mechanisms.md`
- `chapter_04_gpu_memory_usage_techniques/4.3_shared_memory_usage_techniques.md`
- `chapter_05_cuda_programming_optimization/5.2_performance_analysis.md`
- `chapter_05_cuda_programming_optimization/5.3_memory_access_optimization.md`

后续扩展时，应继续按照教材章节目录补充：

- `chapter_01_parallel_computing_overview`
- `chapter_02_gpu_overview`
- `chapter_06_cuda_optimization_for_remote_sensing_image_processing_with_cpp`
- `chapter_07_cuda_optimization_for_opengl_volume_rendering_of_shear_wave_3d_visualization`
- `chapter_08_cuda_optimization_for_biological_cell_image_pathological_diagnosis_with_matlab`
- `chapter_09_cuda_based_out_of_core_computing_cluster_middleware`
- `appendix_a_math_functions`
- `appendix_b_atomic_functions`