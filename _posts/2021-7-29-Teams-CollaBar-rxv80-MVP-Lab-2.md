---
layout:     post
title:      微软MVP研究实验室 | 基于微软Teams的小型会议室设备体验
subtitle:  协作栏 Collaboration Bar - AudioCodes RXV80
date:       2021-7-29
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- AudioCodes
---

[TOC]



# 微软MVP研究实验室

大家好，我是本期的MVP实验室研究员：谭国欣，今天我将介绍通过微软Teams认证的一款会议室终端来带领伙伴们认识微软Teams的后台是如何管理这些会议室终端的。接下来让我们开始这次的探索之旅吧！

# 微软MVP实验室研究员

谭国欣 Nemo Tan

微软Office 365 & Service 方向的最有价值专家，对于Teams语音落地及Teams会议室方案有深入的认识，帮忙伙伴们规划及架构企业内的Teams语音及会议生态。

运营的博客及网站：

- https://blog.51cto.com/nemotan
- https://www.51nemo.info/

# 正文

今天我会介绍一下Microsoft Teams硬件生态圈里面的一个会议室场景：小型会议室 或 电话间  是如何应用上Teams Meeting的能力的。

![image-20210915164819482](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210915164819482.png)

当大家在使用微软Teams作为公司的办公协作平台的时候，就会遇到这样一个情况：“*公司有足够的预算来为大型会议室与培训室配备昂贵的会议设备*” ，但是对于小会议室/电话间 来说却没有必要投入这么高的成本来装备这些会议室，但同时小会议室又是企业里面使用频率最多，协作效率最高的办公场所之一。

如何为这些类型的会议室也能有Teams Meeting的能力呢？

微软的Teams硬件生态里面有一种会议设备叫做Collaboration bar（协作栏：一种集扬声器，麦克风，摄像头于一体的Teams会议终端）就可以较低成本的方式，简单快捷地部署在这些小地方上面。

今天体验的Teams会议设备是奥科公司的一款Collaboration bar，型号为RXV80，它只集成是麦克风与摄像头，扬声器的能力是沿用显示器的喇叭，因为操作界面与MTR完全一样，可以减少使用者的学习成本，这一点非常重要，微软对于所有的认证会议设备来说，一致的会议体验是必选项，如下：

![image-20210729132654144](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210729132654144.png)

# 简单的安装

接下来，我们只需要把Teams协作栏RXV80的网线，HDMI线，电源线连接上后即可使用Teams Meeting Room。同时RXV80 还有扩展麦克风接口、HDMI IN接口与USB接口以适应还多的会议场景 

![image-20210729135020728](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210729135020728.png)

登陆上我们已有的Teams会议室帐号，就可以立即在这个小会议室上面使用Teams Meeting了，如下图：

![image-20210917091506736](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210917091506736.png)

我们可以从另一个Teams帐号上面呼叫到这个会议室，来实现点对点的呼叫

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210917092228359.png" alt="image-20210917092228359" style="zoom:50%;" />

如果你的企业已经配置了Teams Phone System的话，还可以直接把手机用户邀请到Teams Meeting 里面来，如下图：

![image-20210823111548201](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210823111548201.png)

# 轻量化的后台管理

接着，我们来详细看看Teams Admin Center对于会议终端能管理到一个什么程度？

如下图，进入Teams Admin Center > 设备 > 协作栏，一旦设备登陆上Teams就可以直接在后台显示出来，并接受Teams admin的管理

![image-20210822192704752](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192704752.png)

可以对设备进行基本的固件更新与重启

![image-20210822192800208](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192800208.png)

在Teams Admin Center 中，通过创建一个配置文件可以实现批量地对一批Teams协作栏进行配置管理，对于IT Pro，这是一个非常重要的功能。

如下图，可以配置设备的设备锁，语言，时区，日期格式，时间格式，等。

![image-20210822192905770](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192905770.png)

我们还可以配置背景光亮度，背景光超时，高对比度，等等。

> BTW, 为什么要用到显示高对比度模式呢？这类功能跟微软对于D&I文化的宣传与认同有关，大家可以在微软的其它产品中发现这些功能。
>
> 高对比度针对有弱视障碍的用户在阅读低对比度的文本时可能会遇到困难。 例如，有些网站采用的颜色组合较差（比如使用黑色背景和蓝色链接）， 即使视力正常的用户阅读这样的内容也很吃力，更不用说有视力障碍的用户了。 对比强烈的颜色可让他们更快速、更轻松地在电脑/设备上进行阅读或观看。

![image-20210822192916384](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192916384.png)

一些基本的网络配置也可以从Teams Admin Center中远程推送到协作栏当中：

![image-20210822192932753](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192932753.png)

# 总结

在小型会议室里面，我们今天使用了微软的Teams协作栏解决方案（如文中介绍的奥科RXV80）可以：

- 快速地，低成本地，轻量化地部署到小型会议室当中。
- 无论你使用哪个Teams认证的硬件厂家的会议设备，都可以直接注册到Teams Admin Center上面进行统一管理。
- 无须复杂的接线，无须重新学习的成本，统一使用界面，统一管理后台，就可以很简单到部署到一间Teams会议室。

# 参考

#### [AudioCodes RXV80](https://www.microsoft.com/zh-cn/microsoft-teams/across-devices/devices/category/teams-rooms/20?filterIds=24&page=1) 

[Manage phones, collaboration bars, Teams displays, and Teams panels](https://docs.microsoft.com/en-us/microsoftteams/devices/device-management#manage-phones-collaboration-bars-teams-displays-and-teams-panels?WT.mc_id=M365-MVP-5003881)

[Collaboration bar for Microsoft Teams](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/the-first-collaboration-bar-for-microsoft-teams-is-now-available/ba-p/1231706?WT.mc_id=M365-MVP-5003881)

[Global Diversity and Inclusion](https://www.microsoft.com/en-us/diversity/inside-microsoft/default.aspx?WT.mc_id=M365-MVP-5003881)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:25%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />