---
layout:     post
title:      Teams phone Deployment Playbook 系列文章
subtitle:   Conditional Access & Compliance Policy在Teams Phone中的应用 - 2 - 常见问题
date:       2023-02-01
author:  Nemo Tan
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams Voice 
- Teams Phone 
---

# 最最最基本的排错步骤

一般来说，所有的Teams认证话机都应该是经过测试并且是没有问题，才会投入到M365的生产环境，并提供给全球所有客户使用的，这这这这这是最最最最最基本的大大大大大大前提（我口吃了吗？虽然我真人真真真有点口吃），除非有Bug（为了我自己窦个底）

Teams认证电话机排错步骤：

1, 先排除内部环境的影响：在外网（4g, 5g, home wifi），使用其它的M365帐号对话机进行登陆测试，若正常则话机本身没有问题。

2, 再排除网络环境的影响：在外网（4g, 5g, home wifi），使用企业自己的M365帐号进行测试，若有问题，则是帐号或策略问题；若无问题，则可能企业内网的网络问题。

3, 可以尝试把电话机的固件在Teams管理中心里面更新到最新版本再测试。

4, 再次验证是否与网络有关：在内网，使用其它的M365帐号对话机进行登陆测试，若仍有问题，则非常大可能便是网络问题了，反之。

5, 最麻烦的就是帐号或策略问题了，先把许可的问题排除掉，可以把有问题的用户的Intune许可拿走，再测试，若正常，则问题与Intune有关。

对于Intune，有两个方向要检查： Conditional Access & Compliance Policy，大型企业的IT管理员很难接触到这两个跟安全有关系的配置，一般在安全管理员的手上，一般又是国外的管理员手上，排错难度进一步增加。

# 对于Conditional Access 的检查

无论如何，我们还是可以去先检查一下Conditional Access的日志，以下这个日志是针对Teams Phone在登陆过程中的一些访问记录，其中就包括了Conditional Access的应用情况，如下路径：

Conditional Access >> Monitoring >> Sign in logs >> User Sign ins (Non-interactive) >> Filter所需要调查的用户名字与时间

![image-20230201163030300](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230201163030300.png)

# 对于Compliance Policy的检查

我们需要检查有问题的用户在合规策略方面，是不是有被标识为不合规的情况。

依次进入 Device >> Android >> Android Devices，发现刚刚登陆上的设备被标识为 Not Compliant，我们需要检查一下原因在哪里？

![image-20230201145846335](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230201145846335.png)

点击进入之后，再按如下进行 Device compliance，发现其中一条 合规策略有错误，就是那一条我专门设置了很多不符合Teams Phone规定的配置的策略。

![image-20230201150214253](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230201150214253.png)

再点击进去查看，看吧，一堆的Not compliant与Error。

![image-20230201150458426](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230201150458426.png)

# 对于Teams Phone的其它常见问题

- Teams Phone在登陆的时候不停登入与登出 Sign in loops or fails for Teams on Android phones

官方的解释是MFA与ToU不能同时使用： You can't sign in or the sign-in continually loops when both the MFA and the Terms of Use (ToU) Conditional Access (CA) policies are used.

此话何解，ToU是什么？如下图：

- 不能在电话机上面删除联系人信息 Can't delete contacts on Android phones

不支持在安卓系统上面删除联系人信息，解决方案是使用Teams Client进行删除。

- 通话中显示忙音的功能不支持 Busy on Busy feature is unavailable

# 参考

[Known issues in Teams Rooms and devices - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/troubleshoot/teams-rooms-and-devices/rooms-known-issues#issues-with-teams-phones)

[Fix Conditional Access-related issues for Teams Android devices - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/troubleshoot/teams-rooms-and-devices/teams-android-devices-conditional-access-issues?source=recommendations)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

