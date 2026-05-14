# 偏导数计算公式

## 一阶偏导
- fx = 0.5 * (data[1][2] - data[1][0])
- fy = 0.5 * (data[2][1] - data[0][1])
- fs = 0.5 * (data[1][1] - datap[1][1])

## 二阶偏导
- fxx = data[1][0] + data[1][2] - 2*v
- fyy = data[0][1] + data[2][1] - 2*v
- fss = data[1][1] + datap[1][1] - 2*v
- fxy = 0.25 * (data[2][2] + data[0][0] - data[2][0] - data[0][2])
- fxs = 0.25 * (data[1][2] + datap[1][0] - datan[1][0] - datap[1][2])
- fys = 0.25 * (data[2][1] + datap[0][1] - datan[0][1] - datap[2][1])

其中：
- `data[y][x]` 表示当前尺度层的3×3邻域
- `datap[y][x]` 表示上一尺度层对应位置
- `datan[y][x]` 表示下一尺度层对应位置
- `v` 为当前DoG像素值