---
layout:     post
title:     使用Powershell连接到Microsoft Teams Admin后台
subtitle:  M365 Powrshell 后台系列 - Connect-MicrosoftTeams
date:       2021-6-22
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- M365 PowerShell
- PowerShell
---

> Microsoft Teams ，微软的一款企业级的沟通协作平台，系统管理员日常需要对它进行必要的运维管理，它分为面向业务用户的Team Powershell 与面向IT管理员的Teams admin Powershell 。
>
> 那本文主要是介绍面向IT管理员的Teams admin Powershell 连接，即Teams的管理后台命令。

# 前置条件

检查版本： PowerShell 5.1 or higher

```
PS D:\> $PSVersionTable.PSVersion 

Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      18362  1474    
```

安装：PowerShellGet 与 MicrosoftTeams

```
Install-Module -Name PowerShellGet -Force -AllowClobber
Install-Module -Name MicrosoftTeams -Force -AllowClobber #本命令需要科学上网进行下载
```

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210622161042699.png" alt="image-20210622161042699" style="zoom:50%;" />

# 更新Teams Powershell 模块

```
Update-Module MicrosoftTeams
```

# 登陆到 MS Teams后台

```
Connect-MicrosoftTeams
```

![image-20210622161929532](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210622161929532.png)

# 卸载Teams Powershell 模块

```
Uninstall-Module MicrosoftTeams
```

# 相关示例

以下示例，为Teams用户分配电话号码，打开企业语音，并分配语音路由策略及拨号计划，最终可以让Teams用户使用拨号盘进行打电话的操作：

```
Install-Module -Name PowerShellGet -Force -AllowClobber
Install-Module -Name MicrosoftTeams -Force -AllowClobber #本命令需要科学上网进行下载
Connect-MicrosoftTeams

$userlist = import-csv MigrationList.csv
$TenantDialPlan = "CN-Shanghai"
$CsonlineVoiceRoutingPolicy = "CN-Shanghai"


foreach ($user in $userlist)
{
    $uri = "tel:+" + $user.uri1 + $user.uri2
    Set-CsUser $user -OnPremLineURI $uri
    Set-CsUser $user -EnterpriseVoiceEnabled $true -HostedVoiceMail $true -Verbose
    Grant-CsonlineVoiceRoutingPolicy -PolicyName $CsonlineVoiceRoutingPolicy -Identity $user -Verbose
    Grant-CsTenantDialPlan -PolicyName $TenantDialPlan -Identity $user
}
```

# 相关参考

- [Install Microsoft Teams PowerShell Module](https://docs.microsoft.com/en-us/MicrosoftTeams/teams-powershell-install)
- [Microsoft Teams cmdlet reference](https://docs.microsoft.com/en-us/powershell/teams/?view=teams-ps)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:25%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />
