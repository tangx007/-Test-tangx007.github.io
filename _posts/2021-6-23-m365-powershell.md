---
layout:     post
title:     使用Powershell连接到Microsoft 365后台
subtitle:  M365 Powrshell 后台系列
date:       2021-6-23
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- PowerShell
- M365 PowerShell
---

# 业务应用

Microsoft 365 的Powershell模块叫做MSOnline，它可以通过PowerShell连接到M365进行管理与设置，本质上面它是针对Azure AD的管理，例如AAD的用户管理，组管理，域管理等等。

以下为M365的管理后台，通过PowerShell就可以对这个平台进行管理，以提高 IT Pro 系统管理员的日常工作效率。

![image-20210623082040640](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210623082040640.png)

# 安装MSOnline模块

接下来，我们进行MSOnline模块的安装工作：

- Install the 64-bit version of the Microsoft Online Services Sign-in Assistant: [Microsoft Online Services Sign-in Assistant for IT Professionals RTW](https://download.microsoft.com/download/7/1/E/71EF1D05-A42C-4A1F-8162-96494B5E615C/msoidcli_32bit.msi).
- 安装msonline模块

```
# 安装模块
Install-Module MsOnline
Import-Module MsOnline
```

# 连接到Microsoft 365 后台（中国版与国际版）

```
# 国际版
Connect-MsolService

# 中国版，21v
Connect-MsolService -AzureEnvironment AzureChinaCloud

# 其它版本，参考如下
https://docs.microsoft.com/en-us/microsoft-365/enterprise/connect-to-microsoft-365-powershell?view=o365-worldwide#step-2-connect-to-azure-ad-for-your-microsoft-365-subscription-1
```

# 如何使用？

对于MSOnline来说，本质上面就是对Azure AD上面的User, Group, Role, Policy等对象进行增，删，改，查 的操作。

参考：https://docs.microsoft.com/en-us/powershell/module/msonline/?view=azureadps-1.0

Get-msol ：查询，Add-msol：增加，New-msol：新增，Remove-msol：删除，Set-msol：设置

- 列举一个关于分配Teams phone system许可的例子，如下：

```
## 示例
Get-MsolAccountSku
Get-msoluser -UserPrincipalName AA_RA@scnbwy.com | Set-MsolUser -UsageLocation hk
Set-MSOLUserLicense -UserPrincipalName AA_RA@scnbwy.com -AddLicenses scnbwymvp:PHONESYSTEM_VIRTUALUSER 

```

- 另一个使用示例，创建新用户并为其设置密码与分配许可，如下：

```
# 1. 查询许可
Get-MsolAccountSku
# 2. 创建新用户，名字为O365, 密码为 111111
New-MsolUser -UserPrincipalName test1@ucssitest.onmicrosoft.com -DisplayName 'sfb Test1' -FirstName 'O365' -LastName test1 -password 111111
# 3. 为新用户设置第一次进入时不用更改密码
Set-MsolUserPassword -UserPrincipalName test1@ucssitest.onmicrosoft.com  -ForceChangePassword $false -NewPassword 111111
# 4. 设置地区
get-msoluser -UserPrincipalName test1@ucssitest.onmicrosoft.com | Set-MsolUser -UsageLocation cn
# 5. 分配许可
Set-MSOLUserLicense -UserPrincipalName test1@ucssitest.onmicrosoft.com -AddLicenses ucssitest:O365_BUSINESS

```

# 相关参考

- [Connect to Microsoft 365 with PowerShell](https://docs.microsoft.com/en-us/microsoft-365/enterprise/connect-to-microsoft-365-powershell?view=o365-worldwide)
- [Azure ActiveDirectory (MSOnline)](https://docs.microsoft.com/en-us/powershell/azure/active-directory/overview?view=azureadps-1.0&preserve_view=true)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:25%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

