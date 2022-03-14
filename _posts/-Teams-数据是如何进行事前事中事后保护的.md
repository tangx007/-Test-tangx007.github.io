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





Microsoft Teams 中的管制合规性

https://docs.microsoft.com/zh-cn/learn/modules/m365-teams-implement-compliance/2-compliance-teams

Microsoft Teams 中的合规性由 Microsoft 365 功能提供，包括信息屏障（IB）、保留策略和通信合规性。

## 了解详细信息

- [定义信息屏障策略](https://docs.microsoft.com/zh-cn/office365/securitycompliance/information-barriers-policies)
- [Microsoft Teams 中的信息屏障](https://docs.microsoft.com/zh-cn/microsoftteams/information-barriers-in-teams)
- [Microsoft Teams 中的保留策略](https://docs.microsoft.com/zh-cn/microsoftteams/retention-policies)
- [Microsoft 365 中的通信合规性](https://docs.microsoft.com/zh-cn/microsoft-365/compliance/communication-compliance?view=o365-worldwide)



# 管理 Microsoft Teams 的安全性和合规性

​	

考虑迁移到 Microsoft Teams 的公司需要知道其数据将受到到行业领先工具的保护。 并且，无论公司在何处运营，都需要遵守生效的数据保护法规。

假设你已确信 Microsoft 通过安全主体构建了 Teams，但仍然担心恶意或不满的内部用户以及使用附件交流聊天消息。 你还想知道，如果对你采取法律行动，你将如何遵守所在司法管辖范围内的法规。

在这里，你将了解 Teams 中提供的高级保护工具和合规性功能。

## 管理安全性

**Microsoft Defender for Endpoint**  是一个统一平台，用于检测和阻止已识别为恶意的文件。 Microsoft Defender for Endpoint  可保护终结点免遭网络威胁，检测高级攻击和数据泄露，并自动处理安全事件。 它可用于 Microsoft Teams，以及 Team 用于内容管理的 SharePoint 和 OneDrive。 通过 Microsoft Defender for Office  365，可以识别所有这些应用程序中的恶意内容，并阻止用户访问该内容。

**安全附件** 检测恶意附件。 全局管理员或安全管理员将创建处理这些可疑恶意附件的策略，以防止将其发送给用户，并通过单击打开后发起攻击。 安全附件保护已向 SharePoint、OneDrive 和 Microsoft Teams 提供。

**条件访问** 是 Azure Active Directory (Azure AD) 的一项安全功能。  Microsoft Teams 依赖 Exchange Online、SharePoint 和 Skype for Business  Online 获得核心功能，如会议、日历、互操作聊天和文件共享。 用户登录 Microsoft Teams  时，为这些云应用设置的条件访问策略将应用于 Teams 用户。

条件访问使用多个信号来确定用户或设备是否可以信任。 条件访问策略由完成第一因素身份验证后强制实施的 if-then 语句组成。 Microsoft Teams 在 Azure Active Directory 条件访问策略中作为云应用单独受到支持。

## 管理合规性

Teams 提供一系列工具，以帮助你处理合规性工作，包括信息屏障、频道、聊天和附件通信合规性、保留策略、数据丢失保护 (DLP) 以及电子数据展示。 要管理本主题中涵盖的功能设置，请转到 Microsoft 365 合规中心。

**信息屏障** 是 Teams 管理员实施的策略，用于防止人员或组相互通信。 在用户没有业务需求进行通信，或有法规理由阻止其通信的情况下，需要设置信息屏障。 通过信息屏障，还可以设置查找和电子数据展示等策略。 这些策略会影响处于一对一聊天、群组聊天或团队级别的用户。

**通信合规性** 是 Microsoft 365 的一项功能，可缓解用户发送不当邮件的风险。  通信合规性可帮助检测、捕获组织中的不当邮件并采取行动。 使用预定义和自定义策略扫描攻击性语言、敏感信息和与内部和法规标准相关的信息。  可以扫描内部和外部通信，以查找策略匹配项。 审阅者可以调查已扫描的电子邮件、Microsoft Teams、Yammer 或第三方通信。  审阅者确保内容符合组织的邮件标准，并在必要时采取措施。

**保留策略** 帮助管理组织中的信息保留。 使用保留策略保留必须存储的数据，以遵守组织的内部策略、行业法规或法律需求。 保留策略也用于删除被视为不利因素的数据、不再需要保留的数据，或不具有法律或业务价值的数据。

默认情况下，Teams 聊天、频道和文件数据将无限期保留。 Teams 的保留策略在 Microsoft 365  合规中心内，或者通过使用安全与合规中心 PowerShell cmdlet 进行创建和管理。 可将 Teams  保留策略应用于整个组织或特定用户和团队。 你可以为聊天和频道消息设置 Teams 保留策略，并决定保留还是删除数据，或者是将其保留一段特定时间。

**DLP 功能** 包括 Microsoft Teams 聊天和频道消息，以及专用频道消息。 如果组织具有 DLP，则你可以定义策略以防止人员在 Microsoft Teams 频道或聊天会话中共享敏感信息。

**电子数据展示** 是在需要时识别、收集和生成以电子形式存储的信息 (ESI) 的流程。  通过电子数据展示，可以对涉及组织内人员的法律案件作出回应。 此流程可能涉及在电子邮件、文档、即时消息会话和其他位置中查找和保留特定信息。 在  Teams 中，电子数据展示包含聊天、消息、文件、会议和通话摘要。 可在安全与合规中心中执行上述以及其他类似活动。