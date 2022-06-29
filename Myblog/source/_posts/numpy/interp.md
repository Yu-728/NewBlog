---
title: 线性插值 np.interp()
---
# 线性插值 np.interp()
<!--more-->
函数：y = np.interp(x, xp, fp, left, right, period)

单调增加样本点的一维线性插值

将一维分段线性插值返回给具有给定离散数据点 (xp, fp) 的函数，在 x 处求值。

x: 所求位置的x坐标值

xp:离散点的x坐标值范围

fp:离散点的y坐标值范围

left:当索引超出[xp,fp]时的左侧值（x<xp时在x位置的取值）

right:当索引超出[xp,fp]时的右侧值（y>fp时在x位置的取值）

period:

Examples
    --------
    >>> xp = [1, 2, 3]
    >>> fp = [3, 2, 0]
    >>> np.interp(2.5, xp, fp)
    1.0

```
line_len = 50 # line_len 的取值范围30~190
min_volume = 0
max_volume = 100
 vol = np.interp(line_len, [30, 190], [min_volume, max_volume])
```
![示例](https://s3.bmp.ovh/imgs/2022/05/16/6932d4620da7d251.jpg)