---
title: with as 语法
---
# with as 语法
<!--more-->
执行循序
with 执行1 as 变量（执行3）：
    执行2


紧跟with后面的语句被求值后，返回对象的 _enter_() 方法被调用，这个方法的返回值将被赋值给as后面的变量。
当with后面的代码块全部被执行完之后，将调用前面返回对象的 _exit_()方法。
例子：
```
#!/usr/bin/env python
# with_example01.py
class Sample:
    def __enter__(self):
        print "In __enter__()"
        return "Foo"
    def __exit__(self, type, value, trace):
        print "In __exit__()"
def get_sample():
    return Sample()
with get_sample() as sample:
    print "sample:", sample
```
结果:
```
bash-3.2$ ./with_example01.py
In __enter__()
sample: Foo
In __exit__()
```

详见大佬链接
[https://blog.csdn.net/bitcarmanlee/article/details/52745676](https://blog.csdn.net/bitcarmanlee/article/details/52745676)
