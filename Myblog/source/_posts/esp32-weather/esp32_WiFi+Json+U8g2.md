---
title: esp32连接API获取信息
---
## U8g2、ArduinoJson、WiFi、http的实现
<!-- more -->
#### 学习了U8g2、ArduinoJson、WiFi、http这些知识，接下来对其进行实际应用

这是一个不起眼的项目，我在学习的时候遇到的作者本人也是没事做着玩玩，然后就变成了我的学习项目了，我觉得我从中学到挺多的,U8g2库的使用我就是在这里面学到的，代码里对u8g2的注释较多，毕竟第一次学习

废话不多说贴代码：
```
#include <Arduino.h>
#include "WiFi.h"
#include "ArduinoJson.h"
#include "HTTPClient.h"
#include <U8g2lib.h>

#ifdef U8X8_HAVE_HW_SPI
#include <SPI.h>
#endif
#ifdef U8X8_HAVE_HW_I2C
#include <Wire.h>
#endif

//OLED硬件初始化
U8G2_SSD1306_128X64_NONAME_F_SW_I2C u8g2(U8G2_R0, /* clock=*/ SCL, /* data=*/ SDA, /* reset=*/ U8X8_PIN_NONE);   // All Boards without Reset of the Display

// WIFI连接信息
const char* ssid = "HONOR V20"; //wifi nane
const char* password = "yubaolin"; // wifi password

void WiFi_Connect();
void get_Hitokoto();
void OLEDDisplay();

//一言
typedef struct {
  char hitokoto[1024];//一言正文。编码方式 unicode。使用 utf-8。
  char from[60];//一言的出处
} HitokotoData_t;
HitokotoData_t Hitokoto;

//WiFi connect function
void WiFi_Connect(){

    WiFi.begin(ssid, password);
    
    while (WiFi.status() != WL_CONNECTED){  //Connection state judgment
        delay(1000);
        Serial.println("Connecting to WiFi...");

        
        u8g2.clearBuffer();
        u8g2.setFont(u8g2_font_wqy12_t_gb2312);
        u8g2.setCursor(0,16);
        u8g2.print("wifi连接中");
        u8g2.sendBuffer();

    }

    Serial.println("Connected to the WiFi network");

    u8g2.clearBuffer();
    u8g2.setFont(u8g2_font_wqy12_t_gb2312);
    u8g2.setCursor(0,16);
    u8g2.print("wifi成功");
    u8g2.sendBuffer();
}

void get_Hitokoto(void)
{
  HTTPClient http;
  http.begin("https://v1.hitokoto.cn/");//Specify the URL
  int httpCode = http.GET();            //Make the request
  if (httpCode > 0) { //Check for the returning code

    String payload = http.getString();
    Serial.println(httpCode);
    Serial.println(payload);

    StaticJsonDocument<1024> doc;

    DeserializationError error = deserializeJson(doc, payload);

    if (error) {
      Serial.println("JSON parsing failed!");
    } else {
      if (strlen(doc["hitokoto"]) > sizeof(Hitokoto.hitokoto)) {
      
        http.end();
        return;
      }
      strcpy(Hitokoto.hitokoto, doc["hitokoto"]);
      strcpy(Hitokoto.from, doc["from"]);
    }
  }
  else {
    Serial.println("Error on HTTP request");
  }
  http.end(); //Free the resources
  
  //Serial.printf(sizeof(Hitokoto.hitokoto*sizeof(char)));
  Serial.println(Hitokoto.hitokoto);
  Serial.printf("出处：");
  Serial.println(Hitokoto.from);
}

void OLEDDisplay(){
  /**
  * 初始化U8g2库
  * @Note 关联方法 initDisplay clearDisplay setPowerSave
  */
  //u8g2.begin();
  /**
  * 清除内存中数据缓冲区
  */
  u8g2.clearBuffer();
  /**
  * 开启Arduino平台下支持输出UTF8字符集
  */ 
  //u8g2.enableUTF8Print(); 
  /**
  * 设置字体集（字体集用于字符串绘制方法或者glyph绘制方法）
  * @param font 具体的字体集
  * @Note 关联方法  drawUTF8 drawStr drawGlyph print
  */
  u8g2.setFont(u8g2_font_wqy12_t_gb2312); 
  /** 
   * 设置字符串绘制方向
   * 0：左往右，旋转0度 1:上到下，旋转90度 2：右到左，旋转180度 3：下到上，旋转270度
   */
  u8g2.setFontDirection(0); 
  /**
  * 设置绘制光标位置(x,y)
  * x:为绘制横向起点，y；为纵向起点
  * 绘制是从光标位置向左上角方向
  * @Note 关联方法 print
  */
  u8g2.setCursor(0, 16);
  //void DisplayInit();
  /**
  * 绘制内容
  * @Note 关联方法  setFont setCursor enableUTF8Print
  */
  u8g2.println(Hitokoto.hitokoto);
  u8g2.setCursor(0,32);
  u8g2.println("出处：");
  u8g2.print(Hitokoto.from);
  u8g2.sendBuffer();
  delay(1000);

}

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
  delay(3000);
  u8g2.begin();
  u8g2.enableUTF8Print();

  WiFi_Connect();
  //get_Hitokoto();
  //OLEDDisplay();
  
}



void loop() {
  // put your main code here, to run repeatedly:
  get_Hitokoto();
  OLEDDisplay();
  delay(60000);
}
```
### 最终呈现
![一言实现](https://i.niupic.com/images/2022/03/18/9WHU.jpg)

## 参考文档
[https://blog.csdn.net/u014091490/article/details/104803307?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-104803307.pc_agg_new_rank&utm_term=esp32%E8%AF%BB%E5%8F%96json&spm=1000.2123.3001.4430](https://blog.csdn.net/u014091490/article/details/104803307?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-104803307.pc_agg_new_rank&utm_term=esp32%E8%AF%BB%E5%8F%96json&spm=1000.2123.3001.4430)