---
layout:     post
title:     Azure AD的访问评审功能在Teams团队中应用
subtitle:  Teams的日常治理与生命周期管理系列文章-7
date:       2022-02-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的日常治理与生命周期管理
- MS-700-Teams认证考试 
---

# 访问评审的简介

这是Teams日常治理系列的最后一编文章，我们来介绍一个在Azure AD上面管理用户访问权限的比较小众的功能：访问评审（Access Review）。在Azure AD上面我们可以使用背对背（B2B）的方式来轻松地实现跨组织边界的协作，最常见的场景就是你的Teams团队需要其它外部的来宾进行访问以及一起在你的Teams团队中进行协作（例如市场活动专员组织一场发布会，需要多方供应商一同来维护这个发布会项目），那么Teams团队可以邀请他们作来来宾来访问Teams团队进行项目讨论与信息分享。

而Access Review 在这里发挥的作用就是来为这些来宾用户进行所谓的访问评审，这样可以保证来宾用户在公司的Teams团队中有适当的权限进行访问与读写，同时可以保证来宾用户都是活跃用户，而不是一些为时已久的闲置用户。

在Access Review当中，我们指定审阅者即评审员（Reviewer），他可以为来宾用户进行审阅来决定是否删除来宾的访问权限，或者直接由来宾用户自己来决定。

# 许可与权限要求

·    许可：Azure AD Premium P2

·    权限：global administrator；User administrator；M365 or AAD Security Group owner of the group to be reviewed

# 创建访问评审

以下我们来一步一步地展示一下创建Access Review以及其发挥的效果。

首先进入Azure AD管理中心 – Azure Active Directory – Identity Governance – Feature highlights – Access Reviews

> Identity Governance： 确保Azure AD的用户具备的访问权限以及访问权限对应的资源都是正确的，在这文中就是来宾用户Guest User。它具有三大模块（Entitlement management，Access reviews，Privileged Identity Management），其中的Access reviews便是本文在介绍的功能。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image002-16453378897692.jpg)

点击New access review，新建一个访问评审：

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image004-16453378897694.jpg)

因为我们需要对Teams团队中的来宾用户进行评审，所以选择评审的类型为Teams + Groups，选择评审所有Teams团队还是指定的团队。![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image006-16453378897681.jpg)

确定评审员为Teams团队的创建人，或者你可以指定其它用户来评审员。

确定评审周期，视具体情况而定。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image008-16453378897693.jpg)

根据Teams团队的应用场景，按需进行特别的评审设置，例如：

Auto apply results to resource: 是否需要在审核时间结束后自动删除被拒绝（Deny）用户的访问权限。

If reviewers don't respond: 评审员未作出响应的操作，如不改变、删除访问权限、自动同意本访问评审、系统自动进行建议的操作（同意与拒绝）。

Action to apply on denied guest users: ‎如果来宾用户被Deny的操作：从Teams团队中删除用户的成员身份‎、‎阻止来宾用户登录 30 天，然后从租户中删除用户‎。

 

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image010-16453378897695.jpg)

最后，完成创建这个关于Teams团队的访问评审。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image012-16453378897696.jpg)

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image014-16453378897697.jpg)

点开这个“Review guest access across Microsoft 365 groups” 的访问评审，可以查看与修改这个评审的设置、历史、日志以及关联的Teams团队（实质上为M365组）。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image016-16453378897698.jpg)

点开“Nemo私人使用” 这个Teams团队，进一步查看本团队的评审情况：

![Graphical user interface, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image018-164533788976910.jpg)

可以看到我的这个团队里面有两个外部的来宾用户需要进行评审（分别来自139.com 与 hotmail.com的来宾），状态为未评审：

![Graphical user interface, text, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image020-16453378897699.jpg)

在另一边厢，作为评审员（Access Reviewer）的我，系统已经发来一封需要我进行评审的邮件

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image022-164533788976911.jpg)

然后在以下网站即可以针对刚刚创建的请问评审，对这两个来宾来决定他们俩的去留了（Approve or Deny）。

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image024-164533788977013.jpg)

[当我把nemotan@139.com](mailto:当我把nemotan@139.com)给拒绝之后，在访问评审的后台即可以看到评审的结果：

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image026-164533788977012.jpg)

经过一段M365的同步时间后，[被拒绝的nemotan@139.com](mailto:被拒绝的nemotan@139.com) 来宾用户就会从“Nemo私人使用” 的Teams团队成员中被自动删除。

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image028-164533788977014.jpg)

# 来宾用户的自我评审

上述介绍的是通过Teams团队创建者来作为评审员进行评审的展示，但是如果我们组织若有大量的来宾，或者需要多次进行评审操作的话，对于评审员来说这个工作量是非常大的。

那么对于安全要求没有那么高的场景/企业来说，我们可以自行由来宾进行自我评审。以下介绍这种方法：

创建一个安全组（AccessReviewerSecurityGroup），首先增加两个来宾用户的帐号（[nemo.tan@hotmail.com](mailto:nemo.tan@hotmail.com) & nemotan@139.com）

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image030-164533788977015.jpg)

但是我们来改进一下这个AccessReviewerSecurityGroup安全组里面的成员类型，因为公司里面的来宾用户是动态变化的，我们没有办法确定这个安全组里面的成员是谁？

那么，为了解决这个问题，可以把成员类型改为“动态用户”，以及增加一条动态用户的查询，如下图：

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image032-164533788977019.jpg)

在动态成员的查询规则里面，设置属性userType为Guest，保存退出。

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image034-164533788977017.jpg)

之后，在这个安全组里面的成员就会自动地动态更新里面的成员。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image036-164533788977016.jpg)

接下来，按照之前的方法，创建一个Access Review。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image038-164533788977018.jpg)

但是，在选择评审员（Reviewer）的时候，需要选择 User review their own access，这样让来宾用户自行对自己进行评审。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image040-164533788977020.jpg)

完成这个访问评审之后，[在nemo.tan@hotmail.com](mailto:在nemo.tan@hotmail.com)的邮箱中就会收到以下的评审邮件：

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image042-164533788977022.jpg)

来宾用户自行进行评审：

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image044-164533788977021.jpg)

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image046-164533788977023.jpg)

跟之前一样，在Access Review的后台就可以查看到所有来宾用户的评审结果。通过使用基于动态用户的安全组来创建的来宾用户自行评审的方式，就可以大大减少公司中评审员的工作量，也可以自动化地对外部用户作一个初步的筛选了。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image048-164533788977024.jpg)

# 总结

以上我们介绍了这个对于Teams团队的来宾用户管理的小众功能：访问评审，从而实现了可以定期对这些非常企业内用户的访问进行访问评审，来确保这些来宾是否活跃、是否需要、是否安全等问题 由评审员进行人工/自动的响应，可以增强我们在Teams团队使用过程中的组织安全性，以及对于Azure AD帐号的日益增长进行必要的治理。因为在Azure AD可能会存在许多无用或者多余的来宾帐号，需要管理来清理的。

# 参考

Manage guest access with Azure AD access reviews

https://docs.microsoft.com/en-us/azure/active-directory/governance/manage-guest-access-with-access-reviews?WT.mc_id=M365-MVP-5003881

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

