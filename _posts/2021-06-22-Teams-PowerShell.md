---
layout:     post
title:     使用Powershell连接到Microsoft Teams Admin后台
subtitle:  M365 Powrshell 后台系列
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
> 那本文主要是介绍面向IT管理员的Teams admin Powershell 连接。

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

<img src="C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210622161042699.png" alt="image-20210622161042699" style="zoom:50%;" />

# 更新Teams Powershell 模块

```
Update-Module MicrosoftTeams
```

# 登陆到 MS Teams后台

```
Connect-MicrosoftTeams 
```

![image-20210622161929532](C:\Users\Nemo\Documents\GitHub\tangx007\img\image-20210622161929532.png)

# 卸载Teams Powershell 模块

```
Uninstall-Module MicrosoftTeams
```

# 相关参考

- [Install Microsoft Teams PowerShell Module](https://docs.microsoft.com/en-us/MicrosoftTeams/teams-powershell-install)
- [Microsoft Teams cmdlet reference](https://docs.microsoft.com/en-us/powershell/teams/?view=teams-ps)

