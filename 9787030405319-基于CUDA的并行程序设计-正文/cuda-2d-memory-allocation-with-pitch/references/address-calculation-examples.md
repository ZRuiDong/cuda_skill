# 地址计算与使用示例

## 元素地址计算
给定基地址 `void* d_ptr`、实际行宽 `pitch`、数据类型 `float`，访问第 `row` 行、第 `col` 列元素：

```cpp
float* row_ptr = (float*)((char*)d_ptr + row * pitch);
float value = row_ptr[col];
```

## 完整分配与释放示例
```cpp
void** d_ptr;
size_t pitch;
size_t widthInBytes = width * sizeof(float);

// 分配
cudaError_t err = cudaMallocPitch(d_ptr, &pitch, widthInBytes, height);
if (err != cudaSuccess) {
    // 处理错误
}

// 使用...

// 释放
cudaFree(d_ptr);
```

## 性能说明
- 对齐可提升全局内存合并访问效率
- 在Tesla/Volta/Ampere架构上，对齐至128字节边界可获得最佳性能
- 不对齐可能导致带宽下降30%以上