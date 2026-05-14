---
name: CUDA共享存储器分配方法
description: 为CUDA kernel函数正确声明和分配共享存储器，支持静态（编译时确定大小）和动态（运行时由主机指定大小）两种方式。当需要在kernel中使用共享存储器以提升线程间数据交换效率时使用本技能。
---

# CUDA共享存储器分配方法

## 何时使用
- 在编写CUDA kernel函数时，需要在线程块内共享数据
- 已知共享内存大小固定（使用静态分配）或仅在运行时才能确定大小（使用动态分配）
- 需要在同一kernel中同时使用静态和动态共享内存

## 如何执行

### 1. 静态分配
- 在kernel函数内部使用 `__shared__` 关键字声明固定大小数组：
  ```cpp
  __shared__ float s_static[100];
  ```
- 数组大小必须在编译时确定，不能初始化
- 适用于大小已知且不变的场景

### 2. 动态分配
- 在kernel函数内部使用 `extern __shared__` 声明未指定大小的数组：
  ```cpp
  extern __shared__ float s_dynamic[];
  ```
- 在主机端调用kernel时，通过第三个参数传入所需字节数：
  ```cpp
  unsigned int mem_size = N * sizeof(float);
  mykernel<<<blocks, threads, mem_size>>>();
  ```
- 适用于运行时才能确定共享内存大小的场景

### 3. 注意事项
- 共享存储器不能在声明时初始化
- 静态与动态分配可在同一kernel中共存
- 动态分配的实际大小由主机端控制，kernel内视为未定长数组

确保根据实际需求选择合适的分配方式，并正确设置主机端调用参数。