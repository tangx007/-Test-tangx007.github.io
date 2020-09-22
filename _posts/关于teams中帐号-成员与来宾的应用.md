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



![image-20200807114956986](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807114956986.png)





可以直接在Teams中添加

![image-20200807122303886](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807122303886.png)

在对方的邮箱当中，会收到一封邀请邮件

![image-20200807122657225](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807122657225.png)

点开连接之后，会要求对方输入密码，但事实上整个过程没有要求设置过密码，所以我们点击下面的Forgot password来初始化密码

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807122757033.png" alt="image-20200807122757033" style="zoom:50%;" />

然后，接受审核，目的是授权对方访问我的个人数据

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807123907385.png" alt="image-20200807123907385" style="zoom:50%;" />

其实，所有的Teams用户都是基于Azure AD来作为用户管理的，在上面我在Teams中添加了nemotan@139.com到团队之后，在AAD中就已经创建好这个用户（但这时候的用户来源为Invited，意味着这个用户还在邀请阶段，没有真正地创建出来），在接受了上面的权限审核之后，AAD中的源则变为Microsoft Account。

![image-20200807124302236](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807124302236.png)

在这时候，使用nemotan@139.com登陆到Teams后，就可以看到相应的团队

![image-20200807124705528](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200807124705528.png)











































































<https://teams.microsoft.com/l/message/19:862e4ed381ed49f185df383dc88ff309@thread.skype/1576562788168?tenantId=aece5000-341d-493a-841d-f67e417f1447&amp;groupId=2cefd824-1387-43f3-9177-7f1db4f71dcf&amp;parentMessageId=1576562788168&amp;teamName=Microsoft Teams Community&amp;channelName=05-Teams Community Solution Kit&amp;createdTime=1576562788168>





