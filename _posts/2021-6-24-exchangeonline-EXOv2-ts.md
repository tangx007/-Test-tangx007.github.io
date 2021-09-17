---
layout:     post
title:      OAuth验证失败导致EXO V2模块连接时出错
subtitle:  M365 Powrshell 后台系列
date:       2021-06-24
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Powershell
- M365 PowerShell
---

# 使用EXO V2模块连接时的故障现象

对于Exchange online管理员来说，相信你会经常使用命令行的方式来进行日常的邮箱维护，之前连接到Exchange online的Powershell模块叫做EXO V1 （基于基本身份验证的连接方式），而从2020年开始微软已经慢慢地宣传另一种更加安全的连接方式：EXO V2的Powershell模块，这是一种基于OAuth 2.0的新式验证方式，安全，稳定，可靠。

但当你在尝试使用EXO V2来连接到Exchange online 时，可能会出现如下报错，本文将深入分析个中原因并由此加深理解这两种验证方式的异同。

![image-20210826001722888](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210826001722888.png)

其中里面有一段黄色字说明了原因：当Basic Auth关闭时，微软无法建立一个Remote PowerShell会话。

> You can't use other cmdlets as we couldn't establish a Remote PowerShell session as basic auth is disabled in your client machine. To enable Basic Auth  please check instruction here https://docs.microsoft.com/en-us/powershell/exchange/exchange-online-powershell-v2?view=exchange-ps#prerequisites-for-the-exo-v2-module

使用 winrm get winrm/config/client/auth 命令查询发现Basic authentication是false，Basic Auth关闭了。

![image-20210826000926283](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210826000926283.png)

# 解决方法

那么根据上面链接要求，我们发现解决方法其实很简单，只要运行 winrm set winrm/config/client/auth @{Basic="true"} 把Basic authentication打开即可正常连接

![image-20210826002349855](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210826002349855.png)

只要Basic = true，即可以连接到Exchange online

![image-20210826002413771](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210826002413771.png)

# 故障的根本原因分析

EXO V2 module 使用 Modern Auth来创建一个连接会话， 允许你使用基于REST-based的命令 (例如 *-EXO*). 同时EXO V2 module 向后兼容旧版本的Remote PowerShell 命令（即以前的Exchange online命令，如Get-Mailbox, Get-Recipient, Get-CASMailbox 等等），然后这些旧版本的命令需要使用WinRM技术来作为传输，而WinRM需要把Basic Auth启用，这又是为什么呢？原来EXO V2 模块在客户电脑是使用了Modern Auth作为认证方式，但在传输Modern Auth认证时产生的令牌（OAuth Token）后需要使用WinRM的Basic Auth来作为传输载体回传到Exchange online服务器，这样你才能顺利地使用旧版本的Exchange online命令而不会出现报错。

> 什么是WinRM？Windows Remote Management, Windows Remote Management (WinRM) is the Microsoft implementation of WS-Management Protocol, a standard Simple Object Access Protocol (SOAP)-based, firewall-friendly protocol that allows hardware and operating systems, from different vendors, to interoperate.
>
> The WS-Management protocol specification provides a common way for systems to access and exchange management information across an IT infrastructure. WinRM and Intelligent Platform Management Interface (IPMI), along with the Event Collector are components of the Windows Hardware Management features.
>
> 简单来说就是用于windows服务器远程执行命令的工具。WinRM2.0默认端口5985（HTTP端口）或5986（HTTPS端口）
>
> https://www.jianshu.com/p/2a78b343978d

简单归纳一下：

- EXO V2 Module 使用基于REST 的Modern Auth验证方式，直观感受是当你输入完Connect-ExchangeOnline后，会弹出来一个Web页面做身份验证，所以你的密码信息只会让AAD知道，而不是第三方软件。
- EXO V1 Module 使用基于WinRM的Basic Auth验证方式，这种旧的认证方式可能会把密码记录在第三方软件或电脑的缓存当中（例如 CMD窗口，Powershell窗口，$psw=Get-Credential变量），回想起来真的是极不安全。以前在做项目的时候，经常会把密码直接就写在代码里面了。
- 为了向后兼容，EXO V2 Module可以支持旧版本的Exchange online命令，但需要把OAuthToken通过WinRM的Basic Auth来传输给服务器。
- 所以，最后，无脑操作便是：只需要把WinRM Basic Auth打开即解决问题。

再多说一点点，如果不打开WinRM Basic Auth行不，当然可以但是你只能用9种 EXO* 类型的新命令，而其它的旧命令你就无法使用了，并得到如下报错：

```
WARNING:  Please note that you can only use above 9 new EXO cmdlets (the one with  *-EXO* naming pattern).You can't use other cmdlets as we couldn't  establish a Remote PowerShell session as basic auth is disabled in your  client machine.
```

![image-20210827221313478](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210827221313478.png)

# 相关参考

- [Basic Authentication and Exchange Online – February 2021 Update](https://techcommunity.microsoft.com/t5/exchange-team-blog/basic-authentication-and-exchange-online-february-2021-update/ba-p/2111904)
- [Connect to Exchange Online PowerShell - EXO V2 module](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:25%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

