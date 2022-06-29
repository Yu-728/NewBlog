---
title: CNN中channel的理解
---
#  Channel
channel 翻译 通道
最开始我的理解是图像的通道数，例如彩色图像的channel=3，灰度图像channel=1
在学习CNN的时候看网上的博文说一般的channel = 32 o r64
我这就很不理解我们所接触的不就是灰度图像和彩色图像吗？不就是channel要么等于1要么等于3吗？
随后我就在网上查阅了很多资料看了很多文章，终于理解了为什么他们说channel一般等于32 or 64
**或许说的不对，也当做一个学习记录，后续发现错了我会修改**
我们在对灰度图像处理时，**卷积核数量**默认为1。此时图像size为（6 ，6， 1）,卷积核为（3， 3， 1）,进行卷积后得到的图像size（4, 4, 1）channel = 1与卷积核的channel一致
在处理彩色图像时，卷积核数量由我们自行设置，假设我们设置为1（ps:卷积核数量为1）
此时图像size(6, 6, 3),卷积核size（3, 3 ，3），进行卷积后得到的图像size（4, 4, 1）channel = 1与 卷积核的channel不一致！
假设我们把卷积核数量设为3
此时图像size(6, 6, 3),卷积核size（3, 3 ，3），进行卷积后得到的图像size（4, 4, 3）channel = 3与 卷积核的channel一致！
***这就说明了卷积运算后图像的channel就是进行卷积核数量。***
如果我们把卷积核数量弄为64，那么卷积后图像的channel不就等于64了吗。
所以我感觉CNN的channel并不是我最开始理解的channel，我觉得把它理解为对应卷积运算核个数更合适。
在CV里有很多东西不是它原本的意义，就像卷积不是真正的卷积（没有物理意义），只是表示卷积这一运算方式而已。

那么问题来了，对卷积后的图像在卷积此时卷积核的channel应该等于多少？
假设对卷积得到的图像为size（4, 4, 2）再进行卷积这个时候卷积核的channel不就得等于2就是卷积核size（3， 3，2）

这里是一些我学习的链接：
[学习参考1](https://blog.csdn.net/Xiao_Bai_Ke/article/details/98998767?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2-98998767-blog-106979250.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2-98998767-blog-106979250.nonecase&utm_relevant_index=3)
[学习参考2](https://www.zhihu.com/question/47158818)
[学习参考3](https://www.zhihu.com/zvideo/1294313119628353536)
[学习参考4](https://morvanzhou.github.io/tutorials/machine-learning/torch/4-01-CNN/)
