---
layout:     post
title:      Microsoft Teams Voice语音落地系列-6 
subtitle:  实战-自动助理实现免费的 IVR
date:       2020-03-23
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

### 什么是IVR?  

Interactive Voice  Response，就是互动式语音应答，你只须用电话即可进入服务中心或企业前台，可以输入员工分机号码后找到相应的用户。例如当你一个前台总机的时候，就会听到这样一段提示音乐：”欢迎致电xxx信息技术有限公司，请直播分机号，查号请播 0 “ ，有了IVR之后，企业的员工就无须单独分配一个直线电话号码，而通过企业总机就可以让外部的人联系到你。

那本文介绍的Teams AA （auto attendants） 正是用来实现这样一个简单但重要的功能。Teams的前身是Skype for Business /  Lync ，在SFB/Lync时代要实现AA的话，必须要在SBC上面对接 3rd  IVR（包括前期的Teams版本也是一样），因为它们没有所谓的“二次拨号” 的功能，也就是“请直播分机号”  这样一个功能。在SFB中虽然有响应组（RGS）的功能，但RGS只能做到  按1转到销售组，按2转到技术组，按3转到市场组….这样的功能。对接一套3rd IVR 费用不便宜，而且增加了整个系统的复杂性，如以下拓扑：

[![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr1)](https://s4.51cto.com/images/blog/202003/23/bce89e07947e3ee2b74554b06faa4688.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

直到Teams AA 的出现 (Cloud auto attendant)，终于可以原生地实现“二次播号”，结合之前详细讲述的Teams Direct Routing 就可以实现这样一个非常重要的场景：

1. 客户A拨打Contoso.com 的总机 +86 21 12345678，这个呼叫通过PSTN来到企业的SBC。
2. SBC通过Direct Routing 把这个总机号码路由给Teams。
3. 因为总机号码已经事先分配给Teams AA , 所以欢迎语音播放出来：“欢迎致电Contoso 信息技术有限公司, 请直播分机号，查号请播0。
4. 客户输入 4321分机号，4321的Teams用户振铃，双方接通。

如下图：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr2222)

没有 IVR 场景的话，企业是很难把电话系统整体搬到Teams 上面来的，所以说这项功能的实现是Teams 项目完美与否的重要条件之一哦，那Teams AA 可以为用户提供这样一些功能：

1）提供企业的欢迎语音，如 欢迎致电xxx信息技术有限公司。

2）可以自定义用户的按键菜单，如按1转到销售组，按2转到技术组，按3转到市场组。

3）提供企业的目录搜索，可以通过语音识别 或 员工的分机号码 （这是重点功能）。

4）Voice Mail 留言。

5）支持语音提示方式：音频文件，Text-to-speech 问候语。

6）自定义工作时间与节假日时间的提示与操作（呼叫转移，挂断，Voice Mail…）。

7）支持转移呼叫到 定义好的接线生，其它员工，呼叫列队Call Queue，自动助理AA。

8）请参考 *1

### 前置条件

接下来就是如何来部署这个Teams AA了，帮忙大家就把中间的坑过一过，我们需要一些前置条件（磨刀不误砍柴工），如下：

1）需要为资源帐号分配 Phone System Virtual User license, 免费，有了它之后资源帐号才能分配企业语音与总机电话号码。

2）需要有一个Azure 资源帐号，用于关联到AA 或 Call queue当中。

3）AA中的前台总机或呼叫转移的用户，需要有Phone System 许可（理论上面应该是有的，因为需要使用本文的读者都是需要Phone System Lic.）。

4）需要同时登陆到O365 PowerShell 与 Teams PowerShell, 以进行PowerShell 的快速部署（参考*2 与 * 3）。

5）（可选）假设前提是你已经部署并测试好Teams Direct Routing, .可以打电话到Teams用户当中。如果你只想做Teams AA的功能验证，则不需要部署DR, 直接拿一个O365环境即可测试。

### 申请免费的Phone System虚拟用户许可

首先我们需要先申请Phone System Virtual User license，如下图（中间省略了很多关于支付方式的创建步骤）

[![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr333)](https://s4.51cto.com/images/blog/202003/23/9330e30e3fbc10cab0ffb4fce51b3fe2.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

申请下来之后，可以登陆O365 PowerShell （参考 *2）查看许可信息：

```
PS C:\WINDOWS\system32> Get-MsolAccountSku

AccountSkuId              ActiveUnits WarningUnits ConsumedUnits

------------              ----------- ------------ -------------

mvp:FLOW_FREE          10000    0      1      
mvp:PHONESYSTEM_VIRTUALUSER   10     0      0      
mvp:ENTERPRISEPREMIUM_NOPSTNCONF 5      0      4      
```

### 创建资源帐号

接着我们需要创建资源帐号，通过它可以关联多个电话号码到同一个AA 或 Call Queue当中，应用场景就是企业中的AA 或 Call Queue 提供了全球/多地的电话接入，用户可以在不同的地方拨打不同的电话号码接入AA 或 Call Queue当中。

第一，先记住这两个在Teams中hardcode好的应用程序ID：

- **Auto Attendant:** ce933385-9390-45d1-9512-c8d228074e07
- **Call Queue:** 11cd3e2e-fccb-42ad-ad00-878b93575e07

```
PS C:\WINDOWS\system32> New-CsOnlineApplicationInstance -UserPrincipalName AA_RA@scnbwy.com -ApplicationId “” -DisplayName "Resource account 1"

RunspaceId    : 4a20f627-c869-4a50-8732-9759fa18177e
ObjectId     : d535722c-96c4-41ac-ac98-e70f565c5c11
TenantId     : b8304c7f-0006-40b3-8c1e-8bd82e8de0ed
UserPrincipalName : AA_RA@scnbwy.com
ApplicationId   : ce933385-9390-45d1-9512-c8d228074e07
DisplayName    : Resource account 1
PhoneNumber    :
```

创建一个Call queue的资源帐号

```
PS C:\WINDOWS\system32> New-CsOnlineApplicationInstance  -UserPrincipalName CQ_RA@scnbwy.com -ApplicationId  “11cd3e2e-fccb-42ad-ad00-878b93575e07 ” -DisplayName "Resource account 2 - call queue"


RunspaceId    : 4a20f627-c869-4a50-8732-9759fa18177e
ObjectId     : 900c4e0f-3a84-42d9-9ae7-690962f4075e
TenantId     : b8304c7f-0006-40b3-8c1e-8bd82e8de0ed
UserPrincipalName : CQ_RA@scnbwy.com
ApplicationId   : 11cd3e2e-fccb-42ad-ad00-878b93575e07
DisplayName    : Resource account 2 - call queue
PhoneNumber    :
```

第三，记得为创建的资源帐号分配刚刚申请下来的免费Phone System许可，不然就会出错以下错误：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr444)

```
Get-MsolAccountSku
Get-msoluser -UserPrincipalName AA_RA@scnbwy.com | Set-MsolUser -UsageLocation hk
Set-MSOLUserLicense -UserPrincipalName AA_RA@scnbwy.com -AddLicenses scnbwymvp:PHONESYSTEM_VIRTUALUSER 
```

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr555)

第四，为资源帐号分配电话号码（例如分配一个企业的总机电话号码）

```
\#使用Direct Routing的号码时，必须使用以下命令来设置AA的电话号码

PS C:\WINDOWS\system32> Set-CsOnlineApplicationInstance -Identity AA_RA@scnbwy.com -OnpremPhoneNumber +8675712345678



RunspaceId    : 391a2a8d-ea7b-4976-ba17-a60f6c9044c4
ObjectId     : d535722c-96c4-41ac-ac98-e70f565c5c11
TenantId     : b8304c7f-0006-40b3-8c1e-8bd82e8de0ed
UserPrincipalName : AA_RA@scnbwy.com
ApplicationId   : ce933385-9390-45d1-9512-c8d228074e07
DisplayName    : Resource account 1
PhoneNumber    :



\#检查设置

PS C:\WINDOWS\system32> get-CsOnlineApplicationInstance -Identity AA_RA@scnbwy.com


RunspaceId    : 391a2a8d-ea7b-4976-ba17-a60f6c9044c4
ObjectId     : d535722c-96c4-41ac-ac98-e70f565c5c11
TenantId     : b8304c7f-0006-40b3-8c1e-8bd82e8de0ed
UserPrincipalName : AA_RA@scnbwy.com
ApplicationId   : ce933385-9390-45d1-9512-c8d228074e07
DisplayName    : Resource account 1
PhoneNumber    : tel:+8675712345678
```

### 分配phone system许可并打开EV 

> 这一步应该在配置Direct Routing的时候就已经做完的了 *4

```
Get-CsOnlineUser | ?{$_.SipProxyAddress -like '*tangx*'} | Set-CsUser -EnterpriseVoiceEnabled $true

Get-CsOnlineUser | ?{$_.SipProxyAddress -like '*tangx*'} | fl *enter*,*name*
```

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr66)

最后，我们可以使用管理中心对这些资源帐号进行管理，如下：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr7)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr88)

### 配置一个简单的Teams AA

刚刚这么折腾地申请许可，创建资源帐号，分配许可 一系列的工作，就是为了下面顺利地配置一个TeamsAA, 不文字了，直接上图就能说明白一切：

> 主要是业务需求问题，已经没有什么技术问题，配置好之后外线就可以拨打总机号码通过的Direct Routing链路接通这个AA实现IVR的功能了

[<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr999" alt="image" style="zoom:50%;" />

![image-20200615130014069](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200615130014069.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr121212)

以下步骤：“设置菜单选项” 比较重要，它实现了播0后要转接的用户（如 王远），同时 “通过分机号拨叫” 实现直播分机号的功能，也就是所谓的二次拨号功能，在sfb时代原生是做不到的，只能靠第三方软件来实现。

“通过分机号拨叫” 原文叫 Dial by extension, 用户听到提示音输入分机号后，Teams会拿这个分机号去AAD里面匹配用户

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr131313)

所以，我们需要在Active Directory or Azure Active Directory 上面为用户把分机号配置上，要求的栏位是：

- HomePhone 
- Mobile/MobilePhone 
- TelephoneNumber/PhoneNumber 
- OtherTelephone 

分区格式也是有要求的：+<phonenumber>;ext=<extension> or x<extension>，我建议是使用 x8001 这种格式，因为容易区分出来。然后，你可以在M365管理中心中配置，也可以使用set-msoluser命令配置，然后需要等待2~12小时的同步时间才会生效。

例如：

- Example 1: Set-MsolUser -UserPrincipalName usern@domain.com -Phonenumber "+15555555678;ext=5678"
- Example 2: Set-MsolUser -UserPrincipalName usern@domain.com -Phonenumber "+15555555678x5678"
- Example 3: Set-MsolUser -UserPrincipalName usern@domain.com -Phonenumber "x5678"

![image-20210804201953359](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210804201953359.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr141414)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr1515)

可以绑定两个帐号，实现同一个AA可以设置多个电话接入号码，可以应用于全球多地接入，多个热线号码 等一些场景当中。

### ![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr161616)

### 基本排错

若查询到的AA Department不是以下值的话，需要更新一下：

```
PS C:\WINDOWS\system32> Get-MsolUser -UserPrincipalName "aa_ra@scnbwy.com"| fl objectID,department

ObjectId  : d535722c-96c4-41ac-ac98-e70f565c5c11
Department : Skype for Business Communication Application Instance
```

需要改为：

```
Set-MsolUser –ObjectId xxxxx -Department "Microsoft Communication Application Instance"
```

### 参考

*1 What are Cloud auto attendants? https://docs.microsoft.com/en-us/microsoftteams/what-are-phone-system-auto-attendants

*2 使用Powershell 登陆O365 方法：Set-ExecutionPolicy RemoteSigned; Install-Module MSOnline; Import-Module MSOnline ; Connect-MsolService

*3 使用Powershell 登陆MS Teams admin Powershell https://blog.51cto.com/nemotan/2481040

*4 Microsoft Teams Voice语音落地系列 -汇总帖 https://blog.51cto.com/nemotan/2420875

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />