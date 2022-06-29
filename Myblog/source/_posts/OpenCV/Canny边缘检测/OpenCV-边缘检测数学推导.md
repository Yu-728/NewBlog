---
title: 图像梯度算子推导（laplacian算子）
---
# 图像梯度理解
<!--more-->
参考链接：
[图像梯度理解1](https://blog.csdn.net/qq_36622009/article/details/102900447)
[图像梯度理解2](https://blog.csdn.net/saltriver/article/details/78987096)
[图像梯度理解3](https://blog.csdn.net/u011754972/article/details/121783295?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-5-121783295-blog-77775029.pc_relevant_default&spm=1001.2101.3001.4242.4&utm_relevant_index=8)

# 数学推理
一阶导数定义：
![一阶导数](https://s3.bmp.ovh/imgs/2022/06/02/20066692cad13396.jpg)

二阶导数定义：
![二阶导数](https://s3.bmp.ovh/imgs/2022/06/02/c33ba615e88030e9.jpg)

在数字图像处理中，像素点是离散的，并且最小的间距为1，也就是相邻像素之间距离为1，所以Δx最趋近于0的取值是1，即Δx = 1
所以推出：
![一阶导1](https://s3.bmp.ovh/imgs/2022/06/02/645338e2d28015d4.jpg)
![二阶导1](https://s3.bmp.ovh/imgs/2022/06/02/fa5e5e987d69b1c6.jpg)
由(1)(2)和(4)(5)得出：
![](https://s3.bmp.ovh/imgs/2022/06/02/7bc33b621e053118.jpg)
![](https://i.niupic.com/images/2022/06/02/a0fu.jpg)

将 x=x+1 、x=x-1 分别代入(1)然后再代入二阶导的公式得出：
![](https://i.niupic.com/images/2022/06/02/a0fy.jpg)
![](https://i.niupic.com/images/2022/06/02/a0fz.jpg)

我们看到(9)和(10)都是二阶导，但是他们相差2倍的关系。我们的目的是找到边缘，边缘就是该像素位置的一阶导(方向导数)最大或者说梯度(因为方向导数模最大的方向就是梯度方向)，要使得一阶导最大(即取极值)，就要求二阶导等于0时，所以，这个系数并不影响。而二阶导为0，就是公式(10)等于0。
二阶导为0时，是极值，既可以极大值，也可以极小值，极小值就是像素值为0了。如果一张图的非细节部分的像素全为100的话，那么非细节部分像素的二阶导也为0。




[参考链接](https://blog.csdn.net/u011754972/article/details/121783295?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-5-121783295-blog-77775029.pc_relevant_default&spm=1001.2101.3001.4242.4&utm_relevant_index=8)