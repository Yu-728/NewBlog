---
title: esp32 JSON解析和编码
---

### esp32 json信息解析
<!-- more -->
当我们访问网站API获取到得信息一般都为JSON格式，因此如何解析JSON格式信息将其转换成我们需要的信息成了一个问题。
这个时候就体现了咱们Arduino平台的强大，我们有很多开源的库，ArduinoJson库就是一个开源的解析JSON格式信息的库。怎么在platform下载ArduinoJson不在赘述。
#### 主要来看JSON格式信息解析的实现
**直接贴代码**
```
/**
 * 解析Json字符串
 * @author 单片机菜鸟
 * @date 2019/06/02
 */
#include <ArduinoJson.h>
#include <Arduino.h>

void setup() {
  Serial.begin(115200);
  while (!Serial) continue;
  // Json对象对象树的内存工具 静态buffer 
  // 200 是大小 如果这个Json对象更加复杂，那么就需要根据需要去增加这个值.
  // StaticJsonDocument 在栈区分配内存   它也可以被 StaticJsonDocument（内存在堆区分配） 代替
  // DynamicJsonDocument  doc(200); 堆区分配内存
  StaticJsonDocument<200> doc;
  // Json格式写法，创建一个json消息
  char json[] =
      "{\"sensor\":\"gps\",\"time\":1351824120,\"data\":[48.756080,2.302038]}";

  // Add an array.
  //
  // Deserialize the JSON document
  DeserializationError error = deserializeJson(doc, json);

  // Test if parsing succeeds.解析是否成功
  if (error) {
    Serial.print(F("deserializeJson() failed: "));
    Serial.println(error.c_str());
    return;
  }
  // Fetch values.
  //
  // Most of the time, you can rely on the implicit casts.
  // In other case, you can do doc["time"].as<long>();
  const char* sensor = doc["sensor"];
  long time = doc["time"];
  double latitude = doc["data"][0];
  double longitude = doc["data"][1];

  // Print values.
  Serial.println(sensor);
  Serial.println(time);
  Serial.println(latitude, 6);
  Serial.println(longitude, 6);
}

void loop() {
  // not used in this example
}

```

## 参考文档
[https://blog.csdn.net/dpjcn1990/article/details/92831648](https://blog.csdn.net/dpjcn1990/article/details/92831648)
