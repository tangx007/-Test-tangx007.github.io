---
layout:     post
title:     对于Teams团队的存档、删除与还原
subtitle:  Teams的日常治理与生命周期管理系列文章-6
date:       2022-02-08
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的日常治理与生命周期管理
- MS-700-Teams认证考试 
---

随着公司的Teams团队的使用量增加，团队数量一定会日益增加的，这时候就会出现一些已经不经常使用的团队或者已经结项的团队，我们是否需要对这些闲置的团队作一下清理呢？所以本文会介绍关于Teams团队的存档、删除、还原的知识点与展示。

# 存档Teams团队

‎首先，存档不是删除，而是被隐蔽而已。存档Teams团队时，该Teams团队的所有活动都将停止。存档Teams团队还会存档Teams团队及其关联网站集中的专用频道。但是，你仍然可以添加或删除成员以及更新角色，并且仍然可以在标准和私人频道、文件和聊天中查看所有团队活动，但不能再添加新的聊天、频道、文件等内容了。

同时，被存档的团队仍然可以被重新激活，但被删除的Teams团队不可以，所以我们需要谨慎进行删除操作。

进入Teams管理中心-团队-管理团队-选中nemoTeamsSite示例团队-点击存档，即可把它进行存档

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image002.jpg)

当然你还有机会把这个存档的团队设置为只读状态，如下

![Graphical user interface, application, Teams  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image004.jpg)

存档后的团队不再显示在Teams上面，你需要通过搜索才能找到它。同时已不能再新增聊天、会话、频道etc. 直到一个合适的时间点，我们就可以针对这个存档团队进行删除操作了。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image006.jpg)

# 删除Teams团队

公司的Teams团队可能会有大量敏感数据、财务数据、设计文件等等，真的要删除这些看似闲置了一段时间的团队吗？对的，我决定要删除了。但这是真的删除吗，删了就没有了哦。那不一定，在M365的删除总是有这样一条删除策略来防止管理员删除了有价值的数据或者是重要的文件，配置之前介绍的M365组过期策略，它的删除路径是这样子的：

1、 如下图，新建- 存档 – 过期 – 软删除 – 硬删除（30天后） – 保留

2、 过期策略会有通知机制（30天、15天、1天）

3、 软删除：即本节所讲的删除，通过过期后删除或通过Teams管理中心进行删除。

4、 在软删除30天内，可以进行团队的还原操作，30天之后就是硬删除，即真正的删除，也无法恢复了。

5、 如果你有创建Teams团队的保留策略的话，在硬删除之后，还可以通过eDiscovery把团队里面的内容找出来。

![Diagram  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image008.jpg)

选中相关的团队，点击删除即会进行软删除：

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image010.jpg)

以上在Teams管理中心进行的只不过是软删除操作，要想知道真正的硬删除是在什么时候？可以到Azure管理中心中查看，如下：

Azure管理中心-组-已删除的组-找到nemoM365Group组-永久删除日期，也就是软删除日期的30天之后就会把数据从微软的磁盘上面抹掉。

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image012.jpg)

# 还原Teams团队

如果我想把删除的团队还原呢？其实Teams本质上是基于Groups的，所以如果删除Teams其实就是删除了Group, 所以在deleted groups里面可以把删除的teams恢复，如下：

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image014.jpg)

成功还原： ![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image016.jpg)

还原之后，我们回到Teams，再找到nemoTeamsSite团队可以发现里面的内容也是一样的被还原出来：

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/archive-delete-restore/clip_image018.jpg)

# 还原命令

Get-AzureADMSDeletedGroup and Restore-AzureADMSDeletedDirectoryObject



# 总结

以上是关于Teams团队的存档、删除与还原的介绍与展示，可以给大家一个对于这几种应用场景的感性认识，并且希望能帮忙到大家应用到实际工作当中。在下一文中我会基于此来介绍Teams的备份方案来解决当硬删除后，我们还有没有方法来进行数据恢复或查看。

# 参考 Reference 

https://docs.microsoft.com/en-us/microsoft-365/solutions/end-life-cycle-groups-teams-sites-yammer?view=o365-worldwide#teams&WT.mc_id=M365-MVP-5003881

Archive or delete a team in Microsoft Teams

[https://docs.microsoft.com/en-us/microsoftteams/archive-or-delete-a-team#:~:text=Delete%20a%20team%201%20In%20the%20Microsoft%20Teams,Delete%20to%20permanently%20delete%20the%20team.%20See%20More?WT.mc_id=M365-MVP-5003881](https://docs.microsoft.com/en-us/microsoftteams/archive-or-delete-a-team#:~:text=Delete a team 1 In the Microsoft Teams,Delete to permanently delete the team. See More?WT.mc_id=M365-MVP-5003881)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

