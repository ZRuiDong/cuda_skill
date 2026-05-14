# LUT构建公式（公式8.3.1）

给定归一化阈值 `dLow`（默认0.01）和 `dHigh`（默认0.99），对累积直方图值 `cumsum`（归一化到[0,1]）：

- 若 `cumsum ≤ dLow`，则 `Lut = 0`
- 若 `dLow < cumsum < dHigh`，则  
  `Lut = 255 × [low_out + (high_out - low_out)/(high_in - low_in) × (cumsum - low_in)]`
- 若 `cumsum ≥ dHigh`，则 `Lut = 255`

其中 `low_in`, `high_in`, `low_out`, `high_out` 均为[0,1]区间内的浮点数，定义输入/输出的对比度拉伸范围。