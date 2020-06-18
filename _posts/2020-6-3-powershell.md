---
layout:     post
title:      使用Powershell 登陆MS Teams admin Powershell
subtitle:  
date:       2020-6-3
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Powershell
- 
---

先安装SkypeOnlineConnector， https://www.microsoft.com/en-us/download/details.aspx?id=39366

然后运行以下Powershell命令：

```
#解决mfa不弹出认证页面的问题
#解决lyncdiscover在内网不解释的问题。OverrideAdminDomain
$String = "password"
$username = "tangx@xxxxx.onmicrosoft.com"
$TenantDomain = "xxxx.onmicrosoft.com"
Import-Module SkypeOnlineConnector;$PWord = ConvertTo-SecureString -String $String -AsPlainText -Force;
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $username, $PWord;
$CSSession=New-CsOnlineSession -credential $Cred -OverrideAdminDomain $TenantDomain
Import-PSSession $cssession –AllowClobber
```

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/b06745e8e8076ac081521f22504cc1b7.png)

之后就可以使用命令进行Teams的管理

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/557fa68a9e783003c67c8d9816c0ffa5.png)



