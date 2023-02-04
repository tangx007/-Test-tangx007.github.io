---
layout:     post
title:      Teams phone Deployment Playbook 系列文章
subtitle:   [翻译] Skype for Business Online 将于2023年7月31日永久关闭
date:       2023-02-03
author:  Nemo Tan
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams Voice 
- Teams Phone 
---

# Skype for Business Online 将于2023年7月31日永久关闭

官方文章请参阅：https://techcommunity.microsoft.com/t5/microsoft-teams-blog/upgrade-skype-for-business-online-3pip-phones-to-microsoft-teams/ba-p/3645957

# 原文翻译

Microsoft 于 2021 年 7 月停用了 Skype for Business Online (SfBO)。为了帮助保护你在 Skype for Business Online 电话（3PIP 电话）上的投资并帮助你升级到 Teams，我们现在提供一个通过 SIP Gateway 的解决方案来启用这些安全连接并充当 Microsoft Teams 终端设备。**我们鼓励你尽快**进行必要的更新（在本博客后面详细介绍） ，因为 Skype for Business Online **将于 2023 年 7 月 31 日永久关闭**。

> 必要的更新，指的是使用Teams SIP Gateway的解决方案。

 以下是一些可以帮助您 为您的组织制定行动方案的方案：

1. 2023 年 7 月 31 日之后，若您的[兼容](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan#:~:text=common area devices-,Compatible devices,-Vendor) 的 3PIP 电话通过 SIP Gateway 连接到 Teams，则您无需执行任何操作，您的电话将继续通过 SIP Gateway 工作。
2. 您的3PIP 电话连接到 Teams，但在 2023 年 7 月 31 日之后没有使用 Teams SIP Gateway，则您的电话将停止工作，除非您采取下述步骤升级您的设备以通过 SIP Gateway 连接到 Teams。
3. 您有不兼容的 3PIP 设备在 2023 年 7 月 31 日 连接到 Teams – 您的设备将停止工作，您必须将它们替换为 Teams 电话设备或[兼容](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan#:~:text=common area devices-,Compatible devices,-Vendor)的 Teams SIP 设备以获得持续的场景支持。

## **你应该什么时候开始？**

SIP Gateway是客户继续使用 3PIP 电话连接和使用 Teams 的推荐方法。我们认识到将 3PIP 设备升级到 SIP 网关可能需要很长时间，因此我们强烈建议尽快开始该过程。

## **什么是 SIP Gateway？**

[SIP Gateway](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/enable-core-microsoft-teams-calling-functionality-on-compatible/ba-p/3030196/page/3) 使客户能够将[兼容](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan#:~:text=common area devices-,Compatible devices,-Vendor)的第 3方设备与Teams结合使用，并利用以下核心 Teams 呼叫功能：

- 具有保持/恢复和转接功能的来电和去电
- 呼叫转移
- 拨入和拨出会议
- 通话中状态和设备上的“请勿打扰”设置
- 在选定的 SIP 网关[兼容](https://learn.microsoft.com/en-us/microsoftteams/sip-gateway-plan#:~:text=common area devices-,Compatible devices,-Vendor)设备上支持动态 E911
- 语音信箱和留言等待指示灯

SIP 网关适用于启用 PSTN 的 Teams Phone Standard 许可证。

## **为什么要升级到 SIP 网关？**

借助 SIP Gateway，您可以通过更安全、面向未来的核心电话平台继续将您的设备与 Teams 结合使用。SIP Gateway提供以下好处：

1. **增加安全性**：升级到 SIP Gateway，为您的电话设备提供更安全的 Teams 体验。
2. **面向未来的核心电话平台**：SIP Gateway将应用于目前 3PIP 电话无法使用的场景。
3. **在 SIP 设备上加入 Teams 会议**：用户可以从 SIP 设备参加 Teams 会议，与会者可以邀请 SIP 设备参加会议。
4. **管理多个并发呼叫**：进行并发呼叫、通过保持呼叫，接听呼叫或将多个呼叫合并为一个呼叫。

## **将设备升级到 SIP Gateway的过程是怎样的？**

请参考我之前的一篇文章：

- Teams SIP Gateway可以让您的3PIP电话机满血复活

https://www.51nemo.info/2023/02/01/TeamsPhone-3-SIPGateway/

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

