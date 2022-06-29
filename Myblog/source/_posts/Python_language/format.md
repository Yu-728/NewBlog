---
title: python-format
---
# format
<!--more-->
# 前言
在学习KNN算法例子中有这一句：
```
print("result:  {}\n".format(results))
print("neighbours:  {}\n".format(neighbours))
print("distance:  {}\n".format(dist))
```
有点看不懂就去百度看了语法，记录一下自己的学习。
# format()
*args和**kwargs是python中的可变参数;
*args表示任何多个无名参数,是tuple类型;
**表示关键字参数,是dict类型;
同时使用*args和kwargs时,*args必须放在kwargs前面;
例子：
```
def foo(*args, **kwargs):
	print("args={}".format(args))
	print("kwargs={}".format(kwargs))
	for i in range(10):
		print("**"*10,end='')
if __name__ == "__main__":
	# tuple类型数据
	foo(1, 2, 3, 4)
	# dict类型数据
	foo(a=1, b=2, c=3)
	# tuple+dict类型数据
	foo(1, 2, 3, 4, a=1, b=2, c=3)
```
结果：
```
# *args为tuple类型
# **kwargs为dict类型
args=(1, 2, 3)
kwargs={}
********************
args=()
kwargs={'a': 1, 'b': 2, 'c': 3}
********************
args=(1, 2, 3)
kwargs={'a': 1, 'b': 2, 'c': 3}
********************
```

作用： 格式化字符串函数
format是Python2.6开始新增的格式化字符串函数,形式str.format();
使用形式"{}{}".format("a","b");
传统%引用变量输出,即print("%d" %a);
format参数个数不受限制.

例子：
```
a = "{} {}".format("xin", "daqi")
b = "{},{}".format("OK","See you later!")
print(type(a))
print(a)
print(type(b))
print(b)

```
结果：
```
# format输出格式为字符串,默认按照参数先后位置输出
<class 'str'>
xin daqi
<class 'str'>
OK,See you later!
```
参考：
[https://blog.csdn.net/Xin_101/article/details/83784322](https://blog.csdn.net/Xin_101/article/details/83784322)