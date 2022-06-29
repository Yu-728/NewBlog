---
title: Markdown语法
---
# 一、标题：
<!-- more -->
# 这是一级标题
## 二级标题
### 三级标题
#### 依次类推4、5、6...

# 二、字体
## 加粗
要加粗的文字左右分别用<font color=#FF0000>两个*号</font>包起来，如下
**加粗**
## 斜体
要斜体的文字左右分别用<font color=#FF0000>一个*号</font>包起来，如下
*斜体*
## 斜体加粗
要斜体加粗的文字左右分别用<font color=#FF0000>三个*号</font>包起来，如下
***斜体加粗***
## 删除线
要删除的文字左右分别用<font color=#FF0000>两个~~号</font>包起来，如下
~~删除线~~

# 三、引用
在引用的文字前加>即可。引用也可以嵌套，如加两个>>三个>>>n个，如下
>这是引用内容
>>这是也是引用内容
>>>这是也是引用内容
>>>>这是也是引用内容
>>>>>......

# 四、分割线
需要三个或三个以上的-或者*，如下
**********************
***
--------------------
---

# 五、图片
语法： 
![图片alt](图片地址 ''图片title'')

图片alt就是显示在图片下面的文字，相当于对图片内容的解释。
图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加，如下：

![汽车](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fup.enterdesk.com%2Fedpic%2F55%2Fbd%2F7b%2F55bd7b04e3de581cd67576c0529e13e8.jpg&refer=http%3A%2F%2Fup.enterdesk.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1649313531&t=511169c034c9205ffbcd8ac4167a2c15 "奥迪")
**注：** 如果要使用本地图片，建议将图片上传Markdown图床。详细使用方法见大佬写的文章[markdown图床](https://www.jianshu.com/p/ea1eb11db63f)。

# 六、超链接
语法： 
[超链接名](超链接地址 "超链接title")
title可加可不加
示例： 
[简书](http://jianshu.com)
[百度](http://baidu.com)

# 七、列表
## 无序列表
语法： 
无序列表用 - + * 任何一种都可以
示例：
- 列表（-）
+ 列表（+）
* 列表（*）
## 有序列表
语法： 
数字加点，1. 2. 3. ......
示例：
1. 内容
2. 内容
3. 内容
## 列表嵌套
上一级和下一级之间敲**三个空格**即可
- 一级列表
   1. 二级列表
   2. 二级列表
      3. 三级

# 八、表格
语法： 
```
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容
```

第二行分割表头和内容。
-有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右
注：原生的语法两边都要用 | 包起来。此处省略
示例： 
姓名|技能|排行
--|:--:|--: 
刘备|哭|大哥
关羽|打|二哥
张飞|骂|三弟

# 九、代码
语法： 
单行代码：代码之间分别用一个**反引号**包起来 
`代码内容` 
代码块：代码之间分别用三个反引号包起来，且两边的反引号单独占一行
```
  代码...
  代码...
  代码...
```
示例：

`printf("hello, world!");`

```
int main(){
    return 0;
}
```

# 十、流程图

```flow
st=>start: 开始框
op=>operation: 处理框
cond=>condition: 判断框(是或否?)
sub1=>subroutine: 子流程
io=>inputoutput: 输入输出框
e=>end: 结束框
st->op->cond
cond(yes)->io->e
cond(no)->sub1(right)->op
``` 
>更多流程图详见[流程图格式](https://www.runoob.com/markdown/md-advance.html)

# 十一、字体格式和字体颜色
```
<font  更改语法>   你的内容   </font>

更改语法有: 

color=#0099ff   更改字体颜色
face="黑体"   更改字体
size= 7     更改字体大小


<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=7 face="黑体">color=#0099ff size=72 face="黑体"</font>
<font color=#00ffff size=72>color=#00ffff</font>
<font color=gray size=72>color=gray</font>
```

附上更多颜色的rgb代码：
颜色名 十六进制颜色值 颜色
AliceBlue   #F0F8FF rgb(240, 248, 255)
AntiqueWhite    #FAEBD7 rgb(250, 235, 215)
Aqua    #00FFFF rgb(0, 255, 255)
Aquamarine  #7FFFD4 rgb(127, 255, 212)
Azure   #F0FFFF rgb(240, 255, 255)
Beige   #F5F5DC rgb(245, 245, 220)
Bisque  #FFE4C4 rgb(255, 228, 196)
Black   #000000 rgb(0, 0, 0)
BlanchedAlmond  #FFEBCD rgb(255, 235, 205)
Blue    #0000FF rgb(0, 0, 255)
BlueViolet  #8A2BE2 rgb(138, 43, 226)
Brown   #A52A2A rgb(165, 42, 42)
BurlyWood   #DEB887 rgb(222, 184, 135)
CadetBlue   #5F9EA0 rgb(95, 158, 160)
Chartreuse  #7FFF00 rgb(127, 255, 0)
Chocolate   #D2691E rgb(210, 105, 30)
Coral   #FF7F50 rgb(255, 127, 80)
CornflowerBlue  #6495ED rgb(100, 149, 237)
Cornsilk    #FFF8DC rgb(255, 248, 220)
Crimson #DC143C rgb(220, 20, 60)
Cyan    #00FFFF rgb(0, 255, 255)
DarkBlue    #00008B rgb(0, 0, 139)
DarkCyan    #008B8B rgb(0, 139, 139)
DarkGoldenRod   #B8860B rgb(184, 134, 11)
DarkGray    #A9A9A9 rgb(169, 169, 169)
DarkGreen   #006400 rgb(0, 100, 0)
DarkKhaki   #BDB76B rgb(189, 183, 107)
DarkMagenta #8B008B rgb(139, 0, 139)
DarkOliveGreen  #556B2F rgb(85, 107, 47)
Darkorange  #FF8C00 rgb(255, 140, 0)
DarkOrchid  #9932CC rgb(153, 50, 204)
DarkRed #8B0000 rgb(139, 0, 0)
DarkSalmon  #E9967A rgb(233, 150, 122)
DarkSeaGreen    #8FBC8F rgb(143, 188, 143)
DarkSlateBlue   #483D8B rgb(72, 61, 139)
DarkSlateGray   #2F4F4F rgb(47, 79, 79)
DarkTurquoise   #00CED1 rgb(0, 206, 209)
DarkViolet  #9400D3 rgb(148, 0, 211)
DeepPink    #FF1493 rgb(255, 20, 147)
DeepSkyBlue #00BFFF rgb(0, 191, 255)
DimGray #696969 rgb(105, 105, 105)
DodgerBlue  #1E90FF rgb(30, 144, 255)
Feldspar    #D19275 rgb(209, 146, 117)
FireBrick   #B22222 rgb(178, 34, 34)
FloralWhite #FFFAF0 rgb(255, 250, 240)
ForestGreen #228B22 rgb(34, 139, 34)
Fuchsia #FF00FF rgb(255, 0, 255)
Gainsboro   #DCDCDC rgb(220, 220, 220)
GhostWhite  #F8F8FF rgb(248, 248, 255)
Gold    #FFD700 rgb(255, 215, 0)
GoldenRod   #DAA520 rgb(218, 165, 32)
Gray    #808080 rgb(128, 128, 128)
Green   #008000 rgb(0, 128, 0)
GreenYellow #ADFF2F rgb(173, 255, 47)
HoneyDew    #F0FFF0 rgb(240, 255, 240)
HotPink #FF69B4 rgb(255, 105, 180)
IndianRed   #CD5C5C rgb(205, 92, 92)
Indigo  #4B0082 rgb(75, 0, 130)
Ivory   #FFFFF0 rgb(255, 255, 240)
Khaki   #F0E68C rgb(240, 230, 140)
Lavender    #E6E6FA rgb(230, 230, 250)
LavenderBlush   #FFF0F5 rgb(255, 240, 245)
LawnGreen   #7CFC00 rgb(124, 252, 0)
LemonChiffon    #FFFACD rgb(255, 250, 205)
LightBlue   #ADD8E6 rgb(173, 216, 230)
LightCoral  #F08080 rgb(240, 128, 128)
LightCyan   #E0FFFF rgb(224, 255, 255)
LightGoldenRodYellow    #FAFAD2 rgb(250, 250, 210)
LightGrey   #D3D3D3 rgb(211, 211, 211)
LightGreen  #90EE90 rgb(144, 238, 144)
LightPink   #FFB6C1 rgb(255, 182, 193)
LightSalmon #FFA07A rgb(255, 160, 122)
LightSeaGreen   #20B2AA rgb(32, 178, 170)
LightSkyBlue    #87CEFA rgb(135, 206, 250)
LightSlateBlue  #8470FF rgb(132, 112, 255)
LightSlateGray  #778899 rgb(119, 136, 153)
LightSteelBlue  #B0C4DE rgb(176, 196, 222)
LightYellow #FFFFE0 rgb(255, 255, 224)
Lime    #00FF00 rgb(0, 255, 0)
LimeGreen   #32CD32 rgb(50, 205, 50)
Linen   #FAF0E6 rgb(250, 240, 230)
Magenta #FF00FF rgb(255, 0, 255)
Maroon  #800000 rgb(128, 0, 0)
MediumAquaMarine    #66CDAA rgb(102, 205, 170)
MediumBlue  #0000CD rgb(0, 0, 205)
MediumOrchid    #BA55D3 rgb(186, 85, 211)
MediumPurple    #9370D8 rgb(147, 112, 216)
MediumSeaGreen  #3CB371 rgb(60, 179, 113)
MediumSlateBlue #7B68EE rgb(123, 104, 238)
MediumSpringGreen   #00FA9A rgb(0, 250, 154)
MediumTurquoise #48D1CC rgb(72, 209, 204)
MediumVioletRed #C71585 rgb(199, 21, 133)
MidnightBlue    #191970 rgb(25, 25, 112)
MintCream   #F5FFFA rgb(245, 255, 250)
MistyRose   #FFE4E1 rgb(255, 228, 225)
Moccasin    #FFE4B5 rgb(255, 228, 181)
NavajoWhite #FFDEAD rgb(255, 222, 173)
Navy    #000080 rgb(0, 0, 128)
OldLace #FDF5E6 rgb(253, 245, 230)
Olive   #808000 rgb(128, 128, 0)
OliveDrab   #6B8E23 rgb(107, 142, 35)
Orange  #FFA500 rgb(255, 165, 0)
OrangeRed   #FF4500 rgb(255, 69, 0)
Orchid  #DA70D6 rgb(218, 112, 214)
PaleGoldenRod   #EEE8AA rgb(238, 232, 170)
PaleGreen   #98FB98 rgb(152, 251, 152)
PaleTurquoise   #AFEEEE rgb(175, 238, 238)
PaleVioletRed   #D87093 rgb(216, 112, 147)
PapayaWhip  #FFEFD5 rgb(255, 239, 213)
PeachPuff   #FFDAB9 rgb(255, 218, 185)
Peru    #CD853F rgb(205, 133, 63)
Pink    #FFC0CB rgb(255, 192, 203)
Plum    #DDA0DD rgb(221, 160, 221)
PowderBlue  #B0E0E6 rgb(176, 224, 230)
Purple  #800080 rgb(128, 0, 128)
Red #FF0000 rgb(255, 0, 0)
RosyBrown   #BC8F8F rgb(188, 143, 143)
RoyalBlue   #4169E1 rgb(65, 105, 225)
SaddleBrown #8B4513 rgb(139, 69, 19)
Salmon  #FA8072 rgb(250, 128, 114)
SandyBrown  #F4A460 rgb(244, 164, 96)
SeaGreen    #2E8B57 rgb(46, 139, 87)
SeaShell    #FFF5EE rgb(255, 245, 238)
Sienna  #A0522D rgb(160, 82, 45)
Silver  #C0C0C0 rgb(192, 192, 192)
SkyBlue #87CEEB rgb(135, 206, 235)
SlateBlue   #6A5ACD rgb(106, 90, 205)
SlateGray   #708090 rgb(112, 128, 144)
Snow    #FFFAFA rgb(255, 250, 250)
SpringGreen #00FF7F rgb(0, 255, 127)
SteelBlue   #4682B4 rgb(70, 130, 180)
Tan #D2B48C rgb(210, 180, 140)
Teal    #008080 rgb(0, 128, 128)
Thistle #D8BFD8 rgb(216, 191, 216)
Tomato  #FF6347 rgb(255, 99, 71)
Turquoise   #40E0D0 rgb(64, 224, 208)
Violet  #EE82EE rgb(238, 130, 238)
VioletRed   #D02090 rgb(208, 32, 144)
Wheat   #F5DEB3 rgb(245, 222, 179)
White   #FFFFFF rgb(255, 255, 255)
WhiteSmoke  #F5F5F5 rgb(245, 245, 245)
Yellow  #FFFF00 rgb(255, 255, 0)
YellowGreen #9ACD32 rgb(154, 205, 50)

# 参考文档链接
### 更具体的使用方法可以去参考文档看
>https://www.jianshu.com/p/191d1e21f7ed
>https://blog.csdn.net/u011995719/article/details/77232626
>https://www.jianshu.com/p/280c6a6f2594
>https://www.runoob.com/markdown/md-advance.html

后续有缘持续更新...
