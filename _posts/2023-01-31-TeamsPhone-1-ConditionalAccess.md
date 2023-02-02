---
layout:     post
title:      Teams phone Deployment Playbook 系列文章
subtitle:   Conditional Access & Compliance Policy在Teams Phone中的应用 - 1 - 部署与测试
date:       2023-01-31
author:  Nemo Tan
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams Voice
- Teams Phone
---

# Conditional Access & Compliance Policy是什么？

在现实的客户生产环境当中，可能会有成百或上千台Teams Phone电话机或者企业为员工配备的智能手机为作办公之用，这些移动设备接入到M365生产环境成为了IT部门的一个安全挑战，因为我们需要保证移动设备/手机/电脑/电话 在连接到M365之后是安全的、合规的、受监管的。这些就是Microsoft Intune 或者叫 Microsoft Endpoint Manager所要做的工作，当然远不止这些，简单来说这是一套SaaS的终端设备管理的解决方案。

![image-20230131221700659](D:\Nemo-xps的文件\tangx007\img\image-20230131221700659.png)

本文重点介绍如何使用Intune中的Conditional Access 与 Compliance Policy来实现Teams Phone的安全合规地登陆到企业的M365。同时也可以抛砖引玉地延申到Windows设备、MTR设备、iOS设备的接入也可以基于这两项技术实现。

那么，Conditional Access顾名思义就是条件访问，Teams phone可以根据所分配的条件访问策略来决定是否被允许登陆到M365，例如只允许某些APP，某些软件版本，指定的位置，是否打开MFA，是否标识为合规设备...etc, 来作为条件访问，若都满足的话Teams Phone则可以登陆，否则就Deny Access.

通过 Compliance Policy - 合规策略，我们可以分配它给特定的用户或组或设备，当他们尝试使用这些设备（例如Teams Phone）登陆到M365的时候，首先会进行 Compliance Policy的检查，若发现有不合规的地方会进行相应的处理（例如标识为不合规、或发邮件通知用户、或锁定设备...etc）

![img](D:\Nemo-xps的文件\tangx007\img\implement-IntuneCA-01.png)

那么，这两项技术配合使用的话会产生一个什么样的功能呢？

首先在Conditional Access中设置为 发现设备标识为 “不合规” 的时候（Require device to be marked as compliant），拒绝登陆，同时在Compliance Policy 配置不合规的时候进行标识，这样就实现了Teams Phone在安全管理员认为有不合符企业安全要求的配置时拒绝其登陆 这样一个安全要求。例如下图：

![image-20230201102114973](D:\Nemo-xps的文件\tangx007\img\image-20230201102114973.png)

# 许可要求

