---
title: esp32桌面天气代码
---
# 获取天气API说明
<!-- more -->
***本次获取天气的API网站为知心天气***
##### 一、注册知心天气
在注册完成后可在控制台查看你的私钥如下：
![知心天气私钥](https://i.niupic.com/images/2022/03/21/9WTg.png)
##### 二、知心天气的API文档
![知心天气API](https://i.niupic.com/images/2022/03/21/9WTk.png)
红圈就是代码中的API链接，根据需求换成你自己想要的东西
key= 你知心天气的私钥
location= 获取城市的拼音
language= 语言，默认是简体中文
unit= 温度单位℃或者℉ 

# esp32桌面天气实现代码如下：

```
#include <Arduino.h>
#include "WiFi.h"
#include "ArduinoJson.h"
#include "HTTPClient.h"
#include "weatherpicture.h"
#include <U8g2lib.h>

#ifdef U8X8_HAVE_HW_SPI
#include <SPI.h>
#endif
#ifdef U8X8_HAVE_HW_I2C

#include <Wire.h>
#endif

//OLED硬件初始化
U8G2_SSD1306_128X64_NONAME_F_SW_I2C u8g2(U8G2_R0, /* clock=*/ SCL, /* data=*/ SDA, /* reset=*/ U8X8_PIN_NONE);   // All Boards without Reset of the Display

// ------------------------WIFI连接信息---------------------------------//
const char* ssid = "HONOR V20"; //wifi nane
const char* password = "yubaolin"; // wifi password

//-------------------------网络时间获取----------------------------------//
const char *ntpServer = "pool.ntp.org"; //网络时间服务器
const long gmtOffset_sec = 8 * 3600; //时区设置函数，东八区（UTC/GMT+8:00）写成8*3600   
const int daylightOffset_sec = 0;   //夏令时填写3600，否则填0

//==========================函数声明====================================//

void WiFi_Connect();
void ParserJson();
void OLEDDisplay(String cityName, String weather, String temperature, int code_int);
void printLocalTime();


//WiFi connect function
void WiFi_Connect(){

    WiFi.begin(ssid, password);
                    //Connection state judgment
    while(WiFi.status() != WL_CONNECTED){  
        delay(1000);
        Serial.println("Connecting to WiFi...");

        /**********屏幕显示wifi连接状态*************/
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

//获取Json报文并解析
void ParserJson(){

  HTTPClient http;
  //                                                            你的私钥                      查询地址       语言   温度单位
  http.begin("https://api.seniverse.com/v3/weather/now.json?key=SvbJTBaB9LvBrsItI&location=liuzhou&language=zh-Hans&unit=c");//Specify the URL
  int httpCode = http.GET();            // Make the request
  if (httpCode > 0) { // Check for the returning code

    String payload = http.getString();
    Serial.println(httpCode);
    Serial.println(payload);

    StaticJsonDocument<1024> doc; 

    DeserializationError error = deserializeJson(doc, payload);

    if (error) {
      Serial.println("JSON parsing failed!");
    } else {

      JsonObject obj1 = doc["results"][0];  //new 一个对象存储访问result信息
      String cityName = obj1["location"]["name"].as<String>();
      String weather = obj1["now"]["text"].as<String>();
      String code = obj1["now"]["code"].as<String>();
      String temperature = obj1["now"]["temperature"].as<String>();
      int code_int = obj1["now"]["code"].as<int>();
      OLEDDisplay(cityName, weather, temperature, code_int);  //屏幕显示报文信息
      //int code_int = obj1["now"]["code"].as<int>();
      Serial.println(cityName);
      Serial.println(code);
      //Serial.println(weather);
      Serial.println(temperature);

    }
  }else {
    Serial.println("Error on HTTP request");
  }
  http.end(); //Free the resources
}

//屏幕绘制信息
void OLEDDisplay(String cityName, String weather, String temperature, int code_int){

  u8g2.setFont(u8g2_font_wqy12_t_gb2312);
  u8g2.setFontDirection(0);
  u8g2.clearBuffer();
  //u8g2.drawXBMP(3, 3, 35, 32, cloudy); u8g2.setCursor(43 , 16 ); u8g2.println("多"); u8g2.setCursor(43 , 30 ); u8g2.println("云");
  u8g2.setCursor(65, 10);
  u8g2.print("城市: ");
  u8g2.print(cityName);
  u8g2.setCursor(65, 25);
  u8g2.print("天气: ");
  u8g2.print(weather);
  u8g2.setCursor(65, 40);
  u8g2.print("温度: ");
  u8g2.print(temperature);
  u8g2.print("℃");

  //==========================天气图标显示=========================//
  
  //u8g2.drawXBMP(10, 3, sunny_x, sunny_y, sunny);
  switch (code_int){
    case 0:
      u8g2.drawXBMP(3, 3, sunny_x, sunny_y, sunny);
      break;
    case 1:
      u8g2.drawXBMP(3, 3, clear_x, clear_y, clear);
      break;
    case 2:
      //
      break;
    case 3:
      //
      break;
    case 4:
      u8g2.drawXBMP(3, 3, cloudy_x, cloudy_y, cloudy);
      break;
    case 5:
      u8g2.drawXBMP(3, 3, PartlyCloudy_x, PartlyCloudy_y, PartlyCloudy);
      break;
    case 6:
      u8g2.drawXBMP(3, 3, MostlyCloudy_x, MostlyCloudy_y, MostlyCloudy);
      break;
    case 7:
      u8g2.drawXBMP(3, 3, PartlyCloudy_x, PartlyCloudy_y, PartlyCloudy);
      break;
    case 8:
      u8g2.drawXBMP(3, 3, MostlyCloudy_x, MostlyCloudy_y, MostlyCloudy);
      break;
    case 9:
      u8g2.drawXBMP(3, 3, Overcast_x, Overcast_y, Overcast);
      break;
    case 10:
      u8g2.drawXBMP(3, 3, Shower_x, Shower_y, Shower);
      break;
    case 11:
      u8g2.drawXBMP(3, 3, Thundershower_x, Thundershower_y, Thundershower);
      break;
    case 12:
      u8g2.drawXBMP(3, 3, ThundershowerWithHail_x, ThundershowerWithHail_y, ThundershowerWithHail);
      break;
    case 13:
      u8g2.drawXBMP(3, 3, Rain_x, Rain_y, Rain);
      break;
    case 14:
      u8g2.drawXBMP(3, 3, Rain_x, Rain_y, Rain);
      break;
    case 15:
      u8g2.drawXBMP(3, 3, Rain_x, Rain_y, Rain);
      break;
    case 16:
      u8g2.drawXBMP(3, 3, Rain_x, Rain_y, Rain);
      break;
    case 17:
      u8g2.drawXBMP(3, 3, Rain_x, Rain_y, Rain);
      break;
    case 18:
      u8g2.drawXBMP(3, 3, Rain_x, Rain_y, Rain);
      break;
    case 19:
      u8g2.drawXBMP(3, 3, IceRain_x, IceRain_y, IceRain);
      break;
    case 20:
      u8g2.drawXBMP(3, 3, Sleet_x, Sleet_y, Sleet);
      break;
    case 21:
      u8g2.drawXBMP(3, 3, SnowFlurry_x, SnowFlurry_y, SnowFlurry);
      break;
    case 22:
      u8g2.drawXBMP(3, 3, Snow_x, Snow_y, Snow);
      break;
    case 23:
      u8g2.drawXBMP(3, 3, Snow_x, Snow_y, Snow);
      break;
    case 24:
      u8g2.drawXBMP(3, 3, Snow_x, Snow_y, Snow);
      break;
    case 25:
      u8g2.drawXBMP(3, 3, Snow_x, Snow_y, Snow);
      break;
    case 26:
      u8g2.drawXBMP(3, 3, Dust_x, Dust_y, Dust);
      break;
    case 27:
      u8g2.drawXBMP(3, 3, Dust_x, Dust_y, Dust);
      break;
    case 28:
      u8g2.drawXBMP(3, 3, Duststorm_x, Duststorm_y, Duststorm);
      break;
    case 29:
      u8g2.drawXBMP(3, 3, Duststorm_x, Duststorm_y, Duststorm);
      break;
    case 30:
      u8g2.drawXBMP(3, 3, Foggy_x, Foggy_y, Foggy);
      break;
    case 31:
      u8g2.drawXBMP(3, 3, Haze_x, Haze_y, Haze);
      break;
    case 32:
      u8g2.drawXBMP(3, 3, Windy_x, Windy_y, Windy);
      break;
    case 33:
      u8g2.drawXBMP(3, 3, Windy_x, Windy_y, Windy);
      break;
    case 34:
      u8g2.drawXBMP(3, 3, Hurricane_x, Hurricane_y, Hurricane);
      break;
    case 35:
      u8g2.drawXBMP(3, 3, Hurricane_x, Hurricane_y, Hurricane);
      break;
    case 36:
      u8g2.drawXBMP(3, 3, Tornado_x, Tornado_y, Tornado);
      break;
    case 37:
      u8g2.drawXBMP(3, 3, Cold_x, Cold_y, Cold);
      break;
    case 38:
      u8g2.drawXBMP(3, 3, Hot_x, Hot_y, Hot);
      break;
    case 99:
      u8g2.drawXBMP(3, 3, Unknown_x, Unknown_y, Unknown);
      break;
  
  break;
  }

  //--------------------------时间显示--------------------------//
  printLocalTime();

  u8g2.sendBuffer();
  //delay(1000);
}

//时间获取

void printLocalTime(){

  //delay(500);
  struct tm timeinfo;
  if (!getLocalTime(&timeinfo))
  {
      Serial.println("Failed to obtain time");
      u8g2.setCursor(0,60);
      u8g2.print("未获取到时间请重置");
      return;
  }
  Serial.println(&timeinfo, "%F %T %A"); // 格式化输出,串口显示
  //屏幕显示时间
  //u8g2.clearBuffer();
  u8g2.setFont(u8g2_font_wqy12_t_gb2312);
  u8g2.setCursor(30, 64);
  u8g2.print(&timeinfo, "%F");  //日期
  u8g2.setCursor(65, 52);
  u8g2.print(&timeinfo, "%A");  //星期
  u8g2.setCursor(95, 64); 
  u8g2.print(&timeinfo, "%R"); //小时-分钟
  // u8g2.setCursor(80, 64); 
  // u8g2.print(&timeinfo, "%T"); //小时-分钟-秒 

  //==========================底部标语==========================//
  // u8g2.setCursor(0,63);
  // u8g2.print("平安喜乐");

  //u8g2.sendBuffer();
}

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
  delay(1000);
  u8g2.begin();
  u8g2.enableUTF8Print();

  WiFi_Connect();

  //从网络时间服务器上获取并设置时间
  // 获取成功后芯片会使用RTC时钟保持时间的更新
  //            时区           时令             网络服务器
  configTime(gmtOffset_sec, daylightOffset_sec, ntpServer);

  
}

void loop() {
  // put your main code here, to run repeatedly:
  ParserJson();
  delay(3000);  //知心天气API访问限制 20次/min
}

```

# 参考文档
[https://blog.csdn.net/liujialong11/article/details/120068060?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-2-120068060.pc_agg_new_rank&utm_term=esp32%E8%8E%B7%E5%8F%96%E4%BF%A1%E6%81%AF%E6%98%BE%E7%A4%BA&spm=1000.2123.3001.4430](https://blog.csdn.net/liujialong11/article/details/120068060?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-2-120068060.pc_agg_new_rank&utm_term=esp32%E8%8E%B7%E5%8F%96%E4%BF%A1%E6%81%AF%E6%98%BE%E7%A4%BA&spm=1000.2123.3001.4430)
[https://seniverse.yuque.com/books/share/e52aa43f-8fe9-4ffa-860d-96c0f3cf1c49/nyiu3t](https://seniverse.yuque.com/books/share/e52aa43f-8fe9-4ffa-860d-96c0f3cf1c49/nyiu3t)
[https://blog.csdn.net/qq_41868901/article/details/104221495?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-4-104221495.pc_agg_new_rank&utm_term=esp32+%E5%9B%BE%E7%89%87%E5%8F%96%E6%A8%A1&spm=1000.2123.3001.4430](https://blog.csdn.net/qq_41868901/article/details/104221495?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-4-104221495.pc_agg_new_rank&utm_term=esp32+%E5%9B%BE%E7%89%87%E5%8F%96%E6%A8%A1&spm=1000.2123.3001.4430)