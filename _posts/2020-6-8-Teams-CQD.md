---
layout:     post
title:      使用Power BI来抓取Microsoft Teams中的呼叫数据
subtitle:  Microsoft Call Quality Power BI Connector
date:       2020-6-8
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-other
- 

---

> *问渠那得清如许，为有源头活水来 - 南宋-朱熹*

IT管理员总是会想如何挖掘一个系统的后台数据，以帮忙他们分析与预见一些系统性的问题，稳定性的问题。例如微软的UC解决方案：Skype for business, 很多管理员都想知道现在SFB的整体呼叫质量情况如何？现在的质量不佳的呼叫整体占比如何？如果一个重要会议出现呼叫质量问题，如何去事后排查？这些都需要做一些数据挖掘与分析的工作。

那么它的下一代产品Microsoft Teams，我们如何对它进行呼叫质量分析呢？毕竟它是一个基于云的SaaS产品，它的数据库是不会公开给大家去做查询，所以我们需要微软开放更多的接口来让我们做一些数据分析，这就是我在这里为大家介绍的一个工具Microsoft Call Quality Power BI Connector，通过它，我们可以使用PowerBI来查询到Teams的呼叫数据，并以PowerBI来呈现一张非常高大尚的数据报表出来。

> 什么是PowerBI?  一个数据分析工具，它能实现数据分析的所有流程，包括对数据的获取、清洗、建模和可视化展示，从而来帮助个人或企业来对数据进行分析，用数据驱动业务，做出正确的决策。再简单一点，就是把你导入的数据转化成报表的工具。

当然你一开始并不需要学习什么叫PowerBI，因为这个工具自带几个常用的模板，我们可以直接使用并且去分析你的Teams环境，例如：

![](https://gxcuf89792.i.lithium.com/t5/image/serverpage/image-id/178004i68AA2A7986F5C5E2/image-size/medium?v=1.0&px=400)

## 如何安装

首先
下载并安装[最新版本的PowerBI Desktop](https://www.microsoft.com/zh-CN/download/details.aspx?id=58494)
下载[Microsoft Call Quality Power BI Connector](https://raw.githubusercontent.com/MicrosoftDocs/OfficeDocs-SkypeForBusiness/live/Teams/downloads/CQD-Power-BI-query-templates.zip)

解压出来就是这些文件，其中包括了7份模板与一个连接器文件。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608115631.png" style="zoom:50%;" />

接着创建文件夹，如下

```cmd
mkdir 'c:\Users\xxxxx\Documentesktop\Custom Connectors'
```

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608123346.png" style="zoom:50%;" />

打开刚刚安装好的PowerBI, 选择从Microsoft Call QualityMicrosoft Call Quality获取数据：

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608123737.png)

因为需要连接到Teams，所以会弹出身份验证，输入认证信息，如下：

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608123823.png" style="zoom:50%;" />

出现下图时，选择Direct Query即可，至此安装完毕。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608123920.png" style="zoom:50%;" />

## 如何配置？

基本上没有什么配置，安装完毕之后直接打开刚才下载的模板，即可连接到Teams并拉取数据到PowerBI里面了，如下图：

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608141227.png)

## Teams CQD模板列表

在这里，我们简单看看这几份默认的模板都有一些什么内容

### CQD Helpdesk Report.pbit
此报告集成了建筑和 EUII（End User Identifiable Information）数据，旨在让您从单个用户中钻取，以查找该用户呼叫质量差的根本原因（例如，用户位于遇到网络问题的建筑物中）。

其中的EUII数据包含IP address, MAC address, BSSID, user IDs, Machine Endpoint Name, the user’s verbatim feedback, and Object ID，并只能查询到最近30天的数据

所以通过这些EUII数据，这份模板就可以做出很多比较好玩的数据分析了

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608154620.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608154832.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608171133.png)

有没有发现Building栏位是空的，那么你可以一些位置与子网的对齐关系上传到Teams, 这样的话报表会更加直观

> 参考：[Add and update reporting labels](https://docs.microsoft.com/en-US/microsoftteams/learn-more-about-site-upload?WT.mc_id=TeamsAdminCenterCSH)
>
> csv模板：[sample template](https://github.com/MicrosoftDocs/OfficeDocs-SkypeForBusiness/blob/live/Teams/downloads/locations-template.zip?raw=true)

仔细阅读上面的参考文章，把制作好的csv上传到Teams即可

![image-20200612100233451](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200612100233451.png)



![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608154955.png)



![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608171153.png)

### CQD Location Enhanced Report.pbit
基于物理位置(用户的子网与会议室)的CQD报告。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608160040.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608171302.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608160746.png)

### CQD Mobile Device Report.pbit
提供专门针对移动设备用户的见解或数据分析，包括呼叫质量、可靠性和"我的呼叫率"。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608161350.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608161629.png)

### CQD PSTN Direct Routing Report.pbit
提供直接路由的 PSTN 呼叫的数据分析。要了解更多信息，请阅读[使用 CQD PSTN 直接路由报告](https://docs.microsoft.com/microsoftteams/CQD-PSTN-report)。  

![Screenshot: PSTN CQD report](https://docs.microsoft.com/zh-cn/microsoftteams/media/cqd-pstn-report1.png)

![屏幕截图： PSTN CQD 报表](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/cqd-pstn-report2.png)

![屏幕截图： PSTN CQD 报表](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/cqd-pstn-report3.png)

![屏幕截图： PSTN CQD 报表](https://docs.microsoft.com/zh-cn/microsoftteams/media/cqd-pstn-report6.png)

特别说明，通过这份报告我们可以拿到Direct Routing中三个非常重要的数据 Jitter, PacketLoss, Round Trip，这样我们就非常直观地知道我们的DR链路的质量情况了

**Jitter** – Is the millisecond measure of variation in network propagation delay  time computed between two endpoints using RTCP (The RTP Control  Protocol).

**Packet Loss** – Is a measure of packet that failed to arrive; it is computed between two endpoints.

**Latency** - (Also known as round trip time) is the length of time it takes for a  signal to be sent plus the length of time it takes for the  acknowledgment of that signal to be received. This time delay consists of the propagation times between the two points of a signal.

### CQD Summary Report.pbit
这份报告比较有用，可以整体上看到Teams Call的质量，你还可以在这份模板的基础上增加更加多的过滤条件，以进一步发现问题。

整体质量

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608164031.png)

整体可靠性（有没有发现这份报告的整体布局与上一份是一样的，但其实里面的只是简单换了一个度量值而已）

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608164359.png)

会议质量

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200608171457.png)

还有一报告，如P2P 质量，会议可靠性，P2P 可靠性，会议 RMC，P2P RMC，基本上都是换一下PowerBI中的度量值而已，在此就不多截图，大家自己安装来玩玩。

### CQD Teams Utilization Report.pbit

显示组织中的用户如何使用 Teams 以及使用多少。要了解更多信息，请阅读[使用 CQD Power BI 报告以查看 Microsoft 团队的利用率](https://docs.microsoft.com/microsoftteams/CQD-teams-utilization-report)。  

![屏幕截图：团队使用率报告](https://docs.microsoft.com/zh-cn/microsoftteams/media/cqd-teams-utilization-report1.png)

![屏幕截图：团队使用率报告](https://docs.microsoft.com/zh-cn/microsoftteams/media/cqd-teams-utilization-report9.png)

![屏幕截图：团队使用率报告](https://docs.microsoft.com/zh-cn/microsoftteams/media/cqd-teams-utilization-report11.png)

> *掉坑时间到了，*如果你对于这些模板还有还高的需求，那么你需要学习一下PowerBI，因为通过它你可以创造出更符合你要求的报表与数据。当然还可以借此作为跳板，分析你其它业务系统的数据呢，非常不错。

参考：

[Introducing Microsoft Call Quality Power BI Connector (aka CQD Power BI Connector)](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/introducing-microsoft-call-quality-power-bi-connector-aka-cqd/ba-p/1236863) 

[Introducing the Advanced Call Quality Dashboard](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/introducing-the-advanced-call-quality-dashboard/ba-p/972586) 

[Use Power BI to analyze CQD data for Microsoft Teams](https://docs.microsoft.com/zh-cn/microsoftteams/cqd-power-bi-query-templates)

