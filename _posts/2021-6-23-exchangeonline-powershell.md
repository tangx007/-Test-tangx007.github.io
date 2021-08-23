---
layout:     post
title:       使用PowerShell连接到Exchange online后台(EXO V2 module)
subtitle:  M365 Powrshell 后台系列
date:       2021-06-23
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Powershell
- M365 PowerShell
---

# EXO V1 Module - 基本身份验证

在2021年2月之前，以下基于Basic Authentication的身份验证方法来访问Exchange online的方式还能用，也就是所谓的EXO V1 module

> 注意：以下命令已不适用。

```
Set-ExecutionPolicy Unrestricted
$exadmin='tangx@contoso.com'
$cred=Get-Credential $exadmin
$sess=New-PSSession –ConfigurationName microsoft.exchange -Credential $cred -AllowRedirection -Authentication basic -ConnectionUri https://ps.outlook.com/powershell
Import-PSSession $sess
```

# EXO V2 Module - 新式验证

那么，本文介绍的是微软的更安全的验证方法：Modern Authentication，Exchange online V2 PowerShell Module 就是基于这种方式进行验证与连接的。







# 相关参考

- [Basic Authentication and Exchange Online – February 2021 Update](https://techcommunity.microsoft.com/t5/exchange-team-blog/basic-authentication-and-exchange-online-february-2021-update/ba-p/2111904)
- [Connect to Exchange Online PowerShell - EXO V2 module](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps)

