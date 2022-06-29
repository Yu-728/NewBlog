---
title: 基于OpenCV+mediapipe实现简单的手势识别
---
# 基于OpenCV+mediapipe实现简单的手势识别
一开始学习OpenCV就是想做一个手势识别和人脸识别小程序，结果谷歌已经有开源的mediapipe库了，今天看到就去了解了一下，并根据这个库找到了一个大佬写的手势识别小程序，记录下来学习学习。
大佬代码如下:
<!--more-->
```
import cv2
import mediapipe as mp
import time

# 打开计算机自带摄像头
cap = cv2.VideoCapture(0)

mpHands = mp.solutions.hands
hands = mpHands.Hands()  # 设置参数，详见 hands.py 中的 __init__
mpDraw = mp.solutions.drawing_utils  # 将检测出的手上的标记点连接起来

# 定义时间用于后边的fps计算
pTime = 0
cTime = 0

while True:
    success, img = cap.read()
    img = cv2.flip(img, 1)  # 图像翻转
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)  # 将BGR格式图像转换为RGB
    results = hands.process(imgRGB)  # 对输入图像进行处理，探索图像中是否有手
    # print(results.multi_hand_landmarks)  # 如果有手，输出手所有0~20个标记点的比例坐标，如果没有，输出None
    if results.multi_hand_landmarks:
        for handLms in results.multi_hand_landmarks:  # 捕捉画面中的每一只手
            for id, lm in enumerate(handLms.landmark):
                # print(id, lm)
                h, w, c = img.shape
                cx, cy = int(lm.x * w), int(lm.y * h)  # 根据比例还原出每一个标记点的像素坐标
                print(id, cx, cy)  # 根据手上标记点的id打印出其相应所在图像中中的像素位置
                if id == 4:  # 可以根据手上标记点的id获得任意id对应的标记点的信息
                    cv2.circle(img, (cx, cy), 10, (255, 0, 255), cv2.FILLED)  # 这里加粗强调了大拇指上的一个标记点

            mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS)  # 给画面中的每一只手进行标点、连线的操作

    # 得到fps
    cTime = time.time()
    fps = 1/(cTime - pTime)
    pTime = cTime
    # 在画面上显示fps
    cv2.putText(img, 'FPS:' + str(int(fps)), (10, 70), cv2.FONT_ITALIC, 1, (0, 0, 255), 3)
    cv2.imshow("Image", img)
    key = cv2.waitKey(1)    # 自动刷新
    if key == 27:
        break
cv2.destroyAllWindows()

```
[大佬链接](https://blog.csdn.net/weixin_40247876/article/details/117032959?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-117032959-blog-116183404.pc_relevant_default&spm=1001.2101.3001.4242.2&utm_relevant_index=4)