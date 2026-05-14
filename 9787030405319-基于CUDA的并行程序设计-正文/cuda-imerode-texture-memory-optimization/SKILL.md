---
name: CUDA腐蚀操作的纹理内存优化实现
description: 使用CUDA纹理内存加速灰度图像的形态学腐蚀操作，适用于结构元素固定且需优化非合并全局内存访问的场景。当需要对灰度图像执行基于十字形或圆盘形等预定义结构元素的腐蚀，并希望利用纹理缓存提升二维空间局部性访问性能时，使用本技能。
---

# CUDA腐蚀操作的纹理内存优化实现

## 适用条件
- 输入为uint8灰度图像
- 结构元素（如十字形、圆盘形）已知且在运行时固定不变
- 需要加速因结构元素导致的非合并全局内存访问
- 边缘像素可接受特殊处理（如忽略或边界填充）

## 执行步骤
1. **定义2D只读纹理引用**：声明`texture<unsigned char, 2, cudaReadModeElementType> texRef`。
2. **绑定输入图像到纹理**：将输入灰度图像绑定至`texRef`，以启用纹理缓存优化二维空间局部性。
3. **启动CUDA核函数**：每个线程对应输出图像的一个像素位置`(xIdx, yIdx)`，其中：
   - `xIdx = blockIdx.x * blockDim.x + threadIdx.x`
   - `yIdx = blockIdx.y * blockDim.y + threadIdx.y`
4. **边界检查**：若`xIdx ≥ uiRow`或`yIdx ≥ uiCol`，线程直接返回。
5. **邻域采样**：根据预定义结构元素（如十字形或圆盘形），使用`tex2D(texRef, x, y)`从纹理中读取邻域像素值，存入数组`ucDomain`。
6. **计算最小值**：遍历`ucDomain`（通常包含15个有效位置），找出最小值`ucDomainMin`。
7. **写入输出**：将`ucDomainMin`写入输出缓冲区：`d_prBmpBufOut[xIdx + yIdx * uiRow] = ucDomainMin`。
8. **MEX接口调用**：接收单个uint8输入矩阵，创建同尺寸输出矩阵，并调用`CUDA_imerode`函数完成纹理绑定与核函数执行。

## 注意事项
- 不适用于结构元素动态变化的场景
- 纹理内存仅支持只读访问
- 输出图像尺寸与输入一致
- 邻域大小由结构元素决定（如15点）

> 提示：若结构元素复杂或需支持多种形状，请参考`references/structure-elements.md`中的预定义模板。