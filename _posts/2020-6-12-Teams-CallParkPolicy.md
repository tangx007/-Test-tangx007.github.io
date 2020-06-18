---
layout:     post
title:      Microsoft Teams Call Parking
subtitle:  把Teams呼叫放进停车场
date:       2020-6-12
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

> 在粤语中，我们在车库停车的时候，经常会讲“泊好部车”，意思是把车子停好，其中的“泊” 就是 Park的音译

## 什么是Call Park
Call Park, 呼叫寄存，正如上的例子，Park就是泊车/停车场的意思，那Call Park就好像把呼叫放在停车场一样，也就是可以让Teams用户把呼叫寄存在Teams上面，需要的时候再把它拿出来，或者转移给其它人

![image-20200612104433177](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200612104433177.png)

## 应用场景

这是一个简单但实用的功能，有这样一些应用场景“

- 我正在座位上面使用Teams客户端与别人开会，但发现我需要用会议室拉更多地人进来讨论，那么我如何把这通呼叫转到会议室呢？使用Call Park后，我来到会议室，在会议室的Teams Room设备上面输入Call Park ID，搞定~！
- 我正在座位上面使用Teams客户端与别人开会，但我发现这问题跟我没有关系，反而跟研发同事有关，我请他等待一下的同时，我发起一个Call Park；之后我与研发沟通好之后，告诉他Call Park ID，他们俩就可以互相勾兑了，搞定~！

## 前置条件

- 默认的Call Park是关闭的，最佳实践是针对有需求的用户单独打开
- 这些用户必须是启用了Enterprise Voice的用户，打开EV的前提是购买并分配了Phone System许可，所以有这许可的用户一般都是可以打电话的用户，注意了微软Doc上面并没有这样提醒的哦~！
- Skype for Business IP phone不能使用Teams Call Park
- Island mode不能使用Teams Call Park
- 创建 call park user groups，其实也就是使用相同Call Park Policy的Teams用户作为一个组
> 太可怜了这么复杂，只因Call Park是高级语音功能~！ 

## 策略配置与分配

配置非常简单，就一个开关

![image-20200612113839637](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200612113839637.png)

但你需要创建不同的Call Park Policy，然后分配给对应的用户，让他们成为一个Call Park Group

![image-20200612113906685](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200612104433177.png)

![image-20200612120328640](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200612120328640.png)

再重申一下，这些Call Park Policy用户必须分配Phone System许可，打开EV，设置成Teams only模式

![image-20200612115207672](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200612115207672.png)

## 实际操作

最关键的事情到，我们要验证Call Park是否生效。

> 注意：如果你才刚刚把EV启用的话，你需要等待约24小时让它生效

建立一个Teams呼叫，点击“更多”，点击“Park Call” 即可

![image-20200613073430610](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613073430610.png)

之后就会显示Call Park Code, 例如 10

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613073640834.png" alt="image-20200613073640834" style="zoom:50%;" />

现在，来到另一台Teams设备，在快速拨号中，你就可以找到Parked calls按钮，点击后输入park code即可把呼叫从寄存中取出来

![image-20200613073827112](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613073827112.png)

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613073945196.png" alt="image-20200613073945196" style="zoom:50%;" />

> 参考
>
> https://ucstatus.com/2019/06/05/microsoft-teams-call-parking-is-here/
>
> https://www.msxfaq.de/teams/pbx/callpark.htm
>
> https://docs.microsoft.com/en-US/microsoftteams/teams-calling-policy?WT.mc_id=TeamsAdminCenterCSH

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />