---
layout:     post
title:      xxx
subtitle:  xxx
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

[12/17/19 2:06 PM] Ares Chen

有意思的一个小技巧，今天发现的。我们现在可以将guest用户，提升为member，进而设置为Owner了。请参考下面这个powershell命令，将usertype设置为Member

https://docs.microsoft.com/en-us/powershell/module/azuread/set-azureaduser?view=azureadps-2.0 

如果这个guest用户已经存在于团队了，需要手工先删除掉，然后再加一次

(2 liked)

举例说明：Contoso公司的租户名称是Contoso，员工邮箱后缀是a@contoso.com，来宾用户建议先通过AzureAD添加，这时候 user@abc.com 是Contoso公司的来宾用户，在Contoso公司中可以找到这个用户，但是该用户尚不具有任何app的访问权限。其次，在contoso公司的应用层面添加user@abc.com，或者可以通过此命令的 -usertype 提升用户为member，这时候用户依旧使用他的 user@abc.com 但成为了contoso公司的member 可以使用Contoso公司内部的应用。 



亲测有效。 建议 理解 Azure B2B 概念 和 Azure 中的 企业应用 概念。另外 一个有用的链接是：myapps.microsoft.com ，  最后 可以去理解下 Microsoft账户 和 外部 Active Directory 账户的概念。 这就差不多了。 



可以参考这文章，学习中..+

https://docs.microsoft.com/en-us/azure/active-directory/b2b/user-properties



































































































<https://teams.microsoft.com/l/message/19:862e4ed381ed49f185df383dc88ff309@thread.skype/1576562788168?tenantId=aece5000-341d-493a-841d-f67e417f1447&amp;groupId=2cefd824-1387-43f3-9177-7f1db4f71dcf&amp;parentMessageId=1576562788168&amp;teamName=Microsoft Teams Community&amp;channelName=05-Teams Community Solution Kit&amp;createdTime=1576562788168>





