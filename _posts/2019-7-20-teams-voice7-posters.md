---
layout:     post
title:      MS Teams的逻辑架构与语音解决方案
subtitle:  官方海报下载 Official Posters Download
date:       2019-7-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

MS Teams的逻辑架构与语音解决方案-官方海报分享给大家下载

Microsoft Teams IT architecture and telephony solutions posters
https://docs.microsoft.com/en-us/microsoftteams/teams-architecture-solutions-posters

接下来跟大家一起简单过一下关于Teams的两张海报：

Teams是一款很优秀的产品，它就像一只八爪鱼一样，连接着Office365各种不同的资源，为其所用，而且不断地快速地更新迭代，以下这张海报正是说明这个事实，同时让我们了解到Teams的各个功能究竟调用了Office365的哪些组件。

![Teams 的逻辑架构与语音解决方案 - Official Posters Download](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p1111459786693e9a05c39dc062c05ff24d73.png)

1）Files，其实就是1:N chats 里面的Files，它里面的文件是上传到OneDrive for Business里面的，从下图就可以看出了

![Teams 的逻辑架构与语音解决方案 - Official Posters Download](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p22210151ac6f3c85651e7626dfcb3fdde31.png)

2）Team Coversation, 它调用了SharePoint，每一个Team对应着SharePoint的Site, 每一个Channel对应着SharePoint的一个文件夹。
同时Chat and channel 里面的短消息是保存在Exchange里面，为了Information Protection.

![Teams 的逻辑架构与语音解决方案 - Official Posters Download](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p333313c62c90ebf7d8f18c964a90a2596b6e.png)

3）Teams Meeting里面的日历信息就是拉取Exchange里面的Calenda. 很明显嘛。

4）Teams Meeting的会议录制则是同步存放在Microsoft Stream里面，这个功能非常好，录制完的会议自动上传到Stream里面，再配合权限控制可以让更多的与会者回放会议录像。

其实，我手上还有这样一张逻辑图片，拿去参考吧

![Teams 的逻辑架构与语音解决方案 - Official Posters Download](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p44446970a9c9fdd8b44f7994d0ea722a09bb.png)

还有一张海报是关于Teams的电话语音落地的各种解决方案，这个在我另一个Blog里面有具体讲述：Microsoft Teams Voice语音落地系列-1 架构简述：https://blog.51cto.com/nemotan/2377504

归纳如下：
1）Teams + Phone System + Calling Plan , 这是最简单的落地方案，购买相关的许可即可实现
2）Teams + Phone System + Direct Routing + Local SBC, 这是符合中国国情的解决方案，也是最复杂的方案，也是最灵活的解决方案，可以关注这个系列文章：Microsoft Teams Voice语音落地系列

3）Teams/Skype Online + Phone System + CCE + Local SBC,  这是相对于第二种方案来说的，其实是针对Skype online来说，因为skype online不支持Direct Routing,  所以它只能使用本地架设的一台CCE (云连接器) 来与Skype online通讯。

4）Skype for Business on premise的语音落地，跟Teams无关，这也是传统的解决方案，我在这方面有丰富的经验，有需要可以留言。

![Teams 的逻辑架构与语音解决方案 - Official Posters Download](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p555553bbf12a18e118babd1dda01b4814388e.png)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />