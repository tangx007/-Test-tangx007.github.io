---
layout:     post
title:      MS Teams 会议的兼容性：让第三方视频会议终端加入会议
subtitle:  xxx
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

在现在的这个云视频会议2.0时代的开放性与兼容性是评价这个云视频会议平台的好坏标准之一，可以想像一下  一个封闭的会议平台使用自己独有的通讯协议，独有的会议终端，没有开发接口(API or SDK)  在现在这个竞争激烈的视频会议市场上，客户会选择这样一个成本高昂的产品吗？所以，有一部份Teams用户之前是使用其它品牌的会议终端（VTC，如Cisco SX,EX, DX；Lifesize, Polycom HDX, Group, Trio;  Sony）这些终端都不便宜，都需要利旧或等待折旧完毕直接报废，那么它们是如何融入到Teams Meeting当中的呢？

所以针对使用这种第三方的视频会议终端的Teams Meeting 场景，微软是有解决方案的；本文将使用 Bluejeans Teams  连接器来这些第三方视频会议终端，这是一个基于Azure的SaaS服务，为第三方会议终端为提供接入到Teams  Meeting的服务。这样不管你用什么终端都总能好好地接入到Teams Meeting了，兼容的设备列表请参考 *1

[![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s4.51cto.com/images/blog/202004/17/f29ba54c010b2ae3c6d1dfc50e5d8293.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

那么这种服务微软称之为**Cloud Video Interop (CVI),之前是支持三家厂商提供服务（Bluejeans, Polycom, Pexip），在2020年初多了一家Cisco: https://aka.ms/cisco-announcement**

先感受一下开启了这项服务之后的体验，一旦使用了Bluejeans的服务，在你组织的每一场Teams Meeting最下面都要带出Bluejeans 的会议接入地址与它的会议ID

[![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/123123123)](https://s4.51cto.com/images/blog/202004/17/5cbbf8bc83b78cc1e0101a2baf60db6b.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

我们只需要第三方的会议终端中呼叫这个地址即可连通Bluejeans, 输入会议ID后进行Teams Meeting

[![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\rdqwe)](https://s4.51cto.com/images/blog/202004/17/a04dfac27d611fa1c6cb7308865b49e0.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

这是我用的第三方设备，Polycom Group

[![273a0c2e456639627955427f83d70228_](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/3rd11111)](https://s4.51cto.com/images/blog/202004/17/d815fd558b025bcb414586956a4a970d.jpg?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)



### 那如何配置上去的呢？

因为这是SaaS的服务，所以我们还是要有所准备：

1）你当然需要有Teams，同时你是Teams的管理员。

2）你手上有第三方的视频会议终端需要接入到Teams Meeting.

3）需要向Bluejeans申请一个测试许可（找我们）

4）开通防火墙端口

IP Addresses: 40.65.174.101, 40.91.78.170, 52.142.237.32, 52.183.16.252,  52.183.19.47, 52.183.22.7, 40.65.172.32/27, 40.91.116.192/27,  52.236.151.32/27.

- H.323 Room System 
  - Outbound TCP Port 1720 | H.255 Signaling for H.323
  - Outbound TCP Ports 5000 - 5999 | H.245 Call Control for H.323
  - Outbound UDP Ports 5000 - 5999 | RTP Media
- SIP Room System 
  - Outbound TCP Port 5060 | SIP Signaling
  - Outbound TCP Port 5061 | SIP (TLS) Signaling
  - Outbound UDP Ports 5000 - 5999 | RTP Media



一旦我们的测试许可申请下来，会有这样一封邮件

[![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/3rd222222)](https://s4.51cto.com/images/blog/202004/17/efbe4f945079ab581050c6f7eeb84e95.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

### 接着就可以部署下去了，第二步：Bluejeans 网关的权限

因为Bluejeans需要访问O365上面的数据，所以需要做一个授权：以O365 Admin身份进入[Consent Form](https://login.microsoftonline.com/common/adminconsent?client_id=363e7c1b-ca85-4765-bd62-f77a096c407d&state=12345&redirect_uri=https://bluejeans.com) ，并接受如下：

[<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/3rd3333333" alt="clip_image002" style="zoom:50%;" />](https://s4.51cto.com/images/blog/202004/17/3270c827415af059db4006472ecd8790.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

如果同步成功,您将被重定向到 BlueJeans 主页：

[<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/3rd44444" alt="clip_image004" style="zoom:50%;" />](https://s4.51cto.com/images/blog/202004/17/b229833b36790903e171cd78d3ad315a.jpg?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

### 第三步，在Teams PowerShell上面配置Bluejeans 服务

按顺序运行以下 PowerShell 命令

- 启动 PowerShell 会话

```
$cred = Get-Credential YourEmailAddress@YourDomain.onmicrosoft.com
$session = New-CsOnlineSession $cred
Import-PSSession $session -AllowClobber
```

[![clip_image002](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/3rd555555)](https://s4.51cto.com/images/blog/202004/17/19b5f21fa51538cebe622075946569ad.jpg?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

- 检查是否已经存在VIS服务

Get-CsTeamsVideoInteropServicePolicy

现在支持四家的CVI: 

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/rd111111132f45e21b00e863b1bba10a31f2fb33b.jpg" style="zoom: 50%;" />

**#** **设置** **BlueJeans** **服务并确认其设置正确（申请的邮件有TenantKey）**

New-CsVideoInteropServiceProvider -Name BlueJeans -TenantKey 169xxx631@teams.bjn.vc -InstructionUri https://support.bluejeans.com/knowledge/vtc-dial-in-options-for-teams

Get-CsVideoInteropServiceProvider

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/rd2222239683514e6685cb5490ebebba6c60529.jpg)

### 第四步，为用户启用Bluejeans服务

以下两个命令块用于为单个用户或整个租户启用服务

**# ENABLE USER –** **单个用户启用**

Grant-CsTeamsVideoInteropServicePolicy -PolicyName BlueJeansServiceProviderEnabled -Identity UserEmailAddress@YourDomain.onmicrosoft.com

Get-CsOnlineUser -Identity UserEmailAddress@YourDomain.onmicrosoft.com | fl UserPrincipalName,TeamsVideoInteropServicePolicy

**# ENABLE ALL –** **全局启用**

Grant-CsTeamsVideoInteropServicePolicy -PolicyName BlueJeansServiceProviderEnabled -Global

Get-CsTeamsVideoInteropServicePolicy

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/rd333339b21dfd214c2c3ee1c415a562e051096.jpg)

**#****如何测试？**

当VideoInteropServicePolicy成功分配到用户上面后，可以通过在 Microsoft [Teams 会议邀请中查找附加的 BlueJeans 信息](https://support.bluejeans.com/knowledge/vtc-dial-in-options-for-teams)来测试服务，如下：

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/rd444470c726dfd84385419cf0c047491e3fbf.jpg)

[![clip_image006](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/rd55555555)](https://s4.51cto.com/images/blog/202004/17/89c30c641d020677d3e8551e414e67c4.jpg?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)





