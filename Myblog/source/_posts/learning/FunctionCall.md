---
title: C中函数调用
---
## .c文件之间的函数调用方法
<!-- more -->
### 一、通过.h文件调用（推荐使用）
在.c（源文件）文件中对函数进行定义，然后再.h文件中进行声明。
当另一个.c文件调用之前定义好的函数时直接导入声明该函数的.h文件即可。
举个例子：
**这是A.h**
```
#ifndef __A_H__
#define __A_H__

	extern int var_A;
	int func_A();

#endif
```
**这是A.c**
```
#include "A.h"

int var_A = 0;
int func_A()
{
	var_A += 1;
	return var_A;
}

```
假设有一个**main.c**想调用int func_A()函数
**这是main.c**
```
#include "A.h"

//此处可以不写，但是推荐写上便于后期代码维护，不然所调用函数都得去.h文件看
extern int func_A(); 

int main()
{
	int a = 0;
	a = func_A() + 1;
	printf("a = %d", a);
	return 0;
	
}
```
### 二、直接extern声明调用（两个.c需要在同一文件夹下）
举个例子
**这是一个function.c**
```
int a = 0;
int function(){
	a +=  1；
	return a;
}
```
***这是主函数main.c**
```
#include <stdio.h>
extern int function();	//这里直接声明调用
int main(){
	int b = 0;
	b = function() + 1;
	printf("b = %d", b);
	return 0;
}
```
### 三、.h定义函数 .c调用.h(这样用可能会导致链接重复定义)
重复定义例子：
A.h定义了function()
B.h调用A.h, B.c调用B.h使用function()定义了一个func_B()
C.h也调用了A.h, C.c调用C.h使用function()定义了一个func_C()
main.c 调用B.h和C.h然后使用func_B()和func_C()
这时候编译的时候就会报错，重复定义了function()

举个在.h中定义函数的例子
**这是要调用的函数，在.h中定义**
```
#ifndef __func_H__
#define __func_H__

	int function(int a, int b){
		retrun a + b;
	}

#endif
```
**这是主函数**
```
#include <stdio.h>
#include "func.h"

int main(){
	int c = 11;
	int d = 11;
	int temp = 0;
	temp = function(c, d);
	return temp;
}
```
## 参考链接
[https://blog.csdn.net/jingzi123456789/article/details/51286119](https://blog.csdn.net/jingzi123456789/article/details/51286119)
[https://blog.csdn.net/jingzi123456789/article/details/51286146](https://blog.csdn.net/jingzi123456789/article/details/51286146)
[https://blog.csdn.net/jingzi123456789/article/details/51287139](https://blog.csdn.net/jingzi123456789/article/details/51287139)