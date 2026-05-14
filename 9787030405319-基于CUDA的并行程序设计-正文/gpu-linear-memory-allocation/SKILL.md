---
name: GPU线性显存分配
description: 在CUDA上下文中为GPU设备分配未初始化的线性内存块。当需要在GPU上动态申请指定字节数的线性存储空间（如用于后续数据传输或计算）时使用此技能，适用于设备指针操作但不适用于CUDA数组或主机内存。
---

# GPU线性显存分配

## 何时使用
- 需要在GPU设备上分配未初始化的线性内存块
- 已成功初始化CUDA上下文
- 分配目标为设备指针（非CUDA数组、非主机内存）
- 具备有效的size_t类型字节数参数

## 执行步骤
1. 调用 `cudaError_t cudaMalloc(void **devPtr, size_t count)`
2. 系统在GPU设备上分配 `count` 字节的线性存储器
3. 成功分配后，将指向该内存块的设备指针写入 `*devPtr`
4. 注意：所分配内存内容为未定义，不会自动清零
5. 内存对齐已根据底层硬件和类型自动处理
6. 检查返回值：
   - `cudaSuccess` 表示分配成功
   - `cudaErrorMemoryAllocation` 表示失败（如显存不足）
7. 成功分配的指针可用于 `cudaMemcpy`、`cudaMemset` 等后续操作
8. 必须通过 `cudaFree()` 显式释放内存，避免显存泄漏

## 关键变量
- `devPtr` (`void**`)：输出参数，接收分配所得的设备指针地址
- `count` (`size_t`)：要分配的字节数

## 注意事项
- 不可用于CUDA数组分配
- 不适用于主机内存（应使用 `malloc` 或 `cudaMallocHost`）
- 未释放的内存将导致显存泄漏
- 分配失败时 `*devPtr` 的值未定义，不应使用