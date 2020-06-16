---
layout:     post
title:      玩转Microsoft Teams Room系列(1)
subtitle:  部署前置条件与帐号创建
date:       2020-03-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

### MTR会议室规划概述

Microsoft Teams Room 提供了完全一致的会议体验，可为所有规模的会议（从大，中，小，微型会议室）提供高清视频、音频和内容共享。我之前有写过一个介绍Teams Meeting的Blog，可以参考 *0；本系列将会详细介绍MTR的前期规划，部署，管理与排错方法。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p33389e15565a25eb3fe8ec36fac791978a7.png)

如下为MTR在规划阶段需要知道的一些前置条件：

1. 对于制作的安装镜像来说，MTR对于OS的版本有严格的要求（OS Build = 18362.356 or 17134.191）。
2. 为每个MTR会议室设备准备一个Teams会议室帐号。
3. 确保MTR会议室设备可以连接Internet (Access to HTTP ports 80 and 443)。
4. （可选）如果你的Exchange使用内网数字证书的话，则需要把内网根证书安装到MTR里面。
5. MTR电脑的本地帐号为Admin, 密码为sfb。
6. 硬件方面，MTR需要这几块硬件 1）MS Teams认证的MTR控制系统（电脑主机+控制屏）2）MS Teams认证的USB音频，视频终端，摄像头 3）至多两块液晶显示大屏 ，请参考 *2 里面有列出来哪些设备是认证的，使用认证设备的好处在于音频与视频质量好之外，还有它们与Teams系统具有非常好的兼容性。
7. 通常情况下，若你选购了认证的MTR的控制系统（如 *2 所列出的），那么就已预先安装好OS与MTR软件了；但如果像本系列那样，自己选购硬件的话，则需要提供准备一个专门的OS安装镜像，将在第二节中详细介绍。

### MTR会议室的部署流程

1. 选择好硬件设备组合（如本系列会采用 Intel NUC + Logitech Rally + Logitech Tap100 + MTR） ，参考 *2。
2. 开通Teams会议室帐号（前提是你已有O365许可：E2 or Teams Room Lic） 。
3. 下载指定版本的windows镜像。
4. 使用[CreateSrsMedia.ps1](https://go.microsoft.com/fwlink/?linkid=867842)脚本把相关的组件下载并结合windows iso 制作成为一个MTR安装U盘 。
5. 把U盘安装到MTR电脑主机当中 ----> 把所有的外设连接起来，整理好，并登陆到MTR帐号。
6. 最后，用户在Outlook里面就可以预约到这间MTR会议了。

### 创建MTR会议室帐号

你可以手动来创建(开AAD帐号，分许可，开邮箱，设置成会议室帐号…etc…), 但是这次我们使用一个自动化脚本来配置MTR帐号，同时这个脚本是微软官方支持的：(PS脚本的下载请[参考 *1](https://go.microsoft.com/fwlink/?linkid=870105))，依次运行即可：

> Set-ExecutionPolicy RemoteSigned
>
> Install-Module MSOnline; Import-Module MSOnline ; Connect-MsolService #提示输入O365管理员密码
>
> d:\SkypeRoomProvisioningScript.ps1 

运行了上面的命令之后，就会开始使用脚本来创建会议室帐号。这里我们选择2 for a Skype Room Systems v2, 其实所谓的MTR就是之前Skype时代的SRS系统。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr1image_thumb38.png)

选1 开通AD与资源邮箱帐号

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr2222image_thumb37.png)

选2 AD帐号开通在Azure云上面：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr33333image_thumb36.png)

输入完这个之后，就会弹出窗口让你输入O365的管理员密码：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr4444image_thumb35.png)

之后，开始创建MTR帐号：

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr55555image_thumb34.png" alt="image" style="zoom:50%;" />

分配许可，具有 O365 E2以上 或 Teams Meeting Room 许可都可以：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr666image_thumb33.png)

创建完O365用户之后，开始创建资源邮箱帐号：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr777image_thumb32.png)

最后一步，开始创建Exchange Mailbox, 并且把它转成会议室帐号，注意红框的等待时间为10分钟（hardcode在脚本里，当然你可以去改）

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr8888image_thumb31.png)

最后就创建出来了MTR会议室帐号了：

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr9999image_thumb43.png" alt="image" style="zoom:50%;" />

接着，我们来检查一下：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtr101010image_thumb6.png)

放两段手动创建资源邮箱的命令，供参考：

```
New-Mailbox -Name "Project-Rigel-01" -Alias ProjectRigel01 -Room -EnableRoomMailboxAccount $true -MicrosoftOnlineServicesID ProjectRigel01@contoso.onmicrosoft.com -RoomMailboxPassword (ConvertTo-SecureString -String 'P@$$W0rd5959' -AsPlainText -Force)

Set-CalendarProcessing -Identity "Project-Rigel-01" -AutomateProcessing AutoAccept -AddOrganizerToSubject $false -DeleteComments $false -DeleteSubject $false -RemovePrivateProperty $false -AddAdditionalResponse $true -AdditionalResponse "This is a Teams Meeting room!"
```

### 参考

*0 Microsoft Teams Meeting 系列 汇总帖 https://blog.51cto.com/nemotan/2470146

*1 https://download.microsoft.com/download/9/0/D/90D4826A-9FD2-47D2-B911-97BF1737F4F7/SkypeRoomProvisioningScript.ps1.txt

*2 MTR Hardware requirements https://docs.microsoft.com/zh-cn/MicrosoftTeams/rooms/requirements





