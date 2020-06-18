---
layout:     post
title:      微软 Teams Meeting 系列(6) 
subtitle:  使用MMS迁移Skype Meeting
date:       2020-02-10
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

我认为MMS服务对于Teams Meeting来说比较重要，因为除了全新的Teams用户，否则其它Skype onpremise or Skype online 用户迁移到Teams都会遇到一个问题：“我之前预约好的Skype Meeting怎么办，迁移到Teams之后还能使用吗？” ，这里微软提供了一个解决方案：Meeting Migration Service, 它在这些场景下面来更新/迁移 用户现有的会议（特别是Skype Meeting）

- 当用户从本地迁移到云时（无论是到Skype for Business Online还是到TeamsOnly）。
- 管理员更改用户的音频会议设置时（例如为一个用户分配音频会议许可，他之前的预约好的Teams Meeting 就需要把会议拨入号增加到预约信息里面，MMS就可以自动做到）
- 当在线用户仅升级到Teams时
- 当用户的TeamsUpgradePolicy中的用户模式设置为SfBwithTeamsCollabAndMeetings时，会自动触发MMS进行更新
- 当您使用PowerShell cmdlet时，Start-CsExMeetingMigration （即手动迁移触发）

*但是在以下场景下面，是不能使用会议迁移服务MMS的：*

- *用户的邮箱在本地的Exchange 当中，因为MMS必须使用 Exchange online。*
- *用户从 Teams 迁移至 本地部署的Skype 服务器当中。*

*在这种情况下，最终用户可以使用会议迁移工具来迁移自己的会议，请参考 \*1* 

### MMS如何运作的呢？

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/061610d82eafeebc7610b4c313f915d64ec6.png)

1. MMS 在该用户的Exchange邮箱中搜索该用户的所有会议。
2. 根据用户邮箱中的信息，MMS 找到用户的Skype Meeting or Teams Meeting.
3. 在电子邮件中，MMS 将替换会议详细信息中的会议信息替换，例如替换成Teams的会议信息/链接。
4. MMS 会代表会议组织者将该会议的更新版本发送给所有会议接收者，之后，所有的与会者将在相关的会议更新。

以下为一个从Skype Meeting 迁移至 Teams Meeting后的效果，如下：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/260c7f4d555b0a5ad385b40ad21b72c7.png)

### 注意的地方

使用MMS迁移服务需要注意的地方：

1. 从触发MMS的时间开始，通常需要大约2个小时才能迁移用户的会议。
2. 会议迁移后，MMS会替换在线会议信息块中的所有内容。
3. MMS 仅迁移那些通过Outlook Add in生成的Skype or Teams 会议。
4. 与会者超过250人（包括组织者）的会议将不会被迁移。（因为Teams Meeting 最大支持250方与会者）
5. 只有当你单独为用户的TeamsUpgradePolicy分配SfBWithTeamsCollabAndMeetings or TeamsOnly 模式后，才会自动触发MMS。
6. MMS迁移过程至少需要90分钟到120分钟左右。

![image19_thumb](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image19_thumb_thumb1.png)

### 使用Powershell迁移

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

![image_thumb25](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb25_thumb1.png)

```
#为用户分配SfBWithTeamsCollabAndMeetingsWithNotify 模式
Grant-CsTeamsUpgradePolicy -Identity tangx@ucssi.onmicrosoft.com -PolicyName SfBWithTeamsCollabAndMeetingsWithNotify
```

![image_thumb28](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb28_thumb.png)

最后，一旦共存模式改成SfBWithTeamsCollabAndMeetingsWithNotify 之后，大家可以来看看之前的Skype Meeting 就会通过MMS迁移到Teams Meeting

![image_thumb37](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb37_thumb1.png)

```
# 通过命令可以强制迁移Skype Meeting, 并且查看状态。可以看出迁移时间还是比较久的。

Start-CsExMeetingMigration -Identity [tangx@ucssi.onmicrosoft.com]() -TargetMeetingType Teams –Verbose
Get-CsMeetingMigrationStatus -Identity tangx@ucssi.onmicrosoft.com
```

![image_thumb38](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb38_thumb1.png)

除了必需的`Identity`参数外，它还有两个可选参数`SourceMeetingType`和`TargetMeetingType`，它们允许您指定如何迁移会议：

**TargetMeetingType：**

- 使用`TargetMeetingType Current`指定将Skype for Business会议保留为Skype for Business会议，将Teams会议保留为Teams会议。但是，音频会议的坐标可能会更改，并且任何本地Skype for Business会议都将迁移到Skype for Business Online。这是TargetMeetingType的默认值。
- 使用`TargetMeetingType Teams`指定将任何现有会议都必须迁移到团队，而不管该会议是在Skype for Business中在线还是在本地托管，并且无论是否需要任何音频会议更新。

**SourceMeetingType：**

- 使用`SourceMeetingType SfB`表示仅应更新Skype for Business会议（本地会议还是在线会议）。
- 使用`SourceMeetingType Teams`表示仅应更新团队会议。
- 使用`SourceMeetingType All`表示应更新Skype for Business会议和团队会议。这是SourceMeetingType的默认值。

列举一些常用的命令：

```
#以下命令将返回有关2018年10月1日至2018年10月8日发生的所有迁移的完整详细信息。
Get-CsMeetingMigrationStatus -StartTime "10/1/2018" -EndTime "10/8/2018"

#以下命令将返回用户ashaw@contoso.com的状态：
Get-CsMeetingMigrationStatus -Identity [ashaw@contoso.com](mailto:ashaw@contoso.com)

#以下命令以获取受影响的用户列表以及所报告的特定错误：
Get-CsMeetingMigrationStatus| Where {$_.State -eq "Failed"}| Format-Table UserPrincipalName, LastMessage

#要查看您的组织是否启用了MMS
Get-CsTenantMigrationConfiguration

#完全启用或禁用MMS
Set-CsTenantMigrationConfiguration -MeetingMigrationEnabled $false
```

### 参考链接：

*1 Skype for Business Online, Meeting Migration Tool https://www.microsoft.com/en-us/download/details.aspx?id=51659

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />



