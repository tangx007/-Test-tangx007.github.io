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

Microsoft 365 的Powershell模块叫做MSOnline，它可以通过PS连接到M365进行管理与设置，本质上面它是针对Azure AD的管理，例如AAD的用户管理，组管理，域管理等等。

以下为M365的管理后台，通过PS就可以对这个平台的管理，以提高系统管理员的日常工作效率。

![image-20210623082040640](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210623082040640.png)

# 安装MSOnline模块

- Install the 64-bit version of the Microsoft Online Services Sign-in Assistant: [Microsoft Online Services Sign-in Assistant for IT Professionals RTW](https://download.microsoft.com/download/7/1/E/71EF1D05-A42C-4A1F-8162-96494B5E615C/msoidcli_32bit.msi).
- 安装msonline模块

```
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

# 如何使用

参考：https://docs.microsoft.com/en-us/powershell/module/msonline/?view=azureadps-1.0

Get-msolxxxxx ：查询

Add-msolxxxx：增加

New-msolxxxx：新增

Remove-msolxxx：删除

Set-msolxxxxx：设置

# 相关参考

- [Connect to Microsoft 365 with PowerShell](https://docs.microsoft.com/en-us/microsoft-365/enterprise/connect-to-microsoft-365-powershell?view=o365-worldwide)
- [Azure ActiveDirectory (MSOnline)](https://docs.microsoft.com/en-us/powershell/azure/active-directory/overview?view=azureadps-1.0&preserve_view=true)





