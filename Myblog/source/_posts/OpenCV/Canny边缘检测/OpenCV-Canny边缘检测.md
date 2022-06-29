---
title: Canny边缘检测算法
---
# Canny边缘检测算法基本原理
<!--more-->
1. 图像灰度化
2. 高斯滤波
3. 用一阶偏导的有限差分来计算梯度的幅值和方向
4. 对梯度幅值进行非极大值抑制
5. 用双阈值算法检测和连接边缘

参考：
[参考文档](https://blog.csdn.net/likezhaobin/article/details/6892176?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-6892176-blog-117639872.pc_relevant_antiscanv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-6892176-blog-117639872.pc_relevant_antiscanv3&utm_relevant_index=5)

# OpenCV中的实现
代码实现：
```
"""
Canny边缘检测的概念 - OpenCV函数: cv.Canny()
Canny算法
"""

import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt

img = cv.imread(r'C:\Users\admin\Desktop\OpenCV\people.jpg', 0)
'''
Canny(image, threshold1, threshold2, edges=None, apertureSize=None, L2gradient=None):
.   @param image 8-bit input image.
.   @param edges output edge map; single channels 8-bit image, which has the same size as image.
.   @param threshold1 first threshold for the hysteresis procedure.(minVal)
.   @param threshold2 second threshold for the hysteresis procedure.(maxVal, >maxVal ---> 255)
.   @param apertureSize aperture size for the Sobel operator.
.   @param L2gradient a flag, indicating whether a more accurate \f$L_2\f$ norm\f$=\sqrt{(dI/dx)^2 + (dI/dy)^2}\f$
    should be used to calculate the image gradient magnitude (L2gradient=true ), 
    or whether the default \f$L_1\f$ norm \f$=|dI/dx|+|dI/dy|\f$ is enough (L2gradient=false ).
'''
edges = cv.Canny(img, 50, 60)
plt.subplot(121), plt.imshow(img, cmap='gray')
plt.title('Original Image'), plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(edges, cmap='gray')
plt.title('Edge Image'), plt.xticks([]), plt.yticks([])
plt.show()

```
## Soble算子计算梯度
sobel算子主要用于获得数字图像的一阶梯度，常见的应用和物理意义是边缘检测。
原理：
算子使用两个3*3的矩阵（图1）算子使用两个3*3的矩阵（图1）去和原始图片作卷积，分别得到横向Ｇ（ｘ）和纵向Ｇ（ｙ）的梯度值，如果梯度值大于某一个阈值，则认为该点为边缘点

Gx方向的相关模板：
![图1](https://img-blog.csdn.net/20171117094349330?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1bGluemh1bGlubGlu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

Gy方向的相关模板：
![图2](https://img-blog.csdn.net/20171117094551248?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemh1bGluemh1bGlubGlu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)