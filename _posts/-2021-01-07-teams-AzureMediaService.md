---
layout:     post
title:      如何使用Teams Meeting进行直播
subtitle: 	使用OBS推送Teams Meeting到其它直播平台
date:       2021-01-05
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- Azure
- Live Event
- 网络直播
---

> 前文再续，书接上一回，上一文章有讲到腾讯会议的直播平台可以自定义，那么我们来看看是如何自定义其它的直播平台，因为默认的直播平台是使用腾讯云，那么出于某些原因需要用到其它直播平台的话，那应该怎么处理呢？通过本文我https://www.wowza.com/blog/streaming-protocols)
>

[TOC]



# 架构

Teams Meeting to OBS to RTMP to Azure to Live event

# 工具准备

下载OBS，https://obsproject.com/zh-cn/download

![image-20210107144900583](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107144900583.png)



# 流媒体编码器的配置要点

[Configuring on-premises live encoder settings](https://docs.microsoft.com/en-us/azure/media-services/latest/recommended-on-premises-live-encoders?editorHostOrigin=https%3A%2F%2Fportal.azure.com&fromOrigin=https%3A%2F%2Fportal.azure.com&context=context%2Fchromeless&trustedAuthority=https%3A%2F%2Fportal.azu)

Configuration tips

- Whenever possible, use a hardwired internet connection.
- When you're determining bandwidth requirements, double the streaming bitrates. Although not mandatory, this simple rule helps to mitigate the impact of network congestion.
- When using software-based encoders, close out any unnecessary programs.
- Changing your encoder configuration after it has started pushing has negative effects on the event. Configuration changes can cause the event to become unstable.
- Always test and validate newer versions of encoder software for continued compatibility with Azure Media Services. Microsoft does not re-validate encoders on this list, and most validations are done by the software vendors directly as a "self-certification."
- Ensure that you give yourself ample time to set up your event. For high-scale events, we recommend starting the setup an hour before your event.
- Use the H.264 video and AAC-LC audio codec output.
- Stick to supported resolutions and frame rates for the type of Live Event you are broadcasting to (for example, 60fps is currently rejected.)
- Ensure that there is key frame or GOP temporal alignment across video qualities.
- Make sure there is a unique stream name for each video quality.
- Use strict CBR encoding recommended for optimum adaptive bitrate performance.



# 配置步骤

![image-20210107150058578](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107150058578.png)



![image-20210107150416846](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107150416846.png)



![image-20210107150445075](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107150445075.png)



![image-20210107150543928](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107150543928.png)



![image-20210107150645616](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107150645616.png)



![image-20210107150859078](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107150859078.png)



![image-20210107155319553](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107155319553.png)

![image-20210107155348389](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210107155348389.png)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />