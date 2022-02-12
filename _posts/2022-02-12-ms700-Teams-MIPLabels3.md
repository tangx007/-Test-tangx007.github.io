---
layout:     post
title:     信息安全与合规的神器-敏感度标签MIP Label
subtitle:  Teams的安全与合规系列文章-1
date:      2022-02-12
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams的日常治理与生命周期管理
- MS-700-Teams认证考试 
---

# 什么是敏感度标签（MIP Labels）

‎为了完成协作性的工作（如共享文档、文档协作、团队内的聊天、文档访问等等），组织中的员工需要与组织内外的其他人（如供应商、合作伙伴、客户）进行协作。这就意味着组织里面的一些文档、文件、设计图、聊天记录、数据可能有意无意间地被泄漏出去了，那么为了可以满足你组织的业务和合规性策略，微软有一个非常强大而复杂的安全与合规平台专门处理企业内外的信息安全、合规与隐私的问题。‎

在这篇文章里面，我们来介绍其中一种信息安全保护技术：敏感度标签Microsoft Information Protection (MIP) sensitivity labels，同时看看它在Teams团队、M365其它产品上面发挥的信息安全作用。

MIP Label 就像一个信封一样可以被粘在这些数据上面，它有几个特性：

\-     自定义：可以基于Label Policy 自定义这个MIP标签的作用，并作出一系列的限制。

\-     明文：MIP标签本身是明文的，所以3rd应用可以基于这些标签来设计他们的应用。

\-     随着文件跟随：由于MIP标签存储在文件和电子邮件的元数据中，因此无论内容保存或存储在哪里，标签都会随内容一起漫游。这种唯一标签将成为应用和实施您配置的策略的基础。‎ 

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image002-16445953336271.jpg)

MIP标签有以下几个步骤来部署与实施敏感度标签：创建MIP标签、启用MIP标签、为MIP标签发布一个标签策略、终端用户在他们的文档中使用MIP 标签、M365安全合规平台基于文档的MIP标签进行强制的信息保护操作，如下图：

![Diagram showing workflow for sensitivity labels.](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image004.png)

# 如何开启MIP Labels

Azure AD 支持将 ‎‎Microsoft 365 合规性中心‎‎发布的敏感度标签应用于 Microsoft 365 组，简单来说就是为M365组给打上标签。

然后通过创建一些合规策略来限制这些MIP Lables的限制范围，同时也可以应用于M365的其它应用（下节详述）

以下为大家展示一下使用PowerShell把MIP Lable开启的方法：

```
在运行中打开powershell ise
```

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image006-16445953336282.jpg)

```
# 连接到AzureAD Preview
Install-Module AzureADPreview
Import-Module AzureADPreview
Connect-AzureAD
```

![Graphical user interface, text  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image008-16445953336283.jpg)

```
# 获取 Azure AD 组织的当前组设置，如下图
$grpUnifiedSetting = (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ)
$template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
$setting = $template.CreateDirectorySetting()
```

![img](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image010-16445953336284.jpg)

```
# 展示组信息
$Setting.Values
```

![Shape  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image012-16445953336285.jpg)

```
# 把EnableMIPLabels这个敏感度标签开关打开
$Setting["EnableMIPLabels"] = "True"
$Setting.Values
```

![Shape  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image014-16445953336296.jpg)

```
# 保存配置到AZ
Set-AzureADDirectorySetting -Id $grpUnifiedSetting.Id -DirectorySetting $setting
```

```
# 查看配置
get-AzureADDirectorySetting -Id $grpUnifiedSetting.Id | fl
```

![Graphical user interface  Description automatically generated with low confidence](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image016-16445953336297.jpg)

到此，MIP Labels就启用了，‎还需要将敏感度标签同步到 Azure AD‎。

# 敏感度标签同步到 Azure AD‎

操作如下：

```
#连接到安全与合规中心的命令行界面
Import-Module ExchangeOnlineManagement
Connect-IPPSSession
```

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image018-16445953336298.jpg)

# 把MIP标签同步到AAD

```
Execute-AzureAdLabelSync 
```

# 如何创建MIP Labels

首先，我们需要先创建MIP Labels，打开M365安全与合规中心，进入信息保护-创建标签

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image020.jpg)

![Graphical user interface, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image022.jpg)

定义此标签的范围，如文件、电子邮件、组和网站（经过上一节，我们已经把标签同步到AzureAD）…

![Graphical user interface, text, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image024.jpg)

可以针对文件与电子邮件进行加密，与添加水印

![Graphical user interface, text, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image026.jpg)

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image028.jpg)

MIP标签可以基于特定的文件内容进行自动标签

![Graphical user interface, text, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image030.jpg)

控制内部和外部用户对标记的团队和 Microsoft 365 组的访问级别。

![Graphical user interface, text, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image032.jpg)

通过上面的隐私和外部用户访问设置，我们可以使用MIP标签来统一管理Teams团队的隐私级别（公用还是专用），同时还可以控制外部用户是否能访问特定的Teams团队

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image034.jpg)

简单几步之后，这个“设计图-机密”的MIP标签就完成创建。

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image036.jpg)

