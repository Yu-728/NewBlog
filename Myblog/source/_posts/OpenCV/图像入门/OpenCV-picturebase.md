---
title: Opencv 入门篇学习记录（图片）
---
# Opencv 入门篇学习记录(图片)
<!--more-->
## 前言
很早以前就接触Python了，大学的时候还自学了一段时间去做了课设，写了一些最速梯度下降法、黄金分割法、步长优化什么的。现在已经忘得差不多了。。。
最近自学接触了OpenCV图像处理这个库，把自己的python学习过程记录一下。
相关内容我都写到代码里了，直接贴代码，也方便复现
## 图像入门基本函数：

```
# coding:utf-8

import numpy as np
import cv2
import matplotlib.pyplot as plt

# 加载图片
'''
imread(img_path,flag) 读取图片，返回图片对象
    img_path: 图片的路径，即使路径错误也不会报错，但打印返回的图片对象为None,路径前记得加r
    flag：cv2.IMREAD_COLOR，读取彩色图片，图片透明性会被忽略，为默认参数，也可以传入1
          cv2.IMREAD_GRAYSCALE,按灰度模式读取图像，也可以传入0
          cv2.IMREAD_UNCHANGED,读取图像，包括其alpha通道，也可以传入-1
'''
img = cv2.imread(r"C:\Users\Admin\Desktop\opencv\jumao-001.jpg")

'''
cvtColor()函数，用于在图像中不同的色彩空间进行转换，用于后续处理
'''
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

'''
图像的二值化，就是将图像上的像素点的灰度值设置为0或255，也就是将整个图像呈现出明显的只有黑和白的视觉效果。
cv2.threshold(img, threshold, maxval,type)
threshold是设定的阈值
maxval是当灰度值大于（或小于）阈值时将该灰度值赋成的值
type规定的是当前二值化的方式
'''
ret, img_threshold = cv2.threshold(img_gray, 127, 255, cv2.THRESH_BINARY)

'''
imshow(window_name,img)：显示图片，窗口自适应图片大小
    window_name: 指定窗口的名字
    img：显示的图片对象
    可以指定多个窗口名称，显示多个图片
'''
cv2.imshow("img", img)
cv2.imshow("thre", img_threshold)

'''
waitKey(millseconds)  键盘绑定事件，阻塞监听键盘按键，返回一个数字（不同按键对应的数字不同）
    millseconds: 传入时间毫秒数，在该时间内等待键盘事件；传入0时，会一直等待键盘事件
    
destroyAllWindows(window_name) 
    window_name: 需要关闭的窗口名字，不传入时关闭所有窗口
'''
key = cv2.waitKey(0)
if key == 27:  # 按esc键时，关闭所有窗口
    print(key)
    cv2.destroyAllWindows()
'''
imwrite(img_path_name,img)
    img_path_name:保存的文件名,路径前记得加r
    img：文件对象
'''
cv2.imwrite(r"C:\Users\Admin\Desktop\opencv\jumao-001.jpg", img)
cv2.imwrite(r"C:\Users\Admin\Desktop\opencv\jumao-002.jpg", img_threshold)

```

## OpenCV画图方法
```
import numpy as np
import cv2 as cv

'''
opencv常用画图方法
'''

# 创建黑色的图像
img = np.zeros((512, 512, 3), np.uint8)
# 绘制一条厚度为5的蓝色对角线
#       窗口图像名 起点坐标   终点坐标   RGB颜色（蓝色）线宽
cv.line(img, (0, 0), (511, 511), (255, 0, 0), 5)
#           窗口图像名   起点       终点        RGB颜色（绿色） 线宽
cv.rectangle(img, (384, 0), (510, 128), (0, 255, 0), 3)  # 画法为左上往右下的对角线所形成的矩形
#               圆心       半径   RGB       是否填充
cv.circle(img, (447, 63), 63, (0, 0, 255), 2)  # -1：填充，其他值不填充，作为线宽参数，默认线宽为1
# 绘制多边形
pts = np.array([[10, 5], [20, 30], [70, 20], [50, 10]], np.int32)
# pts = pts.reshape((-1, 1, 2))
# ture:为封闭图形 False:为多段折线，首尾那段不连接
cv.polylines(img, [pts], True, (0, 255, 255))
# 绘制文字
# 字体设置
font = cv.FONT_HERSHEY_SIMPLEX
# 绘制函数 绘制窗口名 绘制内容  起始位置 字体设置 字体大小比例  RGB 绘制线条粗细程度 线性  图像数据原点True为左下角，否则左上角(默认)
cv.putText(img, 'OpenCV', (10, 500), font, 4, (255, 255, 255), 2, cv.LINE_AA)

# 查看图像
cv.imshow('image', img)
k = cv.waitKey(0)
if k == 27:  # 等待ESC退出
    cv.destroyAllWindows()
elif k == ord('s'):  # 等待关键字，保存和退出
    cv.imwrite(r"C:\Users\Admin\Desktop\opencv\draw-001.jpg", img)
    cv.destroyAllWindows()

```

