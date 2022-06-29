---
title: esp32显示图片
---

# esp32在OLED上显示图片
<!-- more -->
之前写的桌面天气代码，在屏幕显示上没有图片，感觉缺少了灵魂。写桌面天气的时候参考一位大佬的代码看见他写了关于图片这方面的代码，本着学习和补全项目的意愿，补写了这部分代码。
这部分代码其实不难，主要难点还是在调试图片的显示位置和图片的取模，代码这方面感觉没任何难度。
## 一、图片取模
图片取模这里我用的PCtoLCD2002，这个软件使用也比较简单，就是资源比较难找，随便下载的又怕有病毒，这里放一个这个软件的百度云链接，需要的自取
[PCtoLCD2002](https://pan.baidu.com/s/1hiaKpFbau73P4H7J1IuaqA)，提取码：92yn
**如果链接失效可以发邮件给我或者私信，邮箱：ybl728@163.com**
我这里把我取模的设置给大家看看
![字模设置](https://i.niupic.com/images/2022/03/23/9X0W.png)
***PS：图中的数据前缀是0x不是Ox,标点符号都是英文的*** 不要问我为什么知道，这是试出来的，我也是第一次用。毕竟复制字模数据进代码就报红一大片可太难受了。
## 二、图片取模过程
使用方法：这个方法我是跟这个大佬学的
[大佬链接](https://blog.csdn.net/qq_41868901/article/details/104221495?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-4-104221495.pc_agg_new_rank&utm_term=esp32+%E5%9B%BE%E7%89%87%E5%8F%96%E6%A8%A1&spm=1000.2123.3001.4430)
跟着做就行没什么难点
## 三、天气图标获取
天气图标，知心天气有提供这里放链接，自提
[知心天气天气图标](https://seniverse.yuque.com/books/share/e52aa43f-8fe9-4ffa-860d-96c0f3cf1c49/yev2c3)
这里还有API接口返回天气代码对应的天气状况，写代码的时候需要用到。
## 天气图标显示代码
这里代码有点多，主要是字模的数据太多了，代码我就不放这里放另一篇里。
[天气图标字模头文件](https://yu-728.github.io/2022/03/23/esp32_weatherpicture_H/)
[天气图标代码](https://yu-728.github.io/2022/03/21/esp32_weatherCode/)
这里直接放实现效果：
![图标显示](https://i.niupic.com/images/2022/03/23/9X14.jpg)