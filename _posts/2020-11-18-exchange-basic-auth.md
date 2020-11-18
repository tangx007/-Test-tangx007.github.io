---
layout:     post
title:      Exchange Online中关于基本身份验证停用的相关信息汇总
subtitle:   重点：推迟到2021年中旬
date:       2020-11-18
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Exchange online
- 
---



关于Exchange Online中基本身份验证停用的一些信息：
1）EWS应用程序基于这些身体验证标准：基本身份验证，NTLM身份验证，OAuth 2.0 令牌身份验证
2）2020 10月的时候，会停止那些没有用过基本验证的用户，例如新开的exchange online租户。
3）2021年中旬的时候，会全面停止基本身份验证
4）只针对商用的exchange online, 对于outlook.com没有影响。
5）EWS，Exchange ActiveSync (EAS), IMAP, POP, and Remote PowerShell都会受到影响。
6）SMTP AUTH 不影响，就是发邮件不影响。



## Exchange Online deprecating Basic Authentication

https://docs.microsoft.com/en-us/lifecycle/announcements/exchange-online-basic-auth-deprecated?WT.mc_id=M365-MVP-5003881

## Deferred end of support date for Basic Authentication in Exchange Online

https://developer.microsoft.com/en-us/office/blogs/deferred-end-of-support-date-for-basic-authentication-in-exchange-online/?WT.mc_id=M365-MVP-5003881