> 需要同步的时间耗时：
>
> \-     New label: Wait for one hour.
>
> \-     Existing label: Wait for 24 hours.

# 为MIP标签发布一个标签策略

标签创建完成之后，需要把它应用/发布到相关的用户或用户组，使他们可以自由使用这些标签或强制使用标签（例如工程部门、研发部门、财务部门）

![Graphical user interface, application  Description automatically generated with medium confidence](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image038.jpg)

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image040.jpg)![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image042.jpg)

强制用户为文档打上标签

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image044.jpg)

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image046.jpg)、![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image048.jpg)

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image050.jpg)

![Graphical user interface, text, application, email  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image052.jpg)

完成，可能需要24小时才能将标签发布到所选用户的应用

![Graphical user interface, text, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image054.jpg)

# MIP Labels在Teams & SharePoint & Group & Doc上面的应用

这些信息安全技术都是有点抽象，那么需要看看它实际展现出来的效果才能直观感受它的威力，首先来看看我把自己的帐号应用上‘设计文档-机密’的MIP标签之后，电脑里面的word文档全部变成以下这个样子：

因为我在标签策略中设置了必须应用MIP标签，所以我必须指定一个分配给我的MIP标签才能继续编辑以下这份来自女儿小学的登记表：

![Graphical user interface, application, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image056.jpg)

![Graphical user interface, application, table, Word  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image058.jpg)

同时，还同时强制为文档使用了页眉、页脚、水印。这些手段可以有效地防止企业的信息泄漏，甚至可以限制文件的转发、加密等等…

![Graphical user interface, application, table, Excel  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image060.jpg)

再来看看Teams，当我创建一个Teams团队的时候，就会看到MIP标签，因为这个标签设置了只能使用专用团队，那么其余两个团队类型也变成不可用了。

![Graphical user interface, application  Description automatically generated](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/clip_image062.jpg)

# 许可要求

请参考：

[Microsoft 365 guidance for security & compliance - Service Descriptions | Microsoft Docs](https://docs.microsoft.com/en-us/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-365-security-compliance-licensing-guidance#which-licenses-provide-the-rights-for-a-user-to-benefit-from-the-service-11)

 For manual sensitivity labeling, the following licenses provide user rights:

-  Microsoft 365 E5/A5/G5/E3/A3/G3/F1/F3/Business Premium (Information Protection for Office 365 – Standard should be enabled if E5 license only has been assigned)
-  Enterprise Mobility + Security E3/E5
-  Office 365 E5/A5/E3/A3/F3

-  AIP Plan 1

-  AIP Plan 2


 For both client and service-side automatic sensitivity labeling, the following licenses provide user rights:

- Microsoft 365 E5/A5/G5

- F5 and E5 Compliance

- F5 Security & Compliance

- Microsoft 365 E5/A5/G5 Information Protection and Governance

- Office 365 E5


# 总结

以上便是敏感度标签在M365应用上面的应用场景，整文介绍了什么是敏感度标签、如何开启、如何创建、如何发布标签策略以及生效后在文档与Teams团队中的真实效果。

大家以此为参考可以更加深入地了解与使用其它安全合规中心里面的功能。

# 参考 Reference

Get started with sensitivity labels

[https://docs.microsoft.com/en-us/microsoft-365/compliance/get-started-with-sensitivity-labels?view=o365-worldwide&WT.mc_id=M365-MVP-5003881](https://docs.microsoft.com/en-us/microsoft-365/compliance/get-started-with-sensitivity-labels?view=o365-worldwide&WT.mc_id=M365-MVP-5003881)

 【创建标签】Create and configure sensitivity labels and their policies

[https://docs.microsoft.com/en-us/microsoft-365/compliance/create-sensitivity-labels?view=o365-worldwide&WT.mc_id=M365-MVP-5003881](https://docs.microsoft.com/en-us/microsoft-365/compliance/create-sensitivity-labels?view=o365-worldwide&WT.mc_id=M365-MVP-5003881)

 【启用标签功能】Enable sensitivity label support in PowerShell

[https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-assign-sensitivity-labels#enable-sensitivity-label-support-in-powershell?WT.mc_id=M365-MVP-5003881](https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-assign-sensitivity-labels#enable-sensitivity-label-support-in-powershell?WT.mc_id=M365-MVP-5003881 ) 

【启用标签功能】How to enable sensitivity labels for containers and synchronize labels

[https://docs.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-teams-groups-sites?view=o365-worldwide#how-to-enable-sensitivity-labels-for-containers-and-synchronize-labes&WT.mc_id=M365-MVP-5003881](https://docs.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-teams-groups-sites?view=o365-worldwide#how-to-enable-sensitivity-labels-for-containers-and-synchronize-labes&WT.mc_id=M365-MVP-5003881 ) 

【发布/分配标签】Assign a label to an existing group in Azure portal

[https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-assign-sensitivity-labels#assign-a-label-to-an-existing-group-in-azure-portal?WT.mc_id=M365-MVP-5003881](https://docs.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-assign-sensitivity-labels#assign-a-label-to-an-existing-group-in-azure-portal?WT.mc_id=M365-MVP-5003881)

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />



------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

