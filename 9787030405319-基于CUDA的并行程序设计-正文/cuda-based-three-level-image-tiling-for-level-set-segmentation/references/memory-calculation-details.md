# 显存使用估算细节

每个像素在水平集分割中通常涉及多个中间变量（如距离场、速度场、标志位等），保守估计每个像素需11字节存储空间。该值可根据具体算法调整。

显存使用率阈值（`MemoryUseRate`）通常设为0.8~0.9，以预留安全余量防止OOM。

## 边界处理逻辑

对于最后一块图像区域，其实际尺寸可能小于标准块尺寸。应通过以下方式计算：

```
actual_width = (block_col == ImgBlockCol - 1) ? (lWidth - offset_w) : ImgWidth;
actual_height = (block_row == ImgBlockRow - 1) ? (lHeight - offset_h) : ImgHeight;
```

## 分块尺寸数组 psize 构建示例

使用模运算均匀分配余数：

```
for (int i = 0; i < wp; ++i) {
    psize[i] = ImgPiecewidth + ((i < lWidth % ImgWp) ? 1 : 0);
}
```

类似逻辑适用于高度方向。