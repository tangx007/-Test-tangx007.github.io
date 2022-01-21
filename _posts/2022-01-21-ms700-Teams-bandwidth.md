---
layout:     post
title:     通过Teams Network Planner来预估Teams带宽使用情况
subtitle:  Teams的网络规划与配置系列文章-1
date:       2022-1-21
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的网络规划与配置
- MS-700-Teams认证考试  
---

# Teams的带宽要求

Teams 可以提供最佳的音频、视频和内容共享体验，而不管你的网络条件如何。也就是说，当带宽不足时，Teams 会优先考虑音频质量而不是视频质量，而当在带宽不受限制的情况下，Teams 会优化媒体质量，包括高保真音频、高达1080p 的视频分辨率以及高达 30fps（每秒帧数）的视频和共享内容。‎

所以Teams可以根据你所在网络的带宽情况来调整音频、视频与共享内容的质量来给你带来流畅的使用体验。视频布局、视频分辨率和每秒视频帧数这几个因素会影响到Teams客户端的带宽使用情况，以下表格整理了这些场景下面的Teams带宽使用情况：

![Table  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image002.png)

以上只是介绍出每一个Teams 负载（音频、视频、共享）的带宽建议，但在实际使用中我们会混合了所有的Teams负载来使用，而不是单单地使用某一个负载，例如我们有时候会使用音频+视频的方式，有时候会音频+视频+共享，有时候只使用音频。

所以接下来会介绍的是一个更接地气的带宽评估工具：Teams Network Planner.

 # Teams Network Planner

通过上述表格，我们只能大概了解到Teams在网络带宽上面占用情况，但是对于复杂多变的国内企业或跨国企业的环境，我们需要通过更加灵活的量化工具来协助我们评估Teams在生产环境中的使用情况，那么在Teams管理中心（下述 TAC）中的网络规划器Network Planner可以帮助我们完成这项工作，如下图：

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image004.jpg)

 接下来，如何通过Network Planner来评估网络带宽预估呢？

首先，通过Network Planner中的站点规划把企业内使用Teams的站点与人数先创建出来

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image006.jpg)

以下的ExpressRoute, WAN, Internet链接容量与PSTN出口可以按实际情况填写

![Graphical user interface, text, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image008.jpg)

以下例子，我创建了上海站点（100M的出口带宽）与广州站点（10M的出口带宽）作为示例：

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image010.jpg)

接着，我们回到“角色” 来设置企业中的角色类型，微软已经预定义好三种角色类型（Teams会议室，远程办公，办公室人员），你可以根据需要再定义自己的角色来更加精准计算。

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image012.jpg)

通过添加角色可以我们可以为自定义角色选择不同的Teams应用负载

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image014.jpg)

 完成以上步骤之后，就是生成评估报告，如下示例进入：

具体路径：网络规划器》规划名称》报告》添加报告

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image016.jpg)

通过“二八原则”来分配办公室人员与远程办公人员的比例（80：20）

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image018.jpg)![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image020.jpg)

最后点击“生成报告”后，即可生成报告。如下报告，就可以立即发现在只有10M出口带宽的广州站点如果启用了Teams视频的负载之后，就会超出带宽极限了。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image022.jpg)

通过图表的方式来呈现报告：

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/bandwidth/clip_image024.jpg)

 

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />



