---
layout:     post
title:      Microsoft Teams Voice语音落地系列-3 实战:拨号计划的配置
subtitle:  xxx
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

上一节我们讨论了所有用于Teams语音落地的前置条件准备，一齐来回顾一下：
1)	权限与管理员准备：O365管理员/Teams管理员；SBC管理员；本地Skype管理员；网络管理员；DNS/CA管理员；
2)	许可准备：E3+Phone System Lic or E5 lic; SBC中必要的SIP Lic;；
3)	连接到SFB Online Powershell；
4)	连接到Office 365 Powershell；
5)	已经在O365注册了，并已激活的公司域名；
6)	准备好用于SBC的公网IP与公网FQDN，并做好了公网DNS A记录；
7)	准备好含SBC FQDN的公网证书；
8)	已经制作好相关的防火墙规则，同时网络管理员确认正确；
9)	使用了认证的SBC，并已升级至最新的固件版本；
10)	若对安全有更好的要求，可以在SBC中配置ACL；
以上这些如果你缺了一样，可以预见后面的配置是报错百出，困难重重呀。

接下来这一节，我们将Step by Step地讲述在Teams上面配置拨号计划与语音路由的全过程，其中的逻辑与Skype for Business on premise的语音路由配置基本一样，但是在Teams上面却没有GUI可以配置，只能通过命令来配置，但这样更加有助于大家理解其中的路由逻辑，所以我们就老老实实用命令配置，烧烧脑吧。
首先，先连接到Office 365 Powershell, 需要注意的是如果你使用了MFA的认证方式，在Connect-MsolService的时候不能加入 Credential参数，否则的话Powershell无法弹出二次认证的页面了，如下：

```
$String = "xxxxxxx"
$username = "tangx@contoso.com"
$PWord = ConvertTo-SecureString -String $String -AsPlainText -Force;
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $username, $PWord;
Connect-MsolService -Credential $Cred
```
连接到Skype for Business online Powershell, 用于进行Teams的相关配置。
其中我在New-CsOnlineSession的时候加入了OverrideAdminDomain的参数，主要填写O365租户的域名（如 PoCcontoso.onmicrosoft.com），这样让你避免了这样一个问题：登陆的时候，Powershell在内网会自动发现 Lyncdiscover.xxxx.com域名，但一般正常的Skype on premise混合部署时可能没有使用这个域名，这会导致你用Powershell在内网登陆时失败，如下：

```
$String = "xxxxxxxx"
$username = "tangx@contoso.com"
$TenantDomain = "ucssi.onmicrosoft.com"
Import-Module SkypeOnlineConnector;$PWord = ConvertTo-SecureString -String $String -AsPlainText -Force;
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $username, $PWord;
$CSSession=New-CsOnlineSession -credential $Cred -OverrideAdminDomain $TenantDomain
Import-PSSession $cssession -AllowClobber
```
关闭UseOnPremDialPlan关闭，以避免与本地的拨号计划冲突，如下：
```
Set-CsTenantHybridConfiguration  -UseOnPremDialPlan $false
```

接下来开始配置Teams Dial Plan, 即拨号计划，它的作用是拨号计划是一组命名规范化规则，它将单个用户拨打的电话号码转换为通用的格式（通常为E.164），以便进行呼叫授权和呼叫路由，通俗一点就是把用户的拨号习惯转换成一组通用的格式，例如：

以Skype为例，当用户输入手机号码 159xxxxx 之后，系统会自动通过正则表达式把该号码转换成 +86159xxxxxxx , 这个规则叫做 Normalization Rule ，作用就是进行呼叫授权与呼叫路由（下文的语音路由配置将会使用这个转换来进行路由）
![image-20200613153656290](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153656290.png)

那么，多条Normalization Rule 就组成了一份Dial Plan, 例如：
![image-20200613153847480](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153847480.png) 

更加具体的解释，可参考如下：
https://docs.microsoft.com/en-us/microsoftteams/what-are-dial-plans

在配置拨号计划时，我一般会按照以下原则来配置：
1）	按照用户既有的习惯来配置拨号计划，为什么？
-	因为是用户习惯, 用户就是上帝
-	因为IT改变用户的拨号习惯是非常困难的，如分机号为 5xxxx , 改为 95xxxxx ，你要跟End User讲多少次呀？
-	什么是用户拨号习惯，例如 国际长途拨00xxxxxx, 本地拨8位数字，外地拨 区号+地方号，手机拨 11位数字…

2）	号码转换规范要遵从E.164的号码标准。
-	E164是国际公共电信编号计划，这是个国际标准的电话号码格式，详细如下：
https://www.itu.int/rec/T-REC-E.164-201011-I/en
-	为什么要使用 E.164呢？1. 国际标准 2. 方便与公司其它的PBX系统对接，如果它们也使用同样的标准的话 3. 外资公司的AD信息一般都录入成E.164标准的, 这样会方便你在Teams上面进行快速拨号 4. 别人一看配置就知道你的配置专业与否 5. 最后就是Teams上面强制使用E164标准
-	不知道使用skype for business on premise的同学能不能感受到不使用E.164的疼苦
-	E.164格式的话，我个人的理解很简单，凡是以 + 号开头的号码就可以认为是 E164标准，但后面带的数字又有不同的意思，举例说明一下：
![image-20200613153913574](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613153913574.png)接下来开始真正配置，将会配置一个非常简单的拨号计划（即输入的任何数字都在前面加上一个 + 号），即 12345678 ---> +12345678 （更加复杂的拨号计划会另起一节讲述）

如下图，新建一条Dial Plan, 并且新建转换规则并赋值到 $NR 上面
```
#新建用户级别拨号计划
$DPParent = "CN-Shanghai-Nemo"
New-CsTenantDialPlan $DPParent -Description "Nemo Dial Plan"
$NR += New-CsVoiceNormalizationRule -Name 'CN-Shanghai-All' -Parent $DPParent -Pattern '^(\d*)$' -Translation '+$1' -InMemory
```

 ![image-20200613154423250](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613154423250.png)

把$NR里面的值增加到Dial Plan的转换规则列表里面，为什么这样呢？因为上文有述，多条转换规则组成了一个Dial Plan; 
然后把Dial Plan 应用到相关的用户即可 （Grant-CsTenantDialPlan），最后用Get-CsOnlineUser查询一下是否分配成功，如下：

```
#注意，如果是原来DP基础上更改了Normalization的话，要等比较久的时间才能推送下来
Set-CsTenantDialPlan -Identity $DPParent -NormalizationRules @{add=$NR}
Grant-CsTenantDialPlan -PolicyName $DPParent -Identity tangx@ucssi.com
```

![image-20200613154444964](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613154444964.png)上面是分配Dial Plan, 但其实是还没有这么快生效的，我们使用如下命令测试是否生效：Get-CsEffectiveTenantDialPlan
```
#测试Dial Plan
Get-CsEffectiveTenantDialPlan -Identity tangx@ucssi.com | Test-CsEffectiveTenantDialPlan -DialedNumber 15975733668   
```

![image-20200613154500644](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200613154500644.png)一旦你看到上面的拨号计划按照你配置的正则表达式那样转换号码的话，即表明Dial Plan生效了。至此，我们简单的拨号计划就做完并生效了，但实际的项目中，我们需要把Dial Plan进一步地细分以满足不同的用户习惯，一般我会这样细分出来：短号，市内，国内长途，本地手机，外地手机，Toll Free, 国际长途 （未来将会有一节专门讲述）

本节我们认识到了什么是拨号计划，什么是E.164标准，拨号计划的重要性等；在下一节我们将继续“语音路由配置“，其中的逻辑与Skype for Business 一样有点复杂，大家敬请期待。