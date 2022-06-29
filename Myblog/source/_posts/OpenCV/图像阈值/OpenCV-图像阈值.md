---
title: OpenCV-图像阈值学习
---
#  OpenCV-图像阈值学习
涉及函数：
cv.threshold
cv.adaptiveThreshold
ret8, th2 = cv.threshold(img, 0, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)
cv.GaussianBlur
学习代码如下：
<!--more-->
```
"""
目标
在本教程中，您将学习简单阈值，自适应阈值和Otsu阈值。
你将学习函数**cv.threshold**和**cv.adaptiveThreshold**。
"""
import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt

'''
图像二值化：
定义：图像的二值化，就是将图像上的像素点的灰度值设置为0或255，
也就是将整个图像呈现出明显的只有黑和白的视觉效果。
灰度值0：黑  灰度值255：白
一幅图像包括目标物体、背景还有噪声，要想从多值的数字图像中直接提取出目标物体，
常用的方法就是设定一个阈值T，用T将图像的数据分成两部分：
大于T的像素群和小于T的像素群。
这是研究灰度变换的最特殊的方法，称为图像的二值化（Binarization）。

ret, dst = cv.threshold( src, thresh, maxval, type[, dst] ) 
参数说明：
src：原图像。
dst：结果图像。
thresh：当前阈值。
maxVal：最大阈值，一般为255.
thresholdType:阈值类型，主要有下面几种：
enum ThresholdTypes {
    THRESH_BINARY     = 0,  大于阈值的部分被置为255，小于部分被置为0
    THRESH_BINARY_INV = 1,  大于阈值部分被置为0，小于部分被置为255
    THRESH_TRUNC      = 2,  大于阈值部分被置为threshold，小于部分保持原样
    THRESH_TOZERO     = 3,  小于阈值部分被置为0，大于部分保持不变
    THRESH_TOZERO_INV = 4,  大于阈值部分被置为0，小于部分保持不变 
    THRESH_MASK       = 7,
    THRESH_OTSU       = 8,
    THRESH_TRIANGLE   = 16
};
返回值:
ret： 与参数thresh一致
dst： 结果图像
使用了THRESH_OTSU和THRESH_TRIANGLE两个标志时，输入图像必须为单通道。
'''
img = cv.imread(r'C:\Users\Admin\Desktop\opencv\jumao-002.jpg', 0)
ret, thresh1 = cv.threshold(img, 127, 255, cv.THRESH_BINARY)
ret1, thresh2 = cv.threshold(img, 127, 255, cv.THRESH_BINARY_INV)
ret2, thresh3 = cv.threshold(img, 127, 255, cv.THRESH_TRUNC)
ret3, thresh4 = cv.threshold(img, 127, 255, cv.THRESH_TOZERO)
ret4, thresh5 = cv.threshold(img, 127, 255, cv.THRESH_TOZERO_INV)
titles = ['Original Image', 'BINARY', 'BINARY_INV', 'TRUNC', 'TOZERO', 'TOZERO_INV']
images = [img, thresh1, thresh2, thresh3, thresh4, thresh5]
for i in range(6):
    plt.subplot(2, 3, i + 1), plt.imshow(images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])
plt.show()
'''
自适应阈值
在上一节中，我们使用一个全局值作为阈值。但这可能并非在所有情况下都很好，例如，如果图像在不同区域具有不同的光照条件。
在这种情况下，自适应阈值阈值化可以提供帮助。在此，算法基于像素周围的小区域确定像素的阈值。
因此，对于同一图像的不同区域，我们获得了不同的阈值，这为光照度变化的图像提供了更好的结果。
adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C, dst=None):
除上述参数外，方法**cv.adaptiveThreshold**还包含三个输入参数：
该**adaptiveMethod**决定阈值是如何计算的：
cv.ADAPTIVE_THRESH_MEAN_C::阈值是邻近区域的平均值减去常数**C**。 
cv.ADAPTIVE_THRESH_GAUSSIAN_C:阈值是邻域值的高斯加权总和减去常数**C**。
该**BLOCKSIZE**确定附近区域的大小，**C**是从邻域像素的平均或加权总和中减去的一个常数。
下面的代码比较了光照变化的图像的全局阈值和自适应阈值：
'''

img = cv.medianBlur(img, 5)
ret, th1 = cv.threshold(img, 127, 255, cv.THRESH_BINARY)
th2 = cv.adaptiveThreshold(img, 255, cv.ADAPTIVE_THRESH_MEAN_C,
                           cv.THRESH_BINARY, 11, 2)
th3 = cv.adaptiveThreshold(img, 255, cv.ADAPTIVE_THRESH_GAUSSIAN_C,
                           cv.THRESH_BINARY, 11, 2)
titles1 = ['Original Image', 'Global Thresholding (v = 127)',
          'Adaptive Mean Thresholding', 'Adaptive Gaussian Thresholding']
images = [img, th1, th2, th3]
for i in range(4):
    plt.subplot(2, 2, i + 1), plt.imshow(images[i], 'gray')
    plt.title(titles1[i])
    plt.xticks([]), plt.yticks([])
plt.show()
'''
Otsu的二值化
在全局阈值化中，我们使用任意选择的值作为阈值。相反，Otsu的方法避免了必须选择一个值并自动确定它的情况。
考虑仅具有两个不同图像值的图像（双峰图像），其中直方图将仅包含两个峰。一个好的阈值应该在这两个值的中间。
类似地，Otsu的方法从图像直方图中确定最佳全局阈值。
为此，使用了**cv.threshold**作为附加标志传递。阈值可以任意选择。然后，算法找到最佳阈值，该阈值作为第一输出返回。
'''


# 全局阈值
ret7, th1 = cv.threshold(img, 127, 255, cv.THRESH_BINARY)
# Otsu阈值
ret8, th2 = cv.threshold(img, 0, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)
'''
高斯滤波：
GaussianBlur(src, ksize, sigmaX, dst=None, sigmaY=None, borderType=None):
参数
src:输入图像
ksize:(核的宽度,核的高度)，输入高斯核的尺寸，核的宽高都必须是正奇数。否则，将会从参数sigma中计算得到。
dst:输出图像，尺寸与输入图像一致。
sigmaX:高斯核在X方向上的标准差。
sigmaY:高斯核在Y方向上的标准差。默认为None，如果sigmaY=0，则它将被设置为与sigmaX相等的值。
如果这两者都为0,则它们的值会从ksize中计算得到。计算公式为：
sigma = 0.3*((ksize - 1)* 0.5 -1)+0.8
borderType:像素外推法，默认为None（参考官方文档BorderTypes)
'''
# 高斯滤波后再采用Otsu阈值
blur = cv.GaussianBlur(img, (5, 5), 0)
ret3, th3 = cv.threshold(blur, 0, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)
# 绘制所有图像及其直方图
images = [img, 0, th1,
          img, 0, th2,
          blur, 0, th3]
titles = ['Original Noisy Image', 'Histogram', 'Global Thresholding (v=127)',
          'Original Noisy Image', 'Histogram', "Otsu's Thresholding",
          'Gaussian filtered Image', 'Histogram', "Otsu's Thresholding"]
for i in range(3):
    plt.subplot(3, 3, i * 3 + 1), plt.imshow(images[i * 3], 'gray')
    plt.title(titles[i * 3]), plt.xticks([]), plt.yticks([])
    plt.subplot(3, 3, i * 3 + 2), plt.hist(images[i * 3].ravel(), 256)
    plt.title(titles[i * 3 + 1]), plt.xticks([]), plt.yticks([])
    plt.subplot(3, 3, i * 3 + 3), plt.imshow(images[i * 3 + 2], 'gray')
    plt.title(titles[i * 3 + 2]), plt.xticks([]), plt.yticks([])
plt.show()

```