---
layout:     post
title:      实测腾讯会议的直播功能（1）
subtitle: 	一步一步带你入门腾讯会议直播
date:       2021-01-05
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Tencent-Meeting
- 
---

开启腾讯会议的直播功能需要在预定会议的界面里面的打开，如下图：

- 打开直播之后的会议就具备了直播功能的腾讯会议室了。

- 腾讯会议直播功能当前仅支持 PC 端，腾讯会议直播目前最多支持4路视频推送，同时 Web 端不限人数观看。

  

![image-20210105082416316](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105082416316.png)

# 筹备一场直播的一些安全考虑

开启腾讯会议的直播是很简单的事情，因为这是SaaS服务所以没有太多的技术难度，但是因为直播也许是公开的，也许是设置了密码的半公开直播活动，一些安全的设定还是需要事前考虑清楚，列几点如下：

- [ ] 设置腾讯会议的入会密码，因为腾讯直播是与腾讯会议绑定在一起的，为了保证我们的主会场（腾讯会议室）的安全性，必须为会议设置密码

![image-20210105082507344](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105082507344.png)

- [ ] 当然除了设置入会密码外，更加安全的做法是预定特邀会议，依赖于特邀会议只能通过微信发送给特定与会者，且不能被转发这一特性，我们可以限制只有特定一些与会者才能加入会议，更加安全。

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105144648185.png" alt="image-20210105144648185" style="zoom:67%;" />

- [ ] 上面两个是加入腾讯会议的安全选项，以下是关于这场直播的安全选项。如果您有一些直播是非公开/半公开/知识付费的/机密的...等等，我们可以在开启直播后开启直播密码，如下图：

![image-20210105085731327](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105085731327.png)

- [ ] 开启等候室，启用之后所以与会者参加会议都必须得到主持人的允许
- [ ] 入会议时自动静音，强烈建议启用，其中一个原则就是谁发言谁开麦，不发言不开麦，我认为这是作为是一远程会议参与者对所有人一种基本礼貌。
- [ ] 开启屏幕水印，一旦直播被公开之后，这种技术可以让我们共享出来的材料/PPT/桌面内容的版权得到一个有效的保障。

![image-20210105082542670](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105082542670.png)

- [ ] 举行一场真正的直播并不是一个人可以搞定的，而是需要所有会议与会者，主持人，助理共同协作的成果。那么设置一个备份主持人是非常有必要性的，如下图


![image-20210105082653802](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105082653802.png)

# 腾讯会议与直播的分发

一旦直播被创建出来之后，会自动打开Outlook日历，里面含有必须的参会信息，我们可以通过邮件/微信 等方法分发会议信息 或 直播信息给其它人了，如下图

上面那个链接是腾讯会议的会议链接，下面那个是腾讯会议的直播链接，但自动生成出来的两条链接却没有说明用途让人有点困惑

![image-20210105082802449](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105082802449.png)

# 开始直播前的准备工作

1. 所有需要直播露面的与会者提前进入腾讯会议进行测试。
2. 共享的内容先准备好，并共享到会议中。
3. 也许你的主会场还有更加的复杂的A/V设备来支持这场直播，那就不在本文的范围内了。

![image-20210105090920716](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105090920716.png)

3. 共享屏幕之后的直播开关隐藏得非常深，如下图，更多-》直播-》点击之后就可以打开直播控制窗口

![image-20210105091733962](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105091733962.png)

4. 直播控制台里面，只有一种直播布局（如下图），可以随意把共享屏幕与参会者的视频应用到直播当中，当您点击应用之后，可实时进行画面切换。

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105093817360.png" alt="image-20210105093817360" style="zoom:67%;" />

5. 高级应用的话，我们还可以自定义直播平台，这将在下一节再述。理论上支持RTMP协议的直播平台应该是可以来直接腾讯会议的，如Azure Media Service, 哔哩哔哩的直播平台等... 

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105092624286.png" alt="image-20210105092624286" style="zoom:50%;" />

# 开始你的直播带货之旅

点击开始直播就可以直接开始直播

![image-20210105154258308](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105154258308.png)

腾讯会议与直播的内容一定会有一点延迟的（由于直播的后台架构导致），那么腾讯会议直播的延迟是大约10秒左右

![image-20210105093955991](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210105093955991.png)

# 参考

[腾讯会议 开启直播 - 操作指南 - 文档中心 - 腾讯云 (tencent.com)](https://cloud.tencent.com/document/product/1095/43660)

