---
layout:     post
title:      实测腾讯会议的直播功能（2）
subtitle: 	增加直播平台
date:       2021-01-05
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Tencent-Meeting
- Live Event
- 网络直播
---

> 前文再续，书接上一回，上一文章有讲到腾讯会议的直播平台可以自定义，那么我们来看看是如何自定义其它的直播平台，因为默认的直播平台是使用腾讯云，那么出于某些原因需要用到其它直播平台的话，那应该怎么处理呢？通过本文我们可以了解到以下信息点：
>
> - RTMP协议，以及它在直播中的原理与作用
> - 哔哩哔哩的直播平台开播设置，以及获取RTMP推流地址的介绍
> - 腾讯乐享的直播平台开播设置，以及获取RTMP推流地址的介绍
> - 微软的Azure Media Service的开播设置，以及获取RTMP推流地址的介绍

[TOC]



# RTMP协议介绍



![image-20210114085139440](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210114085139440.png)



推流

![image-20210114085236100](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210114085236100.png)

拉流，反操作

![image-20210114085301382](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210114085301382.png)

音视频传输协议

![image-20210114085359711](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210114085359711.png)

音视频基本参数

![image-20210114085605105](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210114085605105.png)

# 开播前的准备条件

- 哔哩哔哩帐号
- 腾讯会议商业版与企业版（免费版不支持直播推流）https://meeting.tencent.com/buy.html



# 哔哩哔哩的开播设置

哔哩哔哩（Nasdaq:BILI；英文名称：bilibili，简称B站）现为中国年轻世代高度聚集的文化社区和视频平台，该网站于2009年6月26日创建，被粉丝们亲切地称为“B站”，所谓的二次元社区，年青人的世界，所以为了引来更加多的直播流量，把直播流推送到B站是很有必要的，接下来我们来看看怎么玩的？

- 登陆B站

- 进入https://live.bilibili.com/ 哔哩哔哩的直播平台，点击开播设置

![image-20210105122352474](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210105122352474.png)

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210105122604520.png" alt="image-20210105122604520" style="zoom: 33%;" />

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210105122702924.png" alt="image-20210105122702924" style="zoom:33%;" />

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210105162930706.png" alt="image-20210105162930706" style="zoom:33%;" />

经过一天的等待之后...

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210106145056052.png" alt="image-20210106145056052" style="zoom:33%;" />

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210106145127451.png" alt="image-20210106145127451" style="zoom:33%;" />

一旦完成了实名认证之后，在B站后台打开直播之后就会有RTMP地址与直播码

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210106150708963.png" alt="image-20210106150708963" style="zoom:50%;" />

分别复制RTMP地址到腾讯直播的推流地址，直播码复制到推流密钥

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210106150810859.png" alt="image-20210106150810859" style="zoom:50%;" />

然后就可以把腾讯会议的直播推送到b站直播平台上面

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210106151003115.png" alt="image-20210106151003115" style="zoom:50%;" />

![image-20210106154832030](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210106154832030.png)

# 腾讯乐享的开播设置

什么是腾讯乐享？简单来说它是一个企业内部社区沟通与内部培训的平台，可以参考一个这一篇推文：https://mp.weixin.qq.com/s/rFOw6Xt8ZA3WozLvGHivUQ

腾讯乐享是一个企业内部一站式社区产品，提供知识管理、员工培训、企业文化建设、员工内部社交等功能。相对于钉钉是移动办公入口平台，乐享是企业文化建设的应用或知识管理的应用。

其实腾讯会议本身也有推流平台的，为什么要用腾讯乐享来作为直播平台，有什么优势呢？腾讯乐享直播可生成回放，进行观看视频，乐享直播的系统更加完善，比较适用于培训等场景。

这里假设你已经开通了乐享，并进入了它的管理后台，如下图创建一场直播

![image-20210107115326910](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107115326910.png)

当你创建完直播间之后，点击"服务器/串流密钥"

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107115722365.png" alt="image-20210107115722365" style="zoom:50%;" />

这就是我们需要的信息，服务器地址对应的是RTMP服务器地址（推流地址），串流密钥对应的是推流密钥（B站上面叫做直播码）

![image-20210107115651011](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107115651011.png)

把这两个重要的RTMP推流信息添加到腾讯会议直播中

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107120219646.png" alt="image-20210107120219646" style="zoom:50%;" />

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107120125919.png" alt="image-20210107120125919" style="zoom:50%;" />

然后我们就可以轻松地把视频流推送到腾讯乐享的直播间了

![image-20210107120517406](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107120517406.png)

# Azure Media Service的开播设置



# 参考

[RTMP Streaming: The Real-Time Messaging Protocol Explained](https://www.wowza.com/blog/rtmp-streaming-real-time-messaging-protocol) 

Live Video Streaming 原理视频

[Streaming Protocols: Everything You Need to Know | Wowza](https://www.wowza.com/blog/streaming-protocols)





------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />