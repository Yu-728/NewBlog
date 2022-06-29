---
title: YOLOv5实现训练训练集
---
# 环境配置
<!--more-->
pycharm
YOLOv5
pytorch
labelling

# 收集样本，贴标签
## 收集样本用爬虫收集，下面是我用的爬虫代码：
爬取百度图库图片
```
# -*- coding:utf8 -*-
import requests
import json
from urllib import parse
import os
import time


class BaiduImageSpider(object):
    def __init__(self):
        self.json_count = 0  # 请求到的json文件数量（一个json文件包含30个图像文件）
        self.url = 'https://image.baidu.com/search/acjson?tn=resultjson_com&logid=5179920884740494226&ipn=rj&ct' \
                   '=201326592&is=&fp=result&queryWord={' \
                   '}&cl=2&lm=-1&ie=utf-8&oe=utf-8&adpicid=&st=-1&z=&ic=0&hd=&latest=&copyright=&word={' \
                   '}&s=&se=&tab=&width=&height=&face=0&istype=2&qc=&nc=1&fr=&expermode=&nojc=&pn={' \
                   '}&rn=30&gsm=1e&1635054081427= '
        self.directory = r"E:\爬取百度图库相关图片\searchName{}"  # 存储目录  这里需要修改为自己希望保存的目录  {}不要丢

        self.header = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/95.0.4638.54 Safari/537.36 Edg/95.0.1020.30 '
        }

    # 创建存储文件夹
    def create_directory(self, name):
        self.directory = self.directory.format(name)
        # 如果目录不存在则创建
        if not os.path.exists(self.directory):
            os.makedirs(self.directory)
        self.directory += r'\{}'

    # 获取图像链接
    def get_image_link(self, url):
        list_image_link = []
        strhtml = requests.get(url, headers=self.header)  # Get方式获取网页数据
        jsonInfo = json.loads(strhtml.text)
        for index in range(30):
            list_image_link.append(jsonInfo['data'][index]['thumbURL'])
        return list_image_link

    # 下载图片
    def save_image(self, img_link, filename):
        res = requests.get(img_link, headers=self.header)
        if res.status_code == 404:
            print(f"图片{img_link}下载出错------->")
        with open(filename, "wb") as f:
            f.write(res.content)
            print("存储路径：" + filename)

    # 入口函数
    def run(self):
        searchName = input("查询内容：")
        searchName_parse = parse.quote(searchName)  # 编码

        self.create_directory(searchName)

        pic_number = 0  # 图像数量
        for index in range(self.json_count):
            pn = (index+1)*30
            request_url = self.url.format(searchName_parse, searchName_parse, str(pn))
            list_image_link = self.get_image_link(request_url)
            for link in list_image_link:
                pic_number += 1
                self.save_image(link, self.directory.format(str(pic_number)+'.jpg'))
                time.sleep(0.2)  # 休眠0.2秒，防止封ip
        print(searchName+"----图像下载完成--------->")


if __name__ == '__main__':
    spider = BaiduImageSpider()
    spider.json_count = 10   # 定义下载10组图像，也就是三百张
    spider.run()


```
## 贴标签
网上有很多教程，这里放一个我用的
[贴标签教程1](https://blog.csdn.net/GenuineMonster/article/details/118614657)
[贴标签教程2](https://www.cnblogs.com/StarZhai/p/11926610.html)


# 在Pycharm下训练
![](https://i.niupic.com/images/2022/06/09/a0oD.jpg)
相关目录形式如上图所示
其中关于coco128文件夹以及在YOLO中需要修改.yaml文件根据这个[教程](https://blog.csdn.net/GenuineMonster/article/details/118614657)进行对应的设置

在准备好coco128文件和对应的.yaml文件的设置后点开train.py文件找到下图所示代码：
![](https://i.niupic.com/images/2022/06/09/a0oE.jpg)
主要关注红圈或者红线的参数
对应参数的解释大致如下：
**Epoch**
一个epoch , 表示： 所有的数据送入网络中， 完成了一次前向计算 + 反向传播的过程。
由于一个epoch 常常太大， 分成 几个小的 baches .
将所有数据迭代训练一次是不够的， 需要反复多次才能拟合、收敛。
在实际训练时、 将所有数据分成多个batch ， 每次送入一部分数据。
使用单个epoch 更新权重 不够。
随着epoch 数量的增加， 权重更新迭代的次数增多， 曲线从最开始的不拟合状态， 进入优化拟合状态， 最终进入过拟合。
epoch 如何设置： 大小与数据集的多样化程度有关， 多样化程度越强， epoch 越大。
**batchsize**
每个batch 中： 训练样本的数量。
batch size 大小的选择也很重要， 最优化网络模型的性能+速度。
当数据量较小， 计算机可以承载只有1个batch 的训练方式时， 收敛效果会好。
mini-batch : 将所有数据分为若干个batch , 每个batch 包含一部分训练样本。。
**iterations**
完成一次epoch 需要的batch 个数
batch numbers 就是 iterations .
分为了多少个batch? : 数据总数/ batch_size
[参数解释链接](https://blog.csdn.net/weixin_38754799/article/details/109831970?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-5-109831970-blog-107870356.pc_relevant_antiscanv3&spm=1001.2101.3001.4242.4&utm_relevant_index=7)
以上这些参数根据自己需要修改
***红下划线参数，workers，需要将default=0***
这些设置完后就可以开始训练了，右键运行train.py文件，运行过程如下图：
![](https://i.niupic.com/images/2022/06/09/a0oL.jpg)
在训练完后你会在run文件夹中看到你本次训练的结果，如下图：
![](https://i.niupic.com/images/2022/06/09/a0oF.jpg)

