---
layout:     post
title:     尝鲜使用Teams的公共预览版功能
subtitle:  Teams的日常治理与生命周期管理系列文章-1
date:       2022-1-29
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的日常治理与生命周期管理
- MS-700-Teams认证考试 
---

本系列文章会介绍Teams的日常治理以及团队的生命周期管理，其中会包括如何为用户分配不同的Teams功能（聊天，会议，呼叫等功能），Teams的策略包管理，M365的组管理与命名策略，对Team（团队功能）的增删改查的操作，等等一系列日常的运维工作。

![A picture containing graphical user interface  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/teamspublicreview/clip_image002.png)

 # Teams 更新策略

首先介绍的是Teams 更新策略，用于管理 Teams 和 Office 预览用户，这些用户将在 Teams 应用中看到预发布版本或预览功能。（Public preview）公共预览版提供对 Teams 中未发布的功能的早期访问权限。预览版允许浏览并测试即将推出的功能。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/teamspublicreview/clip_image004.jpg)

简单来说，就是用来控制预览功能的一个策略，默认的全局策略是关注 Office 预览版，你还可以自定义更新策略来分配给特定的用户来试用Teams的新功能

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/teamspublicreview/clip_image006.jpg)

接着，把“狗粮更新策略“分配给相关用户即可。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/teamspublicreview/clip_image008.jpg)

# Powershell参考

  `Get-CsTeamsUpdateManagementPolicy`

  `Set-CsTeamsUpdateManagementPolicy`

`New-CsTeamsUpdateManagementPolicy`

![Text  Description automatically generated with medium confidence](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/teamspublicreview/clip_image010.jpg)

# 通过Teams客户端启用

除了在TAC中启用预览功能外，你还可以在客户端中手动打开。选择个人资料左侧的三个圆点，以显示 Teams 菜单。  选择“关于 > 公共预览版”。  选择“切换到公共预览版”。

细心的朋友可以发现我的头像右上角就会多出来一个P字，代表Preview

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/teamspublicreview/clip_image012.jpg)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

