---
layout:     post
title:      Microsoft Teams会议室中的新秀
subtitle:  Collaboration Bar - Poly X50
date:       2021-11-11
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- Poly
---

>  *目前Teams社区有来自日本，新加坡以及美国的朋友，社区每月会组织进行Teams相关技术分享，同时每周五也会有由微软Office365高级产品经理Ares Chen进行的“Teams Friday下午茶”技术讲座分享，当然有疑问或建议也会有专家及时解答！*
>
>  *可以通过以下链接填写相关信息，我们会自动将你加入社区*
>
>  [*https://aka.ms/jointeamsdevcommunity*]()
>
>  *可以通过以下链接收看“Teams Friday下午茶”技术分享*
>
>  [*https://aka.ms/teamsfriday*]()

### 这是什么东西？

Microsoft Teams不单单是一款办公协作平台，它在数字化会议室当中作为远程视频会议平台也是非常出色的，这就是Teams Room了，也是之前的会议室系统SRS的下一代产品，我们简称它为MTR (可参考我之前一篇文章“[把MS Teams Room 成为企业的生产力工具](https://blog.51cto.com/nemotan/2469498)”)

MTR的设备一般都要配合电脑主机，显示屏，扬声器，麦克风，摄像头，MTR控制器一并组合使用，所以可以想像在布线与后期维护都需要不少的工作，但是一旦投入使用，它在会议室进行Teams会议时，可以保持会议体验是一致的，同时入会非常便捷。

今天为大家介绍的是另一款Teams会议室设备, 微软称之为collaboration bar, 我翻译为“协作棒”？据Teams Blog的介绍，它适用于1到5个人的小型会议室，为了保持出色的会议体验而使用，我们来看看这种设备的一些使用场景

小型会议室

![image-20200611171214252](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611171214252.png)

单人使用的协作会议室

![image-20200611161044509](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611161044509.png)

进行白板协作（卖点之一）

![image-20200611161153393](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611161153393.png)

接线非常简单（卖点之二）

![image-20200611161229598](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611161229598.png)

暂时现在只看到有这两款collaboration bar

![image-20200611161317546](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611161317546.png)

### 固件更新

那我拿到手的是Poly Studio x50，这台机器在6月10日才第一次支持Teams Collaboration bar的模式，之前的固件都只支持zoom, gotomeeting, bluejeans的会议，所以首先需要升一下级才能支持

接线非常简单，电源线，网线，HDMI线接入即可使用

![image-20200611162247730](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611162247730.png)

搭配一块PoE供电的会议控制屏TC8

![image-20200611171349170](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611171349170.png)

因为固件非常大，有2G多，所以我们采用离线的方式进行升级，先在自己的电脑里面把IIS搭建出来（自行百度）

下载最新的x50固件，并解压到 C:\inetpub\wwwroot

https://www.poly.com/cn/zh/products/video-conferencing/studio/studio-x50

![image-20200611164817269](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611164817269.png)

接着增加一个.cfg的MIME Type，如下图

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610130807.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610130843.png)

最后，确保下面这份文件能被访问出来

![image-20200610131354288](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200610131354288.png)

进入x50的配置界面（admin, 123），进入设备管理和更新，下载源选择自定义服务器URL，然后按下图把IIS服务器配置上

它会自动检测新固件，然后点击全部更新即可开始更新

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610130616.png)

![image-20200611162459535](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611162459535.png)

经过约一小时左右的升级，重新回到管理后台，进入“提供者”，就可以看到这固件支持Teams了

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610130541.png)

经过基本的Teams身体验证登陆后，就会出来一个熟悉的画面了，是不是很像MTR哈？

![image-20200611170603750](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611170603750.png)

在Teams约一个会议看看？之后，你可以通过触控屏幕上面直接点“加入”，也可以在控制屏上面点“Join”加入会议

![image-20200611165651284](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611165651284.png)

别人共享桌面

![image-20200611170308151](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611170308151.png)

你还可以在别人共享出来的白板上面，通过触摸屏与大家进行协作，交互

这就是collaboration bar的特色，专门设计来进行白板协作与提升会议的交互体验

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611164352979.png" alt="image-20200611164352979"  />

### 使用Teams后台进行设备管理

一旦连接上Teams，这台设备就可以通过Teams TAC进行管理

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610141754.png)

远程重启

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610141850.png)

远程下发配置

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200610141532.png)

这台机器的优势在于集成度非常高，布线非常简单；同时它可以原生地在后台切换zoom, teams, gotomeeting, poly几个会议平台，相对于其它MTR设备来说应该算是比较大的优势。

### 参考

#### Poly Studio X50 https://support.polycom.com/content/support/north-america/usa/en/support/video/poly-studio-x-family/poly-studio-x50.html

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />