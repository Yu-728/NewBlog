---
title: esp32WiFi功能实现
---

### WiFi功能没啥好说的直接贴代码，自己看看就懂了，很简单
<!-- more -->
```
#include <Arduino.h>
#include "WiFi.h

// ----WIFI连接信息-----//
const char* ssid = "写你WIFI的名字"; //wifi nane
const char* password = "Wifi密码"; // wifi password

//这里我把WIFi这个功能封装成一个函数了
//WiFi connect function
void WiFi_Connect(){

    WiFi.begin(ssid, password);
    
    while (WiFi.status() != WL_CONNECTED){  //Connection state judgment
        delay(1000);
        Serial.println("Connecting to WiFi...");
    }
    Serial.println("Connected to the WiFi network");
}

void setup(){
    Serial.begin(115200); //设置波特率，在platformio.ini文件里加上 monitor_speed = 115200
    WiFi_Connect();
    delay(500);
}

void loop(){

}
"
```

#### 然后我们就可以在手机看到板子了（我这里是手机开热点）
![手机查看WIFI连接设备](https://i.niupic.com/images/2022/03/18/9WHT.jpg)