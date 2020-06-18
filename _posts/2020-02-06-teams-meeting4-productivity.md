---
layout:     post
title:      微软 Teams Meeting 系列(4) 
subtitle:  把MS Teams Room 成为企业的生产力工具
date:       2020-02-06
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 本文是基于MS Ignite 2019 MTG30 Microsoft Teams Rooms deployment for an inclusive and enhanced meeting room 的课程视频整理而成的。

在本系列的之前几个文章中，已经简介过Teams Meeting 的一些重要功能了，那本文将会介绍一下Teams Meeting 是如何来做企业的会议室标准化，以及把它成为企业的生产力工具而不是一件摆设。

为什么要会议室标准化呢？因为现在很多大中型企业在物理会议室的配比与合理使用方面可能会面临这样一些问题：

- 实体会议室配比不足：会议室数量不够，员工想开会的时候总是找不到会议室；会议室过多，总是有闲置的会议室当做员工的聚餐地方，造成严重的资源浪费；

- 会议室使用冲突：预约好的会议室被人占用了，召开中的会议被人进来打扰了，到点了还没有人来会议室开会….

- 需要一场协作会议：在实际的会议场景当中，一场远程会议开展一来并不是需要把大家的面看得如何清楚，大家都在忙什么，而是把需要讨论的东西，需要展示的东西说清楚，讲明白，那么除了视频的流畅与质量之外，我们就需要会议室当中的远程协作能力（远程桌面，远程PPT，远程控制）（对于我来说，看到主持人的演示桌面比看到他的脸蛋更加重要与搭实）

- 进入会议室后不知道如何进行视频会议：针对一些移动办公的员工来说，每到不同分公司的会议室后，他都需要学习如何使用会议室里面的设备/或者请IT帮忙，所以为了提升用户满意度，我们需要把会议室进行标准化改造，把各地的会议室务求做成一致性；

- 易于使用的问题：各种反人类的音频与视频接入方式，以至于每一次开会议都需要找IT help desk 来帮忙

- 不同的分公司/各地的办公室使用不同的远程视频会议(北京办公室用Teams, 广州办公室用Zoom, 深圳办公室用腾讯会议…)，这样会导致大家在不同的会议系统当中来回切换，无所适从。

还有，传统会议室是不是还有这些现状？

1. 会议室当中连一台大尺寸的显示屏也没有？现在这么便宜的显示屏
2. 各地的会议室的沟通还是单靠一台模拟电话？
3. 视频会议形同虚设？
4. 分散在不同系统的文件共享系统与聊天系统？

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p1111141839b942f995e7d74739d6d7ad5cf46.png)

所以在不同的会议场景下面需要有专业,专门的设备来支持实现这场会议的一致性,协作性,便捷性,扩展性,包容性 来实现企业的会议室标准化，那本文的主角就是以下产品/解决方案：Microsoft Teams Room (MTR) , 其实这就是一台Mini PC + Touch Panel + Teams Room APP  的一套设备，硬件牌子可以多选，但解决方案就一个：解决了会议室的一致性,协作性,便捷性,扩展性,包容性 问题。

为了避免踩坑，可以直接上下面的MTR系统，因为已经出厂就安装好MTR系统的了。但如果还是天生爱折腾的话，还可以DIY在Surface上面安装MTR来作为触摸控制屏，并且外接一个大显示器作为投影，也是一种比较经济的实现方式，参考 *1

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p22223a651040c1c9d32bd329f32e01b10c07.png)

MTR实现会议室的一致性与包容性：

针对企业中常用到的四大会议场景：大中小微的会议室，都可以使用配置上MTR + MIC + SPK + Cam + ‘显示大屏’ 来实现会议室的一致性，只不过基于不同的会议室大小配置不数数量的显示大屏，不同数量的麦克风，不同功率的喇叭，当然还要加上MTR.

这些都务求可以在会议室当中提供极致的会议体验，让那些远程参与会议的人员能够体验在会议室当中放置的mic, spk的优质效果

正如下图一样，为了实现 MTR 一致性, 所有会议室的会议体验都是一致的，无论你身处哪个地方的哪间会议室，都可以实现一样的会议体验，以提高大家的沟通效率与质量，这不就是我们需要的吗？

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p33389e15565a25eb3fe8ec36fac791978a7.png)

微软已经为一系列的摄像头, 麦克风, 喇叭做了一些认证设备的推荐，可以参考下面的网址，经过Teams认证的设备具有优质的音视频体验，所以还是建议大家去了解一下的，请参考 *2

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p40c0458f05e6c34e61cc83c92d07c1b59.png)

关于会议室的便捷性：

在进入MTR会议室后，对于终端用户来说是需要非常傻瓜式的入会方式：一键入会，

在过去，要完成一场流畅的视频会议，需要IT人工地到会议室去查看所有设备功能都是运行正常使用的，那现在通过MTR的一键入会，就节省了所有人的时间，提高效率的同时，也方便大家进行日常的沟通工作

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p55559800727a0f40ded31cd21af594fe64df.png)

MTR实现会议室的协作性：

当开展一场MTR会议的时候，把主持人的电脑屏幕投到本地的显示大屏，或者远程的与会者电脑上面是很必要的，在MTR控制屏的后方会有一个HDMI IN的接口，只要直接连接到电话的HDMI 口即可把电脑的桌面投到本地以及整个个Teams会议当中，以便所以本地与远程与者的交互。

在MTR当中，还可以进行白板交互：

- 如果配合一块可触摸的显示大屏，就可以使用MS Whiteboard来与多个远程与会者进行电子白板交互

- 如果你只有一场普通的白板呢，当然没有问题，在MTR当中只有使用合适的内容照相机（如下）也能把白板的内容投影到Teams Meeting当中，请参考 *3


![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p6666a1523fb709ef0aae94959a1776afe041.png)

> 20200615注：在小型会议室的场景，Teams的会议室设备还有一款叫Collaboration Bar的产品

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p777717f0c070815584b651b280666df22846.png)

总结一下为什么要选择MTR？

1） 所有的MTR会议室的入会体验都是一致的：一键进入预约好的会议，经过Teams认证音视频设备带来身临其境的会议体验。

2）让Teams Room成为企业的一个重要生产力工具，或生产力场所。

3）使用HDMI IN接入电脑即可把电脑桌面共享出来。

4）灵活多样的MTR硬件可选。

5）还是MTR的会议体验一致。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p9999e4af87703c1dcfadb60c8e9d05bda3f6.png)

最后，在Teams Admin Center还可以对MTR的进行设备管理与控制

> 注：在最新的TAC上面，Teams Room的管理已经被Collaboration Bar管理替代了
>
> 可参考：[Teams会议室中的新秀-Collaboration Bar](https://tangx007.github.io/2020/06/11/Teams-CollaBar-X50/
> )

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p-106930979751b739a59aee9f58e2f342c4.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p-11f861871eba28ab992d7d42c2ab411cff.png)

参考链接：

*1 Microsoft Teams全生命周期会议-03你了解MeetingRoom吗 https://blog.51cto.com/scnbwy/2413631

*2 aka.ms/teamsdevices

*3 Microsoft Teams Rooms Content Camera 革命性更新 https://blog.51cto.com/scnbwy/2434429

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />