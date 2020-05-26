---
layout:     post
title:      xxx
subtitle:  Network Assessor for Microsoft Teams
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

Skype 在线和 Skype 团队都是实时通信工具，它们严重依赖他们运行在上面的网络基础设施。在可预见的将来，这在云部署中一直是，并将继续是一个挑战。为了准备为在线业务或 Microsoft 团队部署 Skype，您需要首先确保您的网络基础结构能够处理这些应用程序产生的实时流量。

为了帮助这一点，Microsoft 发布了一个称为 Skype 业务网络评估工具的工具。此工具通过命令行运行，并使用 Skype 业务客户端使用的相同媒体堆栈，以测试从网络到 Microsoft Office 365 的连接上的数据包丢失、抖动、往返延迟和重新排序数据包百分比。该工具创建的音频流将发送到 Office 365，然后从最近的边缘服务器位置镜像回当前物理位置。基于返回流，该工具能够提供有关数据包丢失、抖动、往返延迟和重新排序数据包百分比的准确统计信息。这将让您了解当前网络环境的拥塞程度，以及您希望 Skype 业务或 Skype 团队会议在您的环境中运行情况。

Microsoft 网络评估工具非常有用，因为它允许您在 Office 365 租户部署之前测试网络，并且它是免费的！然而，该工具所缺乏的是能够在一段时间内轻松监视网络并生成格式良好的输出，以便向经理或客户显示。所以，我想我会花一些时间，试图纠正这些限制，并建立一个新的前端的工具...



![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200526094300.png)

> 参考：
>
> [Network Assessor for Microsoft Teams](https://www.myteamslab.com/2017/08/network-assessor-for-skype-for-business.html)