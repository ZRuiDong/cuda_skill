---
name: CUDA共享存储器Bank冲突优化
description: 当CUDA内核中共享存储器访问成为性能瓶颈时，应用此技能识别并消除bank冲突。适用于使用共享存储器数组、线程索引和特定内存访问模式的场景，需根据目标GPU的计算能力和bank组织结构调整优化策略。
---

# CUDA共享存储器Bank冲突优化

## 何时使用
- 共享存储器访问出现显著延迟或带宽未达预期
- 性能分析工具（如Nsight Compute）报告高shared memory bank conflict
- 内核使用规则但非对齐的访问模式（如列优先访问二维数组、跨步访问等）

## 执行步骤

### 1. 避免同warp线程访问同一bank
- 对于计算能力1.x设备：确保`threadIdx.x`与bank一一对应，例如使用`shared[tid]`
- 避免访问步长为bank数量因子的模式（如在16-bank系统中使用`shared[2 * tid]`会导致2路冲突）

### 2. 处理char型数据的特殊对齐
- 因4个连续char（共4字节）属于同一bank，应使用`shared[4 * tid]`而非`shared[tid]`以避免冲突

### 3. 利用硬件广播机制
- 当整个warp/half-warp读取相同地址时，硬件自动广播，不会产生bank冲突
- 可主动将常用标量值加载到单个线程再广播给同warp其他线程

### 4. 应用数组填充（padding）
- 对于按列访问的二维数组（如`shared[32][32]`），声明为`shared[32][33]`
- 每行末尾添加一个填充元素，使不同行的同列元素落在不同bank

### 5. 谨慎使用矩阵转置
- 在某些访问模式下，可先转置数据再访问
- 注意转置过程本身可能引入新的bank冲突，需同步优化

## 输入参数
- `data_type`：数据类型（如`float`、`int`、`char`）
- `access_pattern`：访问模式（如`row-wise`、`column-wise`、`strided`）
- `array_dimensions`：数组维度信息

## 输出
提供具体的代码修改建议，包括数组声明调整、索引计算重写或访问模式重构，以消除bank冲突。