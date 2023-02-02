---
layout:     post
title:      Teams phone Deployment Playbook 系列文章
subtitle:   Teams SIP Gateway可以让您的3PIP电话机满血复活
date:       2023-02-01
author:  Nemo Tan
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams Voice 
- Teams Phone 
---

# SIP Gateway是什么？

传统意义上的SIP Gateway, 是一个基于SIP协议的处理和传输语音数据的设备（SBC or 语音网关），从模拟到数字的语音设备，市面上只要支持通用的SIP协议的网络电话都可以注册到SIP Gateway上面，就是我们所谓的网络电话。但是这里所谓的SIP Gateway却并不是这样一回事，本文所要介绍的是微软大约在2021年12月发布的一个让那些受微软支持的传统 SIP电话机（Generic SIP Phone）可以注册/登陆到Teams的一个组件。

> 支持SIP话机列表：[Compatible SIP Phone devices - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan#compatible-devices)

如下图，现阶段能够与Teams互通打电话的设备，有四个选项，包括：

1. 基于Direct Routing连接到SBC的各种各样SIP电话或者模拟电话机。
2. 原生的Teams客户端，以及原生的Teams Native 电话机。
3. 基于SIP Gateway的SIP Phone。
4. 基于3PIP的SfB电话机，就是以前用于登陆到Skype for Business online的电话机。（市场存量太多了，不可能全部都停止支持的，但这种电话机在2023年7月将会淘汰），替代方案就是本文的主角Teams SIP Gateway.

> [Upgrade Skype for Business Online (3PIP) phones to Microsoft Teams using SIP Gateway - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/upgrade-skype-for-business-online-3pip-phones-to-microsoft-teams/ba-p/3645957)

本文将会重点介绍如何让SIP电话机注册到Teams SIP Gateway的规划与部署过程。



![image-20230202204341600](D:\Nemo-xps的文件\tangx007\img\image-20230202204341600.png)

# 前期准备与规划

- 先确保你手上的电话机是受支持的，https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan#compatible-devices

- 如果这是一台旧的SIP电话，请首先重置电话机。对于奥科电话机，按着 OK+Menu 来开机即可重置。

- 你至少拥有Teams Device Admin权限。

- 已购买了 M365 Phone System许可，或 Teams Phone 许可。

- 已配置了 Teams Voice （三个选项：Calling Plan, Operator Connect, Direct Routing）。

- 对于公共电话机，省钱方案是购买 Microsoft Teams Shared Devices license。

- 开放到Teams与M365的防火墙，参考：https://learn.microsoft.com/en-us/microsoft-365/enterprise/urls-and-ip-address-ranges

- 确保SIP电话机没有使用代理服务器。

- Open the UDP port. Open UDP port in the range 49152 to 53247 for IP ranges 52.112.0.0/14 and 52.122.0.0/15.

- Open the TCP port. Open TCP port 5061 for IP ranges 52.112.0.0/14 and 52.122.0.0/15.

- Open the following https endpoints (IP addresses and URLs):

  13.75.175.145
  52.189.219.201
  51.124.34.164
  13.74.250.91
  13.83.55.36
  23.96.103.40
  https://blobsdgapac.blob.core.windows.net
  https://blobsdgemea.blob.core.windows.net
  https://blobsdgnoam.blob.core.windows.net
  https://httpblobsdgapac.blob.core.windows.net
  https://httpblobsdgemea.blob.core.windows.net
  https://httpblobsdgnoam.blob.core.windows.net

# 为什么要使用 Teams SIP Gateway

1. 穷？错，不能双重标准，我们专业的说法叫资产保护，设备历旧。
2. 原文为：Skype for Business Online **will be permanently turned off on July 31, 2023**，换句说话，现网使用的3PIP 电话将会在2023年7月之后 Stop Working。
3. 对于一些不需要智能电话，不需要特别功能，没有电脑设备的环境或场景，我们需要更为简单的接入设备，那么简单的SIP Phone都合适不过了。
4. 你听说过DECT吗？在这之前我也没听说过，查了一下原来这是就是无线电话的简称，原来在德国的制造车间里面可能是没有有线网络覆盖的，同时也不可能会安排一台有线电话，原因是都是大型机械的组装或制造，当遇到问题要与其它部门沟通的时候，跑到公共电话机的位置极不方便。所以他们需要一台无线电话，那么在Teams里面，解决方案就是基于Teams SIP Gateway的 DECT Phone. (Frontline worker on the go)

> [https://techcommunity.microsoft.com/t5/microsoft-teams-blog/highlights-from-enterprise-connect-2022-new-microsoft-teams/ba-p/3263176](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/highlights-from-enterprise-connect-2022-new-microsoft-teams/ba-p/3263176?WT.mc_id=M365-MVP-6771)

# Teams管理员中心中启用 SIP Gateway （必须）

[Enable SIP Gateway for the users in your organization](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-configure#enable-sip-gateway-for-the-users-in-your-organization) , 如下配置即可：

![image-20230202110337915](D:\Nemo-xps的文件\tangx007\img\image-20230202110337915.png)

# 在SIP电话机的配置

本文以奥科的SIP电话机为示例，其它牌子的话机基本雷同。首先我们需要记住以下三个URL，例如我希望电话机就近在亚太的SIP Gatway服务器登陆的话，就选APAC，etc.

[Set the SIP Gateway provisioning server URL](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-configure#set-the-sip-gateway-provisioning-server-url)

- EMEA: `http://emea.ipp.sdg.teams.microsoft.com`
- Americas: `http://noam.ipp.sdg.teams.microsoft.com`
- APAC: `http://apac.ipp.sdg.teams.microsoft.com`

本示例，我会手动把这个URL配置到电话机中，但实际的生产环境你更需要的是使用DHCP Option来推送这条URL到电话机。请参考话机厂商的支持文档。

进入Automatic Provisioning >> Static URL >> Config URL为APAC URL >> Check Now，之后，电话机会自动重启并下载最新的固件并更新，务必注意的是不能拔电源线，不然固件刷新失败会变砖。如下图：

![image-20230202111318639](D:\Nemo-xps的文件\tangx007\img\image-20230202111318639.png)

# 两种登陆的方法

完成Provisioning更新与固件更新之后，我们就可以用它来登陆Teams了。这里介绍有且只有的两种话机登陆方法，但请再次确认以下：

- 已购买了 M365 Phone System许可，或 Teams Phone 许可，并分配给相关用户。
- 已配置了 Teams Voice （三个选项：Calling Plan, Operator Connect, Direct Routing）。

![image-20230202112015874](D:\Nemo-xps的文件\tangx007\img\image-20230202112015874.png)

## 第一种方法：SIP 电话机上面登陆

非常简单，请SIP电话机的用户自行点击Sign in，按如下步骤即可登陆。Teams管理员无须界入。

![image-20230202114741337](D:\Nemo-xps的文件\tangx007\img\image-20230202114741337.png)

以下是我手上的四台奥科话机，都可以成功地登入Teams并正常使用。

![image-20230202144722646](D:\Nemo-xps的文件\tangx007\img\image-20230202144722646.png)



## 第二种方法：通过Teams管理员中心登陆 - 适用于公共电话机（Teams CAP）

这是一个我想了半天也不知道为什么的复杂方法，官网的解释是使用Teams管理员中心让电话登陆到Teams 是为了streamline （精简），但事实上一点也不精简/简单。如下让大家围观一下：

进入 Teams Devices >> Phones >> Provision devices

![image-20230202155611687](D:\Nemo-xps的文件\tangx007\img\image-20230202155611687.png)

点击Upload

![image-20230202152912336](D:\Nemo-xps的文件\tangx007\img\image-20230202152912336.png)

若你有多台电话机，可以批量上传电话机的Mac地址 （你需要收集话机的mac list）

![image-20230202152739735](D:\Nemo-xps的文件\tangx007\img\image-20230202152739735.png)

上传完成之后，选中电话机，并点击Gen Verification code，会生成一个验证码。

![image-20230202153153967](D:\Nemo-xps的文件\tangx007\img\image-20230202153153967.png)

神奇的操作来了，这时候，你需要请电话机用户在电话上面拨打 *55* + 验证码，例如：

```
*55*331446
```

你（Teams 管理员）再次在如下界面生成登陆码（步骤四），并发送给电话机用户，再次请他在浏览器上面进行验证与登陆。

到此为止，本方法麻烦了Teams管理员两次，电话机用户两次，才能完成登陆，何谓精简？

![image-20230202155358406](D:\Nemo-xps的文件\tangx007\img\image-20230202155358406.png)

# 最后

划重点：Skype for Business Online **will be permanently turned off on July 31, 2023**，换句说话，现网使用的3PIP 电话将会在2023年7月之后 Stop Working，而本文的Teams SIP Gateway正是这些电话机的替代方法。

# 参考

[Plan SIP Gateway - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan)

[Configure SIP Gateway - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-configure#set-the-sip-gateway-provisioning-server-url)

[Teams SIP Gateway (msxfaq.de)](https://www.msxfaq.de/teams/pbx/teams_sip_gateway.htm)

[https://techcommunity.microsoft.com/t5/microsoft-teams-blog/highlights-from-enterprise-connect-2022-new-microsoft-teams/ba-p/3263176](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/highlights-from-enterprise-connect-2022-new-microsoft-teams/ba-p/3263176?WT.mc_id=M365-MVP-6771)



------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

