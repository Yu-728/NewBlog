---
title: OpenCV+YOLO+IP摄像头实现目标检测
---
# 前言
<!--more-->
学习OpenCV、YOLO到现在我实现了调用本地摄像头使用自己训练的模型进行目标识别，然后想着能不能远程获取视频数据，然后再PC端处理，最后将结果返回给视频流端。然后发现旧手机下载IP摄像头之后可以当做一个远程摄像头使用，并且它还支持rstp网络视频流协议（海康、大华的摄像头也是用这个协议，还可以兼容未来硬件的升级）

# 代码
```
import time
import torch
import cv2 as cv


class MultipleTarget:

    def __init__(self, url):
        """
        初始化
        """
        # 加载训练模型
        self.model = torch.hub.load('./yolov5', 'custom', path='./weight/yolov5s.pt', source='local')
        # 设置阈值
        self.model.conf = 0.52  # confidence threshold (0-1)
        self.model.iou = 0.45  # NMS IoU threshold (0-1)
        # 加载摄像头
        self.url = url
        self.cap = cv.VideoCapture(self.url)
        self.cap.set(cv.CAP_PROP_FOURCC, cv.VideoWriter_fourcc('M', 'J', 'P', 'G'))
        if not self.cap.isOpened():
            print("Cannot open camera")
            exit()

    def draw(self, list_temp, image_temp):
        for temp in list_temp:
            name = temp[6]  # 取出标签名
            temp = temp[:4].astype('int')  # 转成int加快计算
            cv.rectangle(image_temp, (temp[0], temp[1]), (temp[2], temp[3]), (0, 0, 255), 3)  # 框出识别物体
            cv.putText(image_temp, name, (int(temp[0] - 10), int(temp[1] - 10)), cv.FONT_ITALIC, 1, (0, 255, 0), 2)

    def detect(self):
        """
        目标检测
        """
        while True:
            ret, frame = self.cap.read()
            # 如果正确读取帧，ret为True
            if not ret:
                print("Can't receive frame (stream end?). Exiting ...")
                break
            # frame = cv.flip(frame, 1)

            # FPS计算time.start
            start_time = time.time()

            # Inference
            results = self.model(frame)
            pd = results.pandas().xyxy[0]  # tensor-->pandas的DataFrame
            # 取出对应标签的list
            person_list = pd[pd['name'] == 'person'].to_numpy()
            bus_list = pd[pd['name'] == 'bus'].to_numpy()
            # 框出物体
            self.draw(person_list, frame)
            self.draw(bus_list, frame)
            # end_time
            end_time = time.time()
            fps = 1 / (end_time - start_time)

            # 控制台显示
            # results.print()  # or .show(), .save(), .crop(), .pandas(), etc.
            # print(results.xyxy[0])  # img1 predictions (tensor)
            # print('----------------')
            # print(results.pandas().xyxy[0])  # img1 predictions (pandas)

            # FPS显示
            cv.putText(frame, 'FPS:' + str(int(fps)), (30, 50), cv.FONT_ITALIC, 1, (0, 255, 0), 2)

            cv.imshow('results', frame)
            cv.waitKey(10)
            if cv.waitKey(10) & 0xFF == ord('q'):
                break

        self.cap.release()
        cv.destroyAllWindows()


url = 'rtsp://admin:admin@192.168.43.229:8554/live'
test = MultipleTarget(url)
test.detect()

```
# 存在问题
在不进行目标检测的时候，读到的视频流很流畅，进行目标检测后就非常卡几乎不能用。
经过几天的学习和查找，感觉这个问题出在这里：
***CPU和内存在读视频流和处理视频的时候爆了***
我在运行程序的时候看了任务管理器果然如此
然后我就根据[网上的说法](https://blog.csdn.net/qq_44717317/article/details/116052377)使用多进程来解决这个问题，**但是结果还是一个样**
我现在在怀疑是不是我的电脑配置不够（ps：我的电脑配置确实垃圾）

**************************
**有搞了几天没有丝毫进展！！！！！！！！！**
躺了，试了很多方法还是卡的一批，延迟还贼高，无奈
![](https://img0.baidu.com/it/u=197787017,108575186&fm=253&app=120&size=w931&n=0&f=JPEG&fmt=auto?sec=1656435600&t=bbf109097a3d9bb5c2853fb92e98db43)