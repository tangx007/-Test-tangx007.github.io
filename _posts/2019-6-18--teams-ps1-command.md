---
layout:     post
title:      MS Teams 日常管理系列(1)
subtitle:  Teams PowerShell 命令详解
date:       2019-06-18
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Other
- 
---



微软在5月1号发布的针对Teams 团队应用管理的命令模块：Microsoft Teams - PowerShell Module ，它可以让IT管理员针对Teams中的团队应用进行管理，也只针对团队应用而已，即如下应用:

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r11111c567e1df56b770ec7739c41a00c4acc7.png)

因为对于Teams，还有会有另外的一个模块进行Teams Policy & Config的管理（Skype for  Business PowerShell Module），千万不能认为本次发布的Teams PowerShell是什么都能做的,  所以区分并汇总如下：

•   Microsoft Teams PowerShell  Module：用于Teams团队应用的增删改，频道成员的增删改等管理；同时作为IT专业人员，PowerShell对Microsoft  Teams的支持使您可以做到Teams管理的自动化，使团队管理更容易：

> 1)  IT管理员就可以帮助大量业务用户快速生成/删除所需的团队（即团队的生命周期管理），同时若应用到OA系统，则可进行Teams团队的自动化管理了；
>
> 2)  自动配置新团队，新频道，添加成员以及设置图片和成员权限等选项。
>
> 3)  创建一个自助式服务的网站，在后端使用Teams PowerShell进行管理。例如，最终用户则可以轻松创建团队，用户浏览网站表单以创建团队，PowerShell可以检查具有重复名称的团队，以确保用户不会创建具有相同名称的团队。
>
> 4)  如果我需要向团队添加大量成员，使用PowerShell我可以从.csv批量添加这些成员。
>
> 5)  标准化每个创建的团队中的设置。

•   Skype for Business PowerShell Module：用于Teams的各种策略的增删改，各种全局的配置，即Teams Admin Portal中所有的设置都可以使用本命令来设置。

回归正题，我们可以通过Powershell命令即可安装Teams PS Module了。我们通过Install-Module -Name MicrosoftTeams 来自动安装本命令，非常的容易以至于我简化成以下安装截图：

```
Install-Module -Name MicrosoftTeams -Verbose
```

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r222221692e7560414d18886d1daaf46258171.png)

使用Connect-MicrosoftTeams 连接到Teams,就会显示如下内容：

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r333338d18fa487f15cff4b14b9d111b4ae5f8.png)

您现在可以开始运行cmdlet或针对Microsoft Teams的脚本！请注意，您可以随时键入 Get-Command -Module MicrosoftTeams 以查看可用命令的完整列表：
在写本Blog的时候，有如下这些管理命令，相信以后会有更多更好的命令供大家使用(主要分为几大类：Get-TeamXXX, New-TeamXXX, Remove-TeamXXX, Set-TeamsXXX)

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r444446a68c5467c6d2f96d1cfbf65a1018378.png)

PS. 在还没有发布之前的0.9.0版本是比现在的1.0.0多出很多关于Setting的命令，不知道基于什么原因没有释放到GA版本下面，一起期待更新吧。

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r55555b5304b836c292326acf24dd40a6701ad.png)

接下来我们演示一下为中国深圳的员工创建一个新的私有团队。输入以下命令，然后按Enter键：

```
New-Team -DisplayName “CN-ShenZhen” -Visibility Private
```

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r66666f79a228728b63cb13f769bf7650bd6b6.png)

还可以把团队改为公共团队，配置还可以实时生效，如下：

```
Get-Team -DisplayName "CN-ShenZhen" | Set-Team -Visibility Public
```

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r77777b185f76d617729317c515345bee0e2cb.png)

接下来是增加团队成员，注意为了方便在以下命令中我们使用管道符“|”来指定需要操作的团队，不然你就要在命令中指定很长的GroupId，而且也不容易记住。
`Get-Team -DisplayName “CN-ShenZhen” | Add-TeamUser -User tangx@xxxxxx.com`

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/329ca999dbde0750addd80ab1f8a2e3c.png)

创建出来的团队应用之后，我们还可以通过Set-Team来修改各种设置（也许这就是上文没有把Team Setting单独列出的原因）

下图所示，使用 “-”来自动带出所有支持的参数/设置：

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r999999a9b94dbeb64af5e931b1f34aa913b0e8.png)

以下列举各命令的用法，供参考：

```
Set-Team
   -GroupId <String>
   [-DisplayName <String>] 显示名称
   [-Description <String>] 描述
   [-MailNickName <String>] 
   [-Classification <String>]
   [-Visibility <String>] 设置团队是否为公共还是私有团队，Public or Private
   [-AllowGiphy <Boolean>] 允许GIF表情
   [-GiphyContentRating <String>]
   [-AllowStickersAndMemes <Boolean>] 表情开关
   [-AllowCustomMemes <Boolean>] 自定义表情开关
   [-AllowGuestCreateUpdateChannels <Boolean>] 用于确定访客是否可以在团队中创建与更新频道
   [-AllowGuestDeleteChannels <Boolean>]用于确定访客是否可以在团队中删除频道
   [-AllowCreateUpdateChannels <Boolean>] 用于确定是否允许成员（而不仅仅是所有者）是否可以管理团队中的频道。
   [-AllowDeleteChannels <Boolean>] 用于确定是否允许成员（而不仅仅是所有者）删除频道。
   [-AllowAddRemoveApps <Boolean>] 用于确定是否允许成员（不仅是所有者）向团队添加应用程序。
   [-AllowCreateUpdateRemoveTabs <Boolean>] 用于确定成员（而不仅是所有者）是否可以管理频道中的Tabs.
   [-AllowCreateUpdateRemoveConnectors <Boolean>]用于确定成员（而不仅仅是所有者）是否可以管理团队中的连接器。
   [-AllowUserEditMessages <Boolean>]用于确定成员是否可以编辑已发布的消息。
   [-AllowUserDeleteMessages <Boolean>] 用于确定成员是否可以删除已发布的消息。
   [-AllowOwnerDeleteMessages <Boolean>] 用于确定访客是否可以在团队中创建与更新频道
   [-AllowTeamMentions <Boolean>]确定是否可以@提及整个团队的设置（这意味着将通知所有用户）
   [-AllowChannelMentions <Boolean>] 用于确定是否可以@提及团队中的频道，以便通知所有关注该频道的用户。
```

接下来是创建团队中的频道，也是非常简单的几行命令，而且都是有规律的：

```
get-Team -DisplayName “CN-ShenZhen” | New-TeamChannel -DisplayName "ShenZhen Tech Dept"
get-Team -DisplayName “CN-ShenZhen” | New-TeamChannel -DisplayName "ShenZhen Sales Dept"
get-Team -DisplayName “CN-ShenZhen” | Get-TeamChannel
```

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r-10bd5f9115577f6d5f4272eeb315abd94b.png)

接下来介绍修改团队频道的命令：Set-TeamChannel，相比于Set-Team来说这个命令比弱一点，只能修改频道的名字，其它什么都做不了，如下命令：

1）  通过管道符需要修改的频道所属的团队

2）  指定当前频道的显示名字

3）  指定要修改的频道的显示名字

```
Get-Team -DisplayName "CN-ShenZhen" | Set-TeamChannel  -CurrentDisplayName "ShenZhen Tech Dept" -NewDisplayName "ShenZhen Tech  Support Center"
```

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r-11271e16824162f5d8f501ff8e565bd5ec.png)

最后，本文介绍了几个比较常用的Team团队管理的命令如（Connect-MicrosoftTeams, Get-Team, Add-TeamUser, Set-Team, Set-TeamChannel….）其它命令读者可自行尝试也是一件非常容易的事情。

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r-1230c6ea1a63fe18bee874e4530bb9ccdb.png)

最重要的是我们可以通过Github查看本模块里面的源代码，Team  Powershell是通过RESTful技术来调用了非常强大的Microsoft Graph API来对Teams进行设置/配置。通过Team  Powershell里面的源码，可以学习到Microsoft Graph是如何连接到M365, 如何连接到Teams,  如何对Teams进行设置等一系列的功能。

期待下一篇的文章吧：“如何使用Team Powerhsell来学习Microsoft Graph”

![MS Teams 日常管理系列1：Teams PowerShell 命令详解](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/r-135c991244421c507cd46227fcd97ae172.png)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />