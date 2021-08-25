---
layout:     post
title:      Microsoft Teams 在小型会议中的轻量化解决方案
subtitle:  Collaboration Bar - AudioCodes RXV80
date:       2021-7-29
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- AudioCodes
---

本文将再介绍一下Microsoft Teams硬件生态圈里面的一个会议室场景：小型会议室 或 电话间 ，当大家在使用微软Teams作为公司的办公协作平台的时候，就会遇到这样一个情况：“*公司有足够的预算来为大型会议室与培训室配备昂贵的会议设备*” ，但是对于小会议室/电话间 来说却没有必要投入这么高的成本来装备这些会议室，但同时小会议室又是企业里面使用频率最高，协作效率最高的办公场所之一。

如何为这些类型的会议室也能应用上Teams Meeting的能力？就是说用户无论在大中型会议室中可以使用Teams MTR会议终端开启Teams会议，同时在小型会议室中也能方便地开启Teams Meeting呢？微软的Teams硬件生态里面有一种会议设备叫做Collaboration bar（协作栏：一种集扬声器，麦克风，摄像头于一体的Teams会议终端）正可以以较低成本的方式，简单快捷地部署在这些小地方上面。

> 提起生态，微软早在Lync时代已经开始引入生态，围绕Lync这套统一沟通平台，周边会有各种各样的厂家提供不同的解决方案，例如Lync话机，认证语音网关，会议室解决方案SRS，第三方开发的Add-on, Lync SDK....
>
> 到了Skype for business时代，也是沿用了Lync生态的模式，养活了不少的ISV与硬件厂家们，但是这两套解决方案都是基本本地服务器的私有化部署，维护难度大，迭代速度慢，这些周边的硬件如电话机，会议终端是没有办法集成到一个统一平台进行管理的。
>
> 直到现在Lync/Skype 升级到基于公有云的Teams解决方案后，我们发现其实只有位于Teams生态里面的硬件设备（如Teams话机，Teams会议设备，Teams门牌）即使它们不是同一个厂家，但都可以统一在Teams后台进行管理。我认为这就是基于SaaS应用的生态系统的优势之一，软件厂家把SaaS做好，硬件厂家把硬件做实。

# 一致的Teams会议体验 

之前一篇文章介绍过Poly x50的Collaboration bar，本文测试的是奥科公司的一款Collaboration bar，型号为RXV80；相对于Poly x50来说RXV80只集成是麦克风与摄像头，扬声器的能力是沿用显示器的喇叭。

它与其它Teams MTR会议终端一样，因为操作界面完全一样可以减少用户的学习成本，这一点非常重要，微软对于所有的认证会议设备来说，一致的会议体验非常重要。

以下就是RXV80，一个小小的会议终端，它配置的遥控器完成满足了所有的操作需求。当然可以外接一个无线键鼠来提高操作度。

![image-20210729132654144](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210729132654144.png)

我拿了微软关于协作栏的一个GIF作演示，其实所有的协作栏界面都是一样，通过在Teams或Outlook里面预约好Teams会议室后，日程会推送到设备上面，快到会议开始时就可以点击遥控上面的Teams button一键入会，使用体验跟所有的MTR完全是一致的。

![thumbnail image 1 of blog post titled  	 	 	  	 	 	 				 		 			 				 						 							The first collaboration bar for Microsoft Teams is now available! 							 						 					 			 		 	 			 	 	 	 	 	 ](https://techcommunity.microsoft.com/t5/image/serverpage/image-id/178016i7C25DF13FC72BD3C/image-size/large?v=v2&px=999)

如果你的企业已经配置了Teams Phone System的话，还可以直接把外部的PSTN用户邀请到会议室里面来，如下图：

![image-20210823111548201](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210823111548201.png)

# 连接触控大屏来提高Teams会议的交互体验

一般的小会议室，只需要把Teams协作栏RXV80的网络，HDMI高清显示，电源连接上后即使用Teams Meeting Room，但显然它还可以匹配更多的会议场景，如下图：

- （标记1）可以配置一个扩展麦克风，从而提高会议室的收音能力。
- （标记4）用户的笔记本电脑可以通过连接设备后面的HDMI IN接口，直接把电脑桌面共享到Teams Meeting当中。熟悉Teams MTR的朋友一定会知道这个非常好用的功能。
- （标记5）现在的会议室里面，如果没有一块可以触控的大屏的话，应该是非常不方便的，那么RXV80的后置USB接口连接到触控大屏的触控USB之后，我们就可以启用Teams的白板功能与远端用户互动了。

> PS. 吐槽点之一是Teams Meeting是沿用Microsoft Whiteboard作为它的白板共享，相对于国内用户来说连接速度非常的慢，相对于其它大厂的白板共享来说，功能迭代又是相对地慢。

![image-20210729135020728](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210729135020728.png)

但无论如何集音视频于一体的会议终端（bar形态）一定是未来会议室场景的重要形态之一。

购买回来后，接电源，接网络，接显示，即可立即使用，并立即通过Teams admin portal进行设备管理。

重点是无任何许可费用产生哦。

![image-20210729135144226](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210729135144226.png)

![image-20210822191336646](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822191336646.png)

# 轻量的部署与管理 | Simple to deploy and manage

接着，我们来详细看看Teams admin portal对于会议终端能管理到一个什么程度？

如下图，进入Teams admin portal > 设备 > 协作栏，一旦设备登陆上Teams就可以直接在后台显示出来，并接受Teams admin的管理

![image-20210822192704752](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192704752.png)

可以对设备进行基本的固件更新与重启

![image-20210822192800208](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192800208.png)

在Teams后台中，通过创建一个配置文件可以实现批量地对一批Teams协作栏进行配置管理，对于IT Pro这是一个非常重要的功能。

如下，可以设置设备的设备锁，语言，时区，日期格式，时间格式，等。

![image-20210822192905770](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192905770.png)

![image-20210822192916384](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192916384.png)

![image-20210822192932753](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210822192932753.png)

最后总结一下，在小型会议室里面，使用微软的Teams协作栏方案（如文中介绍的奥科RXV80）可以快速地，低成本地，轻量化地部署到这种会议室里，同时无论你使用哪个认证的硬件厂家的会议设备，都可以直接注册到Teams admin portal上面进行统一管理。无须复杂的接线，无须重新学习的成本，统一使用界面，统一管理后台，就可以很简单到部署到一间Teams会议室。

# 参考

#### [AudioCodes RXV80](https://www.microsoft.com/zh-cn/microsoft-teams/across-devices/devices/category/teams-rooms/20?filterIds=24&page=1) 

[RXV80 Standalone Video Collaboration Bar User's and Administrator's Manual (audiocodes.com)](https://www.audiocodes.com/media/15892/rxv80-standalone-video-collaboration-bar-users-and-administrators-manual.pdf)



------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:25%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />