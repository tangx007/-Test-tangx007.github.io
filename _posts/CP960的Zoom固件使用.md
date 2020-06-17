---
layout:     post
title:      xxx
subtitle:  xxx
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 你是如何评价这篇文章的？用一句很简单的话



### [CP960-UVC Zoom Rooms套装](http://support.yealink.com/documentFront/forwardToDocumentDetailPage?documentId=303)

先下载CP960的Zoom固件，升级后的CP960就有zoom room控制屏的能力

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617154451407.png" alt="image-20200617154451407" style="zoom:50%;" />

![image-20200617120645378](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617120645378.png)

登陆到CP960管理界面并升级

![image-20200617140818534](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617140818534.png)

![image-20200617140832623](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617140832623.png)

![image-20200617141809934](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617141809934.png)

### 连接架构图

CP960作为zoom room控制器来控制安装在PC上面的Zoom Rooms, 收音与扬声利用yealink  plugin通过网络传送到CP960上面，摄像头直接用USB连接PC, 这种方案可以应用在小型与中型会议室当中，显示大屏与会议桌之间免布线，会议桌大的话可以使用扩展麦克风解决

![image-20200617143643107](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617143643107.png)

![image-20200617143917380](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617143917380.png)

### [会议主机的插件安装](http://support.yealink.com/forward2download?path=ZIjHOJbWuW/DFrGTLnGypjZRKhDplusSymbolXJQ4Ql5i4/5u8gHT7gvdwO4Z0BnJLG3uL/hnpP04wC2LRhfdw7iHWIy6MwV48yXEdqKGBxsEMsrBEcI5p8STN3VrmIFbHeeSjZBG3u44sYtpJMe7l4hQr6Ii3A==)

插件的作用在于：zoom room的媒体流通过插件传送给CP960

![image-20200617144107333](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617144107333.png)

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617144146716.png" alt="image-20200617144146716" style="zoom:50%;" />

### 会议主机与CP960的配对

CP960作为zoom room控制屏

![image-20200617153927637](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200617153927637.png)

配置完成之后，只要zoom上面可以选择CP960作为收音与扬音设备即代表插件配对成功，可以正常使用