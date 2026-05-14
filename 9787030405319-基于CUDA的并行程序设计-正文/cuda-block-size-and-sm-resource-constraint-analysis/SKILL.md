---
name: CUDA Block尺寸与SM资源约束分析
description: 根据目标GPU的SM资源规格和内核资源需求，计算每个SM可容纳的最大活动block数，以指导block尺寸设计。当设计或优化CUDA内核的block/grid配置时使用本技能，确保资源利用率高且满足最小并发要求（至少2个活动block，6个warp）。
---

# CUDA Block尺寸与SM资源约束分析

## 何时使用
- 正在设计CUDA内核的block线程数（如128、256、512）
- 需要评估寄存器、共享内存或warp数量对SM并发能力的限制
- 性能调优阶段发现occupancy过低

## 执行步骤

1. **收集输入参数**：
   - `threads_per_block`：计划使用的每个block线程数
   - `regs_per_thread`：每个线程使用的寄存器数（可通过`nvcc --ptxas-options=-v`获取）
   - `shared_mem_per_block`：每个block使用的共享内存字节数
   - 目标GPU的SM规格（如A100：寄存器65536/SM，共享内存164KB/SM，最大block数32/SM等）

2. **计算各资源维度下的最大活动block数**：
   - 共享内存限制：`SM_total_shared_mem / shared_mem_per_block`
   - 寄存器限制：`SM_total_regs / (regs_per_thread × threads_per_block)`
   - Warp总数限制：`SM_max_warps / (threads_per_block / 32)`
   - Block总数上限：`SM_max_blocks`

3. **确定实际活动block数**：取上述四项的最小值

4. **验证设计合理性**：
   - 若结果 < 2，必须优化内核（减少寄存器或共享内存使用）或调整block尺寸
   - block尺寸不应小于64
   - 优先选择能提升occupancy且利于线程通信的尺寸

## 输出
返回每个SM的实际活动block数，用于判断当前配置是否满足性能设计目标。