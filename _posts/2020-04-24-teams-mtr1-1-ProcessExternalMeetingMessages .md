---
layout:     post
title:      玩转Microsoft Teams Room系列(1-1)
subtitle:  允许外部用户预约到MTR
date:       2020-04-24
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

当你部署完Microsoft Teams Room之后，你会发现这间会议只能让内部的用户预约上。

但如果你想让你的外部供应商预约MTR会议室，并自动显示在MTR控制屏上面，是做不到的（当然这是安全原因）。

那么只能手动地登陆到MTR的Outlook上面，手动接受外部会议邀请了。

那如果做到自动接受会议邀请呢，非常简单。

首先登陆到Exchange online Powershell，如下

```
$cred = Get-Credential tangx@ucssi.com
$sess = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $cred -Authentication Basic -AllowRedirection
Import-PSSession $sess –DisableNameChecking
```

查询一个参数ProcessExternalMeetingMessages，就是它来控制自动接受外部的会议邀请：

```
Get-Mailbox tangx_mtr1@ucssi.com | Get-CalendarProcessing | Select *external*
```

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/extimage_thumb10.png)

所以，我们直接把这个参数改成True就可以了：

```
Get-Mailbox tangx_mtr1@ucssi.com | Set-CalendarProcessing -ProcessExternalMeetingMessages $true
```

最后就可以被外部用户预约上了：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/extimage_thumb8.png)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />



