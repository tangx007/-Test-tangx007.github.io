---
alayout:     post
title:      MS Teams 会议的兼容性(3)
subtitle:  MTR原生支持Cisco,Zoom的一键入会
date:       2020-1-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

在MS Ignite2019技术大会上面，微软宣称在2020初可以支持Cisco,Zoom的一键入会，这样带来的好处是企业可以同时使用MTR会议设备来支持两套不同的会议系统，这又是微软在兼容性上面再进一步。

在过去的二十年里，视频会议系统是通过昂贵的硬件系统（称为[视频电话会议系统）](https://en.wikipedia.org/wiki/Videotelephony)实现的。这些最初是为同一公司的视频会议室开会而构建的，因此，以前的会议系统都是以设备硬件为主的;

我应该购买 Tandberg（后来被思科购买）还是 Polycom 的视频会议系统呢？后来，采用行业视频互操作性标准，例如是[SIP](https://en.wikipedia.org/wiki/Session_Initiation_Protocol)和[h.323](https://en.wikipedia.org/wiki/H.323)，这样来兼容不同的会议系统，例如Skype 与 Polycom的互通，Skype与Cisco的互通，等等~

它们使公司能够举办高保真视频会议，这些会议可以（几乎）取代面对面的会议。然而，这些系统非常昂贵，因此，视频会议仍然局限于很少的会议室 - 实际上，只有 5-10% 的所有会议室部署了 VTC（具体取决于您阅读的分析师研究）。慢慢地，本地服务器时代开始，我们可以直接把会议系统直接部署的企业的私有云上面，而不需要使用这些昂贵的会议系统，例如微软的Lync or Skype for Business，但这些本地化的会议系统慢慢也暴露了它的一些缺点：维护困难，功能更新慢，外网接入的会议体验不佳，外网带宽消耗大，部署与交付周期长，等。

在最近的几年，这些会议系统又转向云。因为显然，转向云对在线会议来说是一个非常积极的发展。技术供应商构建了"在云中诞生"的新会议服务，并且比本地会议服务器（VTC MCUs）更可靠、质量更高、交付创新的速度要快得多。思科的 WebEx、Zoom 和微软的Teams等服务利用技术创新使会议更丰富、更具吸引力，在过去几年中，在线会议在业务中使用已显著扩展。

![image-20200615112640852](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200615112640852.png)

这些基于云的SaaS视频会议系统基本上没有使用门槛，不同的公司在选择适合于他们自己的会议系统，所以问题又来了，不同公司之间的沟通在所难免，如何在一台设备当中同时支持多种会议系统这是大家都找着的设备，在微软的MTR系统中未来就可以这样来支持，下图中我们可以看到他们的内部版本已经可以实现在MTR中支持了

![undefined](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/large)

但是在最新一版的MTR 4.4.25.0当中仍然还没法正式支持，我们还是持续关注中...

![image-20200615111127978](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200615111127978.png)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />