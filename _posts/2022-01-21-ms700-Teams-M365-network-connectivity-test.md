---
layout:     post
title:     在本地网络测试到Teams的连接性工具：Microsoft 365 network connectivity Test
subtitle:  Teams的网络规划与配置系列文章-2
date:       2022-1-21
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的网络规划与配置
- MS-700-Teams认证考试 
---

介绍另一种在线的测试本地网络与M365之间的连接性能的工具：

Microsoft 365 network connectivity test and dashboard https://connectivity.office.com/

![Graphical user interface, text, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image002.jpg)

 直接点击运行测试，它就会自动检测你的物理位置，并进行与M365的连接测试

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image004.jpg)

 在测试过程中，它会自动提示你下载并安装测试工具，安装完之后就会自动跑测试项目

![Waterfall chart  Description automatically generated with medium confidence](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image006.jpg) ![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image008.jpg)

跑完之后，就会显示我与其它人相比的得分，显然我所在的网络与M365之间的连接不怎么样，只有60-80分。

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image010.jpg)

点开“详细“之后，可以更新详细地看到测试的结果，因为我是用长城带宽的，所以杯具了。

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image012.jpg)

到Exchange online的测试结果

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image014.jpg)

到Share point的测试结果

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image016.jpg)

 最重要的指标来了，到Teams的测试结果如下。影响语音质量的三大指标（掉包，延迟，抖动）就有两个不达标。

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image018.jpg)

 那么，通过这个简单的Microsoft 365 network connectivity 测试工具，能给我们另一个手段来预先测试所有网络对于Teams的质量影响，也可以为故障排错提供一些数据支撑。

 

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />



