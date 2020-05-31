---
layout:     post
title:      xxx
subtitle:  Network Assessor for Microsoft Teams
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Network
- 
---

> 网络质量是基于IP语音的实时通讯系统好坏的重要指标，其中又以Round Trip, Jitter, Packetloss作为三个主要的因素来衡量

Skype for Business online 与 Microsoft Teams 都是微软的实时通信解决方案，但它们严重依赖其运行在上面的网络基础设施。在可预见的将来，这些将是部署在云中系统的一个挑战。为了更好地使用Teams,您需要首先确保您的网络基础结构能够处理这些应用程序产生的实时流量。

为了客户去知道这些，微软发布了一个称为Network Assessor for Microsoft Teams 的网络评估工具。此工具通过命令行运行，并使用与Skype for Business 客户端使用相同媒体堆栈，以测试从用户的本地网络到 Microsoft Office 365 的连接上的数据包丢失、抖动、往返延迟和重新排序数据包百分比。
该工具会自动创建音频流发送到 Office 365，然后从最近的边缘服务器位置镜像回当前用户的物理位置。基于返回流，该工具能够提供有关数据包丢失、抖动、往返延迟和重新排序数据包百分比的准确统计信息。这将让您了解当前网络环境的拥塞程度。

Microsoft 网络评估工具非常有用，因为它允许您在 Office 365 租户部署之前测试网络，并且它是免费的！
然而，该工具所缺乏的是能够在一段时间内轻松监视网络并生成格式良好的输出，以便向经理或客户显示。所以，本文正是向大家介绍这样一个网络工具来监视Teams 客户端所在网络的质量情况。

下载回来后，直接安装即可以使用，如下截图：

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200526094300.png)

使用它来主要监视这几个网络因素：Round Trip, Jitter, Packetloss，xxxx，我们来看看微软为它们定义的好坏阀值：


> 参考：
> [Network Assessor for Microsoft Teams](https://www.myteamslab.com/2017/08/network-assessor-for-skype-for-business.html)
