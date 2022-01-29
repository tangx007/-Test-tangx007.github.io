---
layout:     post
title:     一劳永逸地配置Teams策略 - Teams策略包
subtitle:  Teams的日常治理与生命周期管理系列文章-2
date:       2022-1-29
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的日常治理与生命周期管理
- MS-700-Teams认证考试 
---

在Teams的管理中心（TAC）下面，可以使用许许多多，不同功能的策略来管理我们的Teams平台，例如消息传递策略、会议策略、应用设置策略、呼叫策略、实时事件策略（即直播策略）、呼叫寄存策略、团队策略、语音路由策略…等等，如果你的组织足够大，Teams用户足够多的话，我相信每天这样来分配与调整这些Teams策略的话，一定是个累活，那么Policy Package（策略包）功能就可以派上用场了，先来看看它的位置在哪里

![img](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/policypackages/clip_image002.jpg)

你可以自定义的一组策略和设置，通过一些特别的用户使用场景来自由组合不同的策略，并应用于这个用户的群组上面，这样子你基本上就可以一劳永逸了。

微软已经预定义了一系列的应用场景，但事实上你需要的是合符你需要的策略包，如下例子，我们新建了产线的办公室员工的策略包：

会议策略：不允许录音；语音路由策略：通过北京出局；呼叫策略：允许打电话；

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/policypackages/clip_image004.jpg)

接着，把策略包分配给一个特定的用户群组，以后只要有新员加入到这个群组，都会自动分配上我们定义的策略了。

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/policypackages/clip_image006.jpg)

为了验证是否真的生效，我们随便查询了一下Test群组内的用户，在策略Tab中便可看到“产线的办公室员工“策略包被成功分配。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/policypackages/clip_image008.jpg)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

