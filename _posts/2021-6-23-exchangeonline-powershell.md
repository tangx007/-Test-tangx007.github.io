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

> 注意：以下连接到Exchange online的命令是基于基本身份验证的，用户名与密码会保存在设备上面而带来安全风险。

```
Set-ExecutionPolicy Unrestricted
$exadmin='tangx@contoso.com'
$cred=Get-Credential $exadmin
$sess=New-PSSession –ConfigurationName microsoft.exchange -Credential $cred -AllowRedirection -Authentication basic -ConnectionUri https://ps.outlook.com/powershell
Import-PSSession $sess
```

例如以下，用户名与密码会保存在$cred变量当中，从而带来安全风险

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210827222107179.png" alt="image-20210827222107179" style="zoom:50%;" />

# EXO V2 Module - 新式验证

那么，本文介绍的是微软的更安全的验证方法：Modern Authentication（基于OAuth 2.0 令牌身份验证的方式），Exchange online V2 PowerShell Module 就是基于这种方式进行验证与连接的，它是基于web的形式把用户名与密码传递给AAD来验证身份

![image-20210827222256994](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210827222256994.png)

## 前置条件

```
# 允许从互联网下载受信任签名的PowerShell脚本
Set-ExecutionPolicy RemoteSigned
# 配置这台电脑可以允许从其它机器传递 WS-Management 请求.
winrm quickconfig
```

![image-20210826000302613](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210826000302613.png)

## 安装模块

```
Install-Module -Name ExchangeOnlineManagement
```

## 连接到Exchange online

```
Connect-ExchangeOnline
```

## 更新模块

```
Import-Module ExchangeOnlineManagement; Get-Module ExchangeOnlineManagement
Update-Module -Name ExchangeOnlineManagement
```

## Troubleshoot installing the EXO V2 module

```
# 以下查询必须为TLS1.2
[Net.ServicePointManager]::SecurityProtocol
```

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210827222634121.png" alt="image-20210827222634121" style="zoom:50%;" />

## 卸载模块

```
Uninstall-Module -Name ExchangeOnlineManagement
```

# 相关参考

- [Basic Authentication and Exchange Online – February 2021 Update](https://techcommunity.microsoft.com/t5/exchange-team-blog/basic-authentication-and-exchange-online-february-2021-update/ba-p/2111904)
- [Connect to Exchange Online PowerShell - EXO V2 module](https://docs.microsoft.com/en-us/powershell/exchange/connect-to-exchange-online-powershell?view=exchange-ps)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:25%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

