---
layout:     post
title:      使用Powershell 登陆Exchange online
subtitle:  
date:       2020-6-2
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Powershell
- 
---

非常简单，直接上脚本，上图

```
$credential = Get-Credential tangx@xxxxx.com
Install-Module MsOnline ; Import-Module MsOnline
Connect-MsolService -credential $credential
$exchSession = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $credential -Authentication Basic -AllowRedirection
Import-PSSession $exchSession –DisableNameChecking
```

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/5cbaf2fcae9df71f68c75f53411cc09f.png)







