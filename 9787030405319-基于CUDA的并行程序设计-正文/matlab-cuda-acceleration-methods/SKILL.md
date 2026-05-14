---
name: MATLAB CUDA加速方法选择
description: 本技能帮助MATLAB用户、图像处理开发者和高性能计算工程师在具备NVIDIA GPU的环境下，根据硬件条件、预算和开发经验，选择最适合的CUDA加速方法。当需要在MATLAB中加速数值计算或图像处理任务且已配置CUDA驱动与Toolkit时使用此技能。
---

# MATLAB CUDA加速方法选择

## 触发条件
当满足以下所有条件时应用本技能：
- 已安装MATLAB
- 具备NVIDIA GPU且计算能力≥2.1
- 已正确安装CUDA驱动和Toolkit（版本≥3.1）
- 需要加速MATLAB中的数值或图像处理任务

## 四种主要加速方法

### 1. MATLAB自带方法（Parallel Computing Toolbox）
- **适用场景**：标准GPU加速需求，无需额外依赖
- **操作步骤**：
  1. 使用`gpuArray`将数据从工作空间传输至GPU
  2. 调用支持GPU的内建函数（如`fft`, `mldivide`）
  3. 必要时通过`parallel.gpu.CUDAKernel`执行PTX内核
- **要求**：GPU计算能力≥2.1，CUDA Toolkit≥3.1

### 2. Jacket软件包
- **适用场景**：需高性能、多GPU支持，且可接受商业许可成本
- **操作步骤**：
  1. 安装Jacket中间件
  2. 直接调用重写的500+ GPU兼容函数
  3. 使用GFOR循环进行并行化
- **注意**：部分图像处理函数（如`imopen`）仅部分支持

### 3. GPUmat工具箱
- **适用场景**：开源免费方案，快速迁移现有代码
- **操作步骤**：
  1. 安装GPUmat（支持Windows/Linux）
  2. 将变量声明为GPU类型（如`gdouble`）
  3. 原有MATLAB代码基本无需修改即可在GPU运行
- **限制**：性能通常低于Jacket

### 4. MATLAB CUDA插件法（MEX + CUDA C）
- **适用场景**：需极致性能优化，具备C/C++和CUDA开发能力
- **操作步骤**：
  1. 编写CUDA C内核实现核心算法（如2D FFT）
  2. 通过`mexFunction`封装为MEX文件
  3. 在MATLAB中调用该MEX函数替代原生实现
- **优势**：可实现高达14倍加速

## 方法选择决策树
1. **无C/C++经验且追求简单** → 优先选MATLAB自带方法或GPUmat
2. **需最高性能且有预算** → 选用Jacket
3. **需定制化极致优化** → 采用CUDA插件法
4. **跨平台开源需求** → GPUmat（注意性能权衡）

> 注意：所有方法均不适用于无GPU环境；部分方法对操作系统有特定要求，请查阅对应文档。