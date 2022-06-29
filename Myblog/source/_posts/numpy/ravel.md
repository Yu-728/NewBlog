---
title: numpy-ravle()
---
# numpy-ravle()
<!--more-->
ravel()将多维数据展平为一维数据，可以选择不同的数据索引方式（见文档参数四个可选值）
使用：
例子：
```
>>> import numpy as np
>>> a = np.array([[1,2,3],[4,5,6]])
>>> a
array([[1, 2, 3],
       [4, 5, 6]])
>>> a.ravel()
array([1, 2, 3, 4, 5, 6])

```

参考
[https://blog.csdn.net/qq_34159047/article/details/111300468?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-1-111300468-blog-84305399.pc_relevant_baidufeatures_v7&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-1-111300468-blog-84305399.pc_relevant_baidufeatures_v7&utm_relevant_index=1](https://blog.csdn.net/qq_34159047/article/details/111300468?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-1-111300468-blog-84305399.pc_relevant_baidufeatures_v7&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-1-111300468-blog-84305399.pc_relevant_baidufeatures_v7&utm_relevant_index=1)