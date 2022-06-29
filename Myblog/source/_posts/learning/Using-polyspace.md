---
title: matlab的polyspace使用
---
# polyspace使用记录
<!--more-->
## polyspace介绍
Polyspace是Mathwork旗下的代码静态检查工具。olyspace会检查源代码，以确定可能在哪里发生潜在的运行时错误，例如算术溢出，缓冲区溢出，被零除和其他错误。软件开发人员和质量保证经理使用此信息来识别代码的哪些部分有故障或被证明是可靠的。该代码的其他部分已标记为未经验证的检查，应接受个人审查。诸如MISRA C之类的代码标准或准则试图解决代码质量，可移植性和可靠性。该产品检查C和C ++源代码是否符合这些编码标准中的一部分规则。

Polyspace的Bug Finder工具通过对源代码执行静态程序分析来识别软件错误。它会发现缺陷，例如数值计算，编程，存储和其他错误。它还会生成软件指标，例如源文件的注释密度，循环复杂性，行数，参数，调用级别等，并在软件中标识出运行时错误

## 使用方法
### 一、新建工程
![图1](https://img-blog.csdnimg.cn/20200501195805400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1OTIwMDkx,size_16,color_FFFFFF,t_70)
![picture1](https://img-blog.csdnimg.cn/20191202204042955.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODQ1MTgwMA==,size_16,color_FFFFFF,t_70)
在输入工程信息的时候，可以勾选**Use temple**使用polyspace中带有的测试模板，随后根据自身需要选择相应的模板。

### 二、添加测试文件
![图2](https://img-blog.csdnimg.cn/20200501195936273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1OTIwMDkx,size_16,color_FFFFFF,t_70)
![picture2](https://img-blog.csdnimg.cn/20200501200221958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1OTIwMDkx,size_16,color_FFFFFF,t_70)

在添加文件时，是先添加源文件再添加头文件。记得勾选 **<font color=#FF0000>Add Source Floders</font>** 在点击Add Source Floders。
**PS：如果使用了IDE自带的库文件也要记得添加进去**

### 三、检查设置
![1](https://img-blog.csdnimg.cn/20200501200311991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1OTIwMDkx,size_16,color_FFFFFF,t_70)
如果使用了模板就不用修改此图片内容，否则根据自己代码要求进行设置。

![2](https://img-blog.csdnimg.cn/20200221212146874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTMyODg5MjU=,size_16,color_FFFFFF,t_70)
### 四、运行和报告
![3](https://img-blog.csdnimg.cn/2020022121293912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTMyODg5MjU=,size_16,color_FFFFFF,t_70)
### 五、报告分析
![4](https://img-blog.csdnimg.cn/20200221213659477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTMyODg5MjU=,size_16,color_FFFFFF,t_70)

经Polyspace分析后的代码结果以不同颜色表:

绿色： 代表为安全代码，无需花过多精力审查；

红色： 代码问题代码，需要立刻解决；

灰色： 代表不可达代码，需要审查是设计错误还是有意为之；

橙色： 代表有风险代码，需要重点审查。

另外还可以设定编码规范(如MISRA)和自定义代码风格，违反之处以紫色显示；同时可以看到代码变量随控制流的数据范围变化情况，快速查找和定位问题原因。


# 参考文章
[https://blog.csdn.net/qq_25920091/article/details/105881534](https://blog.csdn.net/qq_25920091/article/details/105881534)
[https://blog.csdn.net/weixin_38451800/article/details/103352448](https://blog.csdn.net/weixin_38451800/article/details/103352448)

[polyspace教程](https://blog.csdn.net/u013288925/article/details/104433825?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1.pc_relevant_antiscanv2&spm=1001.2101.3001.4242.2&utm_relevant_index=4)