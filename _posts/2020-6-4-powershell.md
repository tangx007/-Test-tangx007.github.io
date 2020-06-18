---
layout:     post
title:      使用PowerShell连接O365 Exchange online
subtitle:  
date:       2020-6-4
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Powershell
- 
---

非常简单，直接上脚本，用于日后查询之用

```
Set-ExecutionPolicy Unrestricted
$exadmin='tangx@contoso.com'
$cred=Get-Credential $exadmin
$sess=New-PSSession –ConfigurationName microsoft.exchange -Credential $cred -AllowRedirection -Authentication basic -ConnectionUri https://ps.outlook.com/powershell
Import-PSSession $sess
```







