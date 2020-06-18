---
layout:     post
title:      MS Teams 日常管理系列(2)
subtitle:  使用Teams PowerShell 批量增加用户
date:       2020-6-5
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Other
- 
---

> 在[MS Teams 日常管理系列1](https://blog.51cto.com/nemotan/2396571)中，介绍了Team Powershell 模块是如何对Teams团队的命令化快速管理的，这个模块主要用于你可以进行一些例行的自动化任务，以节省你的维护时间
>
> 今天这篇文章正是对这个Powershell模块的具体实践，希望可以抛砖引玉

一天，在微软Office 365技术群(O萌)微信群中，有这样一个问题/需求：

*A：请问各位大拿，往teams里面批量增加用户有对应的powershell可以操作么？*
*B：powershell组用户添加模块就行*
*A：有没有链接share一下啊？谢谢*
*B：https://docs.microsoft.com/en-us/powershell/module/teams/add-teamuser?view=teams-ps*
*A：谢谢，所以还是自己要写一个循环比如从一个excel表格里面读取用户名，然后反复运行这个命令对吧*
*A：这个普通用户可以往自己的团队里面用powershell来添加用户么？还是要IT admin？*
*X：teams后面对应的是 office group。一个teams就会有一个对应的office group。所以，往office group里加人就行了。*
*A：可以批量加入么？有界面可以直接操作么？*

详细研究了这个问题，如何批量对Teams团队的用户进行管理？会有以下几种方法

### 使用Teams团队手动增加成员

第一种方法，团队Owner直接在Teams团队中手动增加成员
但如果有很多的用户要加入团队的话，你的工作量就会变得很大。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200605161213637.png)

### Powershell批量增加成员

第二种方法是使用Add-TeamUser命令来批量增加用户

- 先创建这样一个csv，命名为addTeamsUser.csv

```xml
teamname,member
NemoTest,tangx_mtr2@ucssi.com
NemoTest,tangx_e5@ucssi.com
NemoTest,tangx_pbi@ucssi.com
```

- 安装并登陆到Teams Powershell模块，注意这个模块只能管理到Teams团队的内容，不能管理Teams后台的策略配置

```
Install-Module -Name MicrosoftTeams -Verbose
Connect-MicrosoftTeams
```

- 接着用以下脚本导入csv,并用循环来批量增加Teams团队用户

```
$temp = Import-Csv C:\addTeamsUser.csv
foreach($i in $temp)
{
	Get-Team -DisplayName $i.teamname | Add-TeamUser -User $i.member 
}

#你可以改进这一段脚本，例如自动创建不存在的团队
```

- 最后我们查看一下这个团队里面的用户，就增加出来

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200605171843.png" style="zoom: 80%;" />

### 使用O365 Group增加成员

第二是使用O365 Group来增加团队用户
其实当你创建一个Teams团队后，其实后台都会自动创建一个O365 Group，团队里面的成员就是Group里面的成员

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200605173100.png)

所以使用这种方法也可以向Teams团队里面增加用户，就看你的方便了

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200605173150.png" style="zoom:50%;" />

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />