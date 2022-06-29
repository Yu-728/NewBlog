---
title: ESP32通过WIFI获取网络时间
---

### 一、这里首先要知道esp32的wifi如何连接，才能获取到网络服务器的时间
<!-- more --> 
***不知道wifi怎么连接的这里有相关链接***——[esp32wifi连接网络](https://yu-728.github.io/2022/03/18/esp32_WiFi/)

### 二、esp32中与时间相关的函数了解
1. 使用STA或ETH模式连上互联网
2. 使用下面函数从网络时间服务器上获取并设置时间：
```
void configTime(long gmtOffset_sec, int daylightOffset_sec, const char* server1, const char* server2 = nullptr, const char* server3 = nullptr)
```
网络时间服务器最常用的主机名是 pool.ntp.org ；
**这时就有人说了，我不喜欢这个服务器还有其他的吗？**
以下是一些常用的网络时间服务器供大家使用：

Name|Address
---          |:--:
Worldwide    |pool.ntp.org
Asia         |asia.pool.ntp.org
Europe       |europe.pool.ntp.org
North America|north-america.pool.ntp.org
South America|south-america.pool.ntp.org
Oceania|oceania.pool.ntp.org

通过网络时间服务器获得的时间是世界协调时间（UTC）/格林尼治时间（GMT），不同地区的时间可以通过时区换算
***gmtOffset_sec*** 参数就是用来修正时区的，比如对于我们东八区（UTC/GMT+08:00）来说该参数就需要填写 8 * 3600 
如果使用夏令时 daylightOffset_sec 就填写3600，否则就填写0；
3. 设置完成后就可以使用下面函数读取当前时间了：
bool getLocalTime(struct tm * info, uint32_t ms = 5000)
ms 为该操作超时时间，超时则返回false；
info 是一个 ***struct tm*** 结构体对象，用于接收当前时间；
#### Struct tm 结构体解析：

```
struct tm {
int tm_sec; // 秒，取值0~59；
int tm_min; // 分，取值0~59；
int tm_hour; // 时，取值0~23；
int tm_mday; // 月中的日期，取值1~31；
int tm_mon; // 月，取值0~11；
int tm_year; // 年，其值等于实际年份减去1900；
int tm_wday; // 星期，取值0~6，0为周日，1为周一，依此类推；
int tm_yday; // 年中的日期，取值0~365，0代表1月1日，1代表1月2日，依此类推；
int tm_isdst; // 夏令时标识符，实行夏令时的时候，tm_isdst为正；不实行夏令时的进候，tm_isdst为0；不了解情况时，tm_isdst()为负
};
```

***结构体格式化输出规则***

>%a 星期几的简写
%A 星期几的全称
%b 月分的简写
%B 月份的全称
%c 标准的日期的时间串
%C 年份的后两位数字
%d 十进制表示的每月的第几天
%D 月/天/年
%e 在两字符域中，十进制表示的每月的第几天
%F 年-月-日
%g 年份的后两位数字，使用基于周的年
%G 年分，使用基于周的年
%h 简写的月份名
%H 24小时制的小时
%I 12小时制的小时
%j 十进制表示的每年的第几天
%m 十进制表示的月份
%M 十时制表示的分钟数
%p 本地的AM或PM的等价显示
%r 12小时的时间
%R 显示小时和分钟：hh:mm
%S 十进制的秒数
%t 水平制表符
%T 显示时分秒：hh:mm:ss
%u 每周的第几天，星期一为第一天 （值从0到6，星期一为0）
%U 第年的第几周，把星期日做为第一天（值从0到53）
%V 每年的第几周，使用基于周的年
%w 十进制表示的星期几（值从0到6，星期天为0）
%W 每年的第几周，把星期一做为第一天（值从0到53）
%x 标准的日期串
%X 标准的时间串
%y 不带世纪的十进制年份（值从0到99）
%Y 带世纪部分的十进制年份
%z，%Z 时区名称，如果不能得到时区名称则返回空字符


## 示例代码：
```
#include <WiFi.h>

const char* ssid = "HONOR V20"; //wifi nane
const char* password = "yubaolin"; // wifi password

//-----------网络时间获取-----------//
const char *ntpServer = "pool.ntp.org"; //网络时间服务器
//时区设置函数，东八区（UTC/GMT+8:00）写成8*3600
const long gmtOffset_sec = 8 * 3600;    
const int daylightOffset_sec = 0;   //夏令时填写3600，否则填0

void printLocalTime()
{
    struct tm timeinfo;
    if (!getLocalTime(&timeinfo))
    {
        Serial.println("Failed to obtain time");
        return;
    }
    Serial.println(&timeinfo, "%F %T %A"); // 格式化输出
}

void setup()
{
    Serial.begin(115200);
    Serial.println();

    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED)
    {
        delay(500);
        Serial.print(".");
    }
    Serial.println("WiFi connected!");

    // 从网络时间服务器上获取并设置时间
    // 获取成功后芯片会使用RTC时钟保持时间的更新
    configTime(gmtOffset_sec, daylightOffset_sec, ntpServer);
    printLocalTime();

    // WiFi.disconnect(true);
    // WiFi.mode(WIFI_OFF);
    // Serial.println("WiFi disconnected!");
}

void loop()
{
    delay(1000);
    printLocalTime();
}
```
### 结果
![wifi获取时间](https://i.niupic.com/images/2022/03/21/9WTc.png)


## 参考文档
[https://blog.csdn.net/Naisu_kun/article/details/115627629?spm=1001.2101.3001.6650.17&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17.pc_relevant_default&utm_relevant_index=19](https://blog.csdn.net/Naisu_kun/article/details/115627629?spm=1001.2101.3001.6650.17&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17.pc_relevant_default&utm_relevant_index=19)