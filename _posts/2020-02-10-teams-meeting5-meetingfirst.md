---
layout:     post
title:      微软 Teams Meeting 系列(5) 
subtitle:  让Skype 用户尝鲜使用Teams Meeting
date:       2020-02-10
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

> 本文介绍了一种Teams的升级模式：SfBWithTeamsCollabAndMeetingsWithNotify 
>
> 基于MS Ignite 2019 MTG40 Meeting First: Microsoft Teams meetings for Skype for Business Server customers的课程视频整理而成的。

关于Teams的Meeting First, 就是让Skype用户在迁移到Teams的过程中，首先把Meeting的功能迁移到Teams上面，并完全使用Teams Meeting 来进行会议。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb21.png)

在本文中，大家会学习到以下这些点：

1. 为什么需要Teams Meeting First? 它是否适合你使用？
2. 当使用了Meeting First之后，Teams and Skype 客户端会有什么变化与体验？
3. 部署Meeting First的一些要求简介。

首先，Meeting First是否适合你使用 ，因为Skype Meeting与Teams Meeting是两个不同的会议系统，所以在它们俩上面，我们现在遇到的挑战是：

- 客户想把会议系统迁移到Teams上面，但在企业本地即还是想沿用现有的SFB系统（包括Skype Meeting, Skype Voice）而Skype Voice却是没有这么简单地迁移到Teams上面，为此我也写过一个专门的系列来说明如何让Teams可以有PSTN语音的能力，请参考 *1。
- SFB的前期投资，将会因为Teams的使用而浪费（例如系统部署，硬件设备投入，项目管理，语音网关投入, etc…）。
- 突然之间上马Teams 的话，会与其它在运行中的项目冲突，造成资源不足。
- 企业语音已经在SFB上面好好地运行了。

那为什么又需要Teams Meeting First 呢？我的理解有如下：

- 因为Skype for Business Meeting 与Teams Meeting 是不能共存的，就是 Skype Meeting 不等于 Teams Meeting，你需要两者选其一（如果你是微软系的话）
- SFB User不能加入Teams Meeting, Teams User不能加入Skype Meeting。
- 微软的大趋势是最终淘汰SFB, 完全使用完全基于Azure Cloud的Teams。
- 需要更加好的入会体验与会议质量，那么可以尝试使用一下这个基于Skype online 与Skype consumer 改进过来的云视频会议系统。
- 相对于Skype来说，Teams Meeting有更加完善的会议生命周期（会前，会中，会后），请参考*2
- 如果你也计划着把Skype迁移到Teams的话，可能需要一步一步来把特定功能先迁移到Teams, 而不是一刀切的话。
- 适合一些已经部署的SFB，并且使用得好好的企业，但他们需要尝试使用Teams。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb20.png)

接着，一旦决定使用了Teams Meeting First 之后, 从客户端的角色来看，会有如何的变化与体验呢？

1. 云视频会议将会从之前的Skype Meeting转成 Teams Meeting 。
2. 参加会议的体验（入会体验）将使用Teams的模式，包括之前的Room System , SFB设备，3rd的视频会议设备。
3. 在Skype方面，从之前的Calling, IM, Presence, Meeting, Collaboration, Enterprise Voice ，变成只承载Calling, IM, Presence, EV，而Teams 则承载着Meeting与Collaboration 的功能。
4. 用户之前已经预约好的Skype Meeting将会自动使用Meeting Migration Service (MMS) 服务把它们迁移至 Teams Meeting，以至于大家都可以统一使用Teams Meeting，请参考 *3
5. Skype 与 Teams, 将各自各的独立地运行，而没有功能上面的冲突。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image16_thumb5.png)

最后一个话题是关于Meeting First的部署（非常简单），但我们首先来简单说明一下从Skype 迁移到Teams 的几种模式



1）第一种是使用Island Mode, 岛屿模式，这是微软推荐的使用模式并且也是默认的模式，可以让Skype与Teams两者独立的工作，所以它们俩就有会一些功能重叠在一起让用户在使用上面造成困惑了, 例如

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb9.png)

2）第二种是使用Skype for Business w/ Collaboration & Meetings Mode （本文就是重点讲述这种模式来尝鲜使用Teams Meeting）

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb11.png)

3）第三种是 Teams Only Mode，一旦使用本模式后Skype 将会无法使用，并将会把所有功能迁移至Teams, 这也是微软的最终目的

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb15.png)

下图把三种主要的模式做了一个汇总，希望能否更加深入地了解各种迁移路径：

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb23.png)

设置Meeting First非常简单，登陆Teams Admin Center, 如下图把Teams共存模式改为Skype for Business 与 Teams协作和会议 即可，值得注意的是：

1. 只有单独为用户分配 Teams only or SfBWithTeamsCollabAndMeetings 模式后，才会触发MMS服务来自动迁移Skype Meeting到 Teams Meeting.
2. MMS只适用于Exchange online的 Teams User. 请参考* 3
3. MMS迁移过程至少需要90分钟到120分钟左右，请参考 *4

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image19_thumb.png)

使用Powershell命令：

```
#登陆到Teams Powershell
$String = "xxxxxx"
$username = "xxxxxx@demo.onmicrosoft.com"
$TenantDomain = "demo.onmicrosoft.com"
Import-Module SkypeOnlineConnector;$PWord = ConvertTo-SecureString -String $String -AsPlainText -Force;
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $username, $PWord;
$CSSession=New-CsOnlineSession -credential $Cred -OverrideAdminDomain $TenantDomain
Import-PSSession $cssession –AllowClobber

#查询一下用户的Teams共存模式
Get-CsonlineUser tangx@ucssi.onmicrosoft.com | fl *teamsupgrade*
```

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb25.png)

```
#为用户分配SfBWithTeamsCollabAndMeetingsWithNotify 模式
Grant-CsTeamsUpgradePolicy -Identity tangx@ucssi.onmicrosoft.com -PolicyName SfBWithTeamsCollabAndMeetingsWithNotify
```

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\image_thumb28.png)

以下为全局修改：

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfiles95823B/image22_thumb1.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfiles95823B/image222.png)



最后，一旦共存模式改成SfBWithTeamsCollabAndMeetingsWithNotify 之后，大家可以来看看之前的Skype Meeting 就会通过MMS迁移到Teams Meeting

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfiles95823B/image_thumb37.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfiles95823B/image67.png)



\# 通过命令可以强制迁移Skype Meeting, 并且查看状态。可以看出迁移时间还是比较久的。

Start-CsExMeetingMigration -Identity tangx@ucssi.onmicrosoft.com -TargetMeetingType Teams –Verbose

Get-CsMeetingMigrationStatus -Identity [tangx@ucssi.onmicrosoft.com](mailto:tangx@ucssi.onmicrosoft.com)

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfiles95823B/image_thumb38.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfiles95823B/image68.png)



参考链接：

*1 Microsoft Teams Voice语音落地系列: https://blog.51cto.com/nemotan/2420875

*2 Teams Meeting 会议生命周期中的功能更新: https://blog.51cto.com/nemotan/2467152

*3 Updating meetings when assigning TeamsUpgradePolicy https://docs.microsoft.com/en-us/skypeforbusiness/audio-conferencing-in-office-365/setting-up-the-meeting-migration-service-mms#updating-meetings-when-assigning-teamsupgradepolicy

*4 How MMS works https://docs.microsoft.com/en-us/skypeforbusiness/audio-conferencing-in-office-365/setting-up-the-meeting-migration-service-mms#how-mms-works

