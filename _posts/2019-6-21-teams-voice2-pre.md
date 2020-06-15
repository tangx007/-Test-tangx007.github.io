---
layout:     post
title:      Microsoft Teams Voice语音落地系列-2 
subtitle:  实战-前置条件准备
date:       2019-6-21
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

> 上一章中，我们了解在Teams中使用Direct Routing进行本地语音落地的一些架构与方法。

磨刀不误砍柴工，在本章节中我会把一些基本要用到的Teams语音落地的所有前置条件与准备技术要点都作一个具体的说明，以下为主要参考地址：https://docs.microsoft.com/en-us/MicrosoftTeams/direct-routing-plan#

首先大致列出需要准备的东西 

1) 需要用到的权限 

2）连接到Skype online Powershell 

3) 连接到office  365 Powershell 

4) 注册并激活域名 

5) 准备Session Boarder Controller的对外公网IP与公网FQDN  

6) 公网证书 

7) 防火墙规则要求 

8) Session Boarder Controller的固件版本要求 

9) 本地Session  Boarder Controller/GW已经与PSTN对接成功 

10）E3+Phone System许可；或E5许可，如下图所示：

![image-20200613152048758](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152048758.png)

简单一点的就是为了建立一条像这样的SIP路径：

> Microsoft Teams/Phone System <----tls sip trunk---->Local Session Boarder Controller<---sip/pri--->PSTN

接下来，我们一条一条的过一下：

0.20191209 增加一条重要的前置条件：需要Teams only模式

### ![image-20200613152431967](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152431967.png)

### A.  各种权限要求

首先确保以下管理权限都已经要到手了，或者管理员在旁边协助，不然的话配置的时候各种不顺利。

- 团队通信管理员 – Direct Routing配置
- Session Boarder Controller管理员 – Session Boarder Controller配置 （这里的前提是您已经有一台本地的Session Boarder Controller）
- Skype for Business 管理员 –  Skype for Business to Teams用户迁移

### B.  许可要求

Direct Routing的话是要有许可要求的，具体可以参考如下图片：

- E3+Phone System or E5

- 如果你需要有会议接入号与在会议中呼叫电话用户，那么你需要外加一个Audio Conferencing的许可

- 当然如果你不需要BYOT, 不需要沿用自己的电话号码，那么微软有更简单的解决方案，就是Phone System + Calling  Plan的方案，你只需要在E3/E5基础上购买这两个许可，那么微软直接就向你提供电话服务，你只需要做的事情是给钱买许可与分配电话号码即可使用。

  ![image-20200613152514402](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152514402.png)
  
### C.  连接到 Skype for Business Online Powershell

在配置Teams Voice的过程中，基本上大部份的操作都需要使用命令进行，所以还是老老实实用命令吧，如下：

- 用管理员权限打开Powershell

- 安装SkypeOnlineConnector， https://www.microsoft.com/en-us/download/details.aspx?id=39366

- 以管理员身份打开Powershell

- 运行以下命令连接到 Skype for Business Online Powershell. (代码托管在Github)
  
  ![image-20200613152623951](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152623951.png)
  
### D.  连接到Office 365 Powershell

1)  安装Microsoft Online Services Sign-In Assistant for IT Professionals RTW

- https://www.microsoft.com/en-us/download/details.aspx?id=41950
  2)  安装 Windows Azure Active Directory 模块

- 以管理员身份打开Powershell

- 运行Install-Module MSOnline

- 运行 Connect-MsolService ，输入office365管理员帐号与密码

- 如下图操作即可连接到O365
  ![image-20200613152734774](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152734774.png)

- 如果你要连接到其它版本的office365, 可参考下图：
  
  ![image-20200613152759262](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152759262.png)

### E.  注册并激活域名后缀

这个注册的域名是后面配置Session Boarder Controller FQDN的时候要用到的，如果你已经有一个已经添加在O365上的默认企业域名（如contoso.com），那么下面的要求就可以检查一下即可：

- Session Boarder Controller FQDN的域名必须要在O365上面有注册，如下图：
  

![image-20200613152900816](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152900816.png)

- 注意: 新增加的域名需要等待约2小时左右才能查询出来，命令为：Get-CsTenant | fl Domains

- （保守起见）再新建一个Session Boarder Controller FQDN相同的域名，如下图。

- 激活域名：为新增加的域名建立一个用户并分配一个Enterprise许可（E1 E2 E3）
  ![image-20200613152935602](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152935602.png)
  
  如果你的Session Boarder Controller 公网FQDN并不是公司默认的域名，而是一个全新的域名的话，你必须要进行激活域名的操作，如下图：
  https://docs.microsoft.com/en-us/microsoftteams/direct-routing-sbc-multiple-tenants
  
  ![image-20200613152956256](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613152956256.png)
  
  如果不激活的话，会有以下报错，并且让你不知道什么回事：
  
  ![image-20200613153010448](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153010448.png)
  
  注意：不能使用*.onmicrosoft.com这种域名，原因其实很简单，就是这个不是你的自有域名，无法指定A记录到Session Boarder Controller的公网IP，如下图：
  
  ![image-20200613153028642](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153028642.png)

### F.  准备Session Boarder Controller的公网IP与公网FQDN，并新增至公网A记录

在Teams Phone System与本地Session Boarder Controller建立的SIP Trunk需要把信令与媒体流都加密，所以需要公网FQDN，以便等下我们把这个FQDN增加到公网证书里面。

![image-20200613153041109](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153041109.png)

### G.  公网证书要求

- 你必须有一张包含Session Boarder Controller FQDN域名的公网证书，用于Microsoft Phone System与本地Session Boarder Controller建立加密的SIP Trunk时使用。
- 必须使用认证的第三方公网证书提供商
- 可以使用通配符证书，如*.contoso.com (但只匹配 xxx.contoso.com, 不匹配 test.xxx.contoso.com
- 这里推荐使用Godaddy的证书：1）便宜 2）有中文服务 3）技术支持给力 4）24小时服务

https://docs.microsoft.com/en-us/MicrosoftTeams/direct-routing-plan#public-trusted-certificate-for-the-sbc

### H.  防火墙规则要求

因为我们的本地Session Boarder Controller需要暴露在Internet上面，所以必须要把它放在对外防火墙的后面，或者直接放在DMZ区。
如下规则：

![image-20200613153146781](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153146781.png)

注意一下：

- 红色字体的是可以根据需要更改的，在Session Boarder Controller上面。
- 黑色字体的是Phone System上面要求的，不能更改。
- 其中 xxx.pstnhub.microsoft.com 就是传说中的Phone System FQDN了
- 这些规则包含了TCP/IP的五元组信息：源IP地址、目的IP地址、协议号、源端口、目的端口

### I.  Session Boarder Controller的固件版本要求(支持媒体旁路)

对于Ribbon/Sonus SWeLite至少要在705以上, 并最好使用最新的FW:支持媒体旁路，这是一个很重要的功能当你的UC系统在云上的时候，所有的媒体流量都不直接经过o365, 而是直接路由到本地的语音网关（可以回顾一下之前的[架构与部署简述中的媒体旁路](https://blog.51cto.com/nemotan/2377504)）。

具体的认证Session Boarder Controller，如下图：
https://docs.microsoft.com/en-us/MicrosoftTeams/direct-routing-border-controllers

### J. 最后就是你本地的Session Boarder Controller已经与PSTN网络对接成功

为什么会把这个这么重要的信息放在最后面呢，是因为：
1）一般情况下，你的内网已经有一台语音网关与PSTN对接，并已在用了。
2）Sonus/Audiocodes的Session Boarder Controller或语音网关对其它设备的兼容性很好，不管对接什么类型的语音网关（pri / FXO / sip）都能很好地对接
3）退一万步，若只是作为测试之用，您也可以部署测试版Session Boarder Controller，只要抓包可以看到正确的信令在传输，Session Boarder  Controller也有SIP消息头修改的能力，帮忙你应对各种问题。（后面会专门有一节，专门介绍如何快速在Azure上面部署Session  Boarder Controller）

### 最后总结一下

1）本节主要列举了实施Teams Direct Routing前期的所有资源准备。
虽然有点多，但是该跳的坑，该填的坑，我都列出来了，路是平坦的，就看同学们要不要向前走了。

2）接下来，会有以下内容介绍给大家：

- 配置与管理Teams Calling的拨号计划
- 配置Teams语音路由与SIP Trunk
- 配置本地Session Boarder Controller与Teams Direct Routing的配对 
  （这部份估计已超出同学们的知识界限了，实在不行只能找合作伙伴了）