[Licenses available for Microsoft Intune | Microsoft Learn](https://learn.microsoft.com/en-us/mem/intune/fundamentals/licenses#microsoft-intune)

## Microsoft Intune

Intune is included in the following licenses:

- Microsoft 365 E5
- Microsoft 365 E3
- Enterprise Mobility + Security E5
- Enterprise Mobility + Security E3
- Microsoft 365 Business Premium
- Microsoft 365 F1
- Microsoft 365 F3
- Microsoft 365 Government G5
- Microsoft 365 Government G3
- Intune for Education

# 如何为Teams Phone设置Conditional Access Policy

以下是Conditional Access的基本工作流程，会为每一个进入请求都过滤一下，并执行相应的动作（如允许、需要MFA才能进入、拒绝进入），之后就可以访问M365里面的数据。

![Conditional Access overview](D:\Nemo-xps的文件\tangx007\img\conditional-access-overview-how-it-works.png)

当你为当前用户分配了M365 E5许可之后，就可以进行接下来的Conditional Access Policy的创建了。通过我为你整理的以下创建//部署步骤，希望可以让你快速地熟悉条件访问策略里面的内容与逻辑。

首先登陆到Intune的管理界面：https://endpoint.microsoft.com/ ，进入 Devices >> Conditional access

![image-20230131170829630](D:\Nemo-xps的文件\tangx007\img\image-20230131170829630.png)

如以下界面，我已经专门用于测试的访问策略，并设置了一些不支持Teams Phone的参数，后述。

点击 New Policy

![image-20230131170927058](D:\Nemo-xps的文件\tangx007\img\image-20230131170927058.png)

本示例，策略名字为 Supported Conditional Access for Teams Phone

分配相关的用户/组到这个Teams Phone的访问策略，例如我们可以新建一个动态组（AudioCodes_TeamsPhone）可以自动把奥科牌子的Teams Phone加入到本组当中。

![image-20230131171112176](D:\Nemo-xps的文件\tangx007\img\image-20230131171112176.png)

选择如下图所示的Cloud App, 因为Teams Phone在登陆与使用过程中会访问到 Office 365 APP 或 只允许其中三个(Exchange, Teams, SharePoint)

![image-20230131172111058](D:\Nemo-xps的文件\tangx007\img\image-20230131172111058.png)

在Conditions当中选择Android，因为暂时所有的Teams Phone 都是安卓系统的，所以只允许安卓设备进入。

![image-20230131172204339](D:\Nemo-xps的文件\tangx007\img\image-20230131172204339.png)

Optional, 可以为不同厂家的电话机设置 Filter，如下

![image-20230131172306656](D:\Nemo-xps的文件\tangx007\img\image-20230131172306656.png)

以下Client App不支持

![image-20230131172424657](D:\Nemo-xps的文件\tangx007\img\image-20230131172424657.png)

进入 Grant, 这时候可以按你的需要是否设置MFA准入与合规准入的开关。同时以下红框中的四个选项不支持，不能配置。

一般情况下，建议打开Require device to be marked as compliant，这样就可以联合compliant policy来对设备进行安全与合规管理了。

在本文的后段，我还会介绍国外大神的一段测试命令，来测试我们的配置是否符合Teams Phone的使用要求的。

![image-20230131172517260](D:\Nemo-xps的文件\tangx007\img\image-20230131172517260.png)

# 如何为Teams Phone设置Compliance Policy

接着开始创建与配置Compliance Policy, 这部分我们要非常的小心，因为有非常多的雷，非常多的选项是不支持Teams Phone的。

如下，进入Compliance Policy

![image-20230131174930821](D:\Nemo-xps的文件\tangx007\img\image-20230131174930821.png)

创建

![image-20230131175029759](D:\Nemo-xps的文件\tangx007\img\image-20230131175029759.png)

选择Android device administrator

![image-20230131175102465](D:\Nemo-xps的文件\tangx007\img\image-20230131175102465.png)

命名为 Supported Teams Phone compliance policy

![image-20230131175149291](D:\Nemo-xps的文件\tangx007\img\image-20230131175149291.png)

以下选项不支持

![image-20230131175503717](D:\Nemo-xps的文件\tangx007\img\image-20230131175503717.png)

Rooted devices对应value为securityBlockJailbrokenDevices = False, 如果设置为Block 可能会导致设备登陆有问题。

以下选项不支持

![image-20230131214212275](D:\Nemo-xps的文件\tangx007\img\image-20230131214212275.png)

建议对于OS版本不作限制

![image-20230131220934183](D:\Nemo-xps的文件\tangx007\img\image-20230131220934183.png)

以下三个位置的选项不支持，但Company Portal 与 Block USB Debugging需要配置上。

![image-20230131215526876](D:\Nemo-xps的文件\tangx007\img\image-20230131215526876.png)

以下选项不支持

![image-20230131210951902](D:\Nemo-xps的文件\tangx007\img\image-20230131210951902.png)

为不合规的设备配置下一步行动，例如以下，标识为不舍规。

![image-20230131211207481](D:\Nemo-xps的文件\tangx007\img\image-20230131211207481.png)

分配策略给动态组，或指定的用户。

![image-20230131211517346](D:\Nemo-xps的文件\tangx007\img\image-20230131211517346.png)

完成

![image-20230131211543849](D:\Nemo-xps的文件\tangx007\img\image-20230131211543849.png)

# 验证与检查

配置完两条策略之后，我们开始验证与检查是否有生效。首先需要为用户分配 E5的MS Intune许可，之后使用这个用户登陆到Teams 电话机上面。

<img src="D:\Nemo-xps的文件\tangx007\img\image-20230201144349851.png" style="zoom: 67%;" />

登陆完成之后，可以在动态组中发现设备成功加入到该组中。

![](D:\Nemo-xps的文件\tangx007\img\image-20230201145346551.png)

回到Intune中，依次进入 Device >> Android >> Android Devices，发现刚刚登陆上的设备被标识为 Not Compliant，我们需要检查一下原因在哪里？

![image-20230201145846335](D:\Nemo-xps的文件\tangx007\img\image-20230201145846335.png)

点击进入之后，再按如下进行 Device compliance，发现其中一条 合规策略有错误，就是那一条我专门设置了很多不符合Teams Phone规定的配置的策略。

![image-20230201150214253](D:\Nemo-xps的文件\tangx007\img\image-20230201150214253.png)

再点击进去查看，看吧，一堆的Not compliant与Error，与此同时，我旁边的测试电话机在不停地重启Teams App, 不停在重登陆。

所以如果你是第一次使用Conditional Access 或 Compliance Policy， 或者你的Intune有很多的策略的话，可以首先按照上述的配置先做一轮测试，再导入生产环境。

![image-20230201150458426](D:\Nemo-xps的文件\tangx007\img\image-20230201150458426.png)

# 使用脚本检查

那么，对于这两条策略来说，其实是有很多的配置/参数是不被Teams Phone支持的，可以参考：

[Supported Conditional Access and Intune device compliance policies for Microsoft Teams Rooms - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/rooms/supported-ca-and-compliance-policies?tabs=mtr-w#supported-conditional-access-policies)

但在这里，我介绍的是国外大神做的一个PowerShell脚本，可以用来快速检查我们的配置是否正确，如下：

```
# 导入MS Graph的模块
$GraphModule = Get-Module -Name Microsoft.Graph
if ($GraphModule -eq $null){Install-Module Microsoft.Graph -Force -AllowClobber}

# 导入UcLobbyTeams的模块
$UcLobbyTeamsModule = Get-Module -Name UcLobbyTeams
if ($UcLobbyTeamsModule -eq $null){Install-Module UcLobbyTeams -Force -AllowClobber}

# 运行测试命令
Test-UcTeamsDevicesConditionalAccessPolicy -Detailed
```

![image-20230131170511332](D:\Nemo-xps的文件\tangx007\img\image-20230131170511332.png)

```
# 运行测试命令
Test-UcTeamsDevicesCompliancePolicy -Detailed
```

![image-20230131220018205](D:\Nemo-xps的文件\tangx007\img\image-20230131220018205.png)

# 参考

[How to Check Conditional Access Policies and Compliance policy unsupported settings for the Teams Android Devices | UC Mess (wordpress.com)](https://ucmess.wordpress.com/2022/10/24/how-to-check-conditional-access-policies-and-compliance-policy-unsupported-settings-for-the-teams-android-devices/)

[Checking Intune Compliance Policies for unsupported settings | UC Mess (wordpress.com)](https://ucmess.wordpress.com/2022/10/24/checking-intune-compliance-policies-for-unsupported-settings/)

[UCLobby Teams PowerShell Module – UC Lobby](https://uclobby.com/uclobby-teams-powershell-module/)

[Supported Conditional Access and Intune device compliance policies for Microsoft Teams Rooms - Microsoft Teams | Microsoft Learn](https://learn.microsoft.com/en-us/microsoftteams/rooms/supported-ca-and-compliance-policies?tabs=mtr-w#supported-conditional-access-policies)

[Understand Conditional Access policies using Microsoft Endpoint Manager - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/policy-security-management-using-microsoft-endpoint-manager/?source=recommendations)

[什么是 Microsoft Intune | Microsoft Learn](https://learn.microsoft.com/zh-cn/mem/intune/fundamentals/what-is-intune#key-features-and-benefits)



------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

