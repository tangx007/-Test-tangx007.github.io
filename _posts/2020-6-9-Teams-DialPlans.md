---
layout:     post
title:      Microsoft Teams Voice语音落地系列-4-外传
subtitle:  实战-使用Teams后台管理中心TAC配置拨号计划
date:       2020-6-9
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

> 本文的拨号计划完成基于国际对于电话号码的标准 E.164格式展开

在大约一年前，我写过关于Teams语音落地的系列文章，详细把整个架构与配置过程都过了一遍，之前看过我的文章的同学会发现为什么配置这个本来就复杂的拨号计划与语音路由需要用到命令，为什么没有一个友好的配置界面来完成这些任务。

直到最近，这些配置终于是可以通过TAC界面完成了，这文章将会带着大家来完成这一部份的配置。

在开始之前，我还是不厌其烦地先把整个Direct Routing前置条件再回顾一次，：
1)  权限与管理员准备：O365管理员/Teams管理员；SBC管理员；本地Skype管理员；网络管理员；DNS/CA管理员；
2)  许可准备：E3+Phone System Lic or E5 lic; SBC中必要的SIP Lic;
3)  连接到SFB Online Powershell；（不需要了，因为可以通过界面配置）
4)  连接到Office 365 Powershell；（不需要了，因为可以通过界面配置）
5)  已经在O365注册了，并已激活的公司域名；
6)  准备好用于SBC的公网IP与公网FQDN，并做好了公网DNS A记录；
7)  准备好含SBC FQDN的公网证书；
8)  已经制作好相关的防火墙规则，同时网络管理员确认正确；
9)  使用了认证的SBC，并已升级至最新的固件版本；
10) 若对安全有更好的要求，可以在SBC中配置ACL；
以上这些如果你缺了一样，可以预见后面的配置是报错百出，困难重重呀。

### 基本概念

整个Teams语音的配置逻辑与Skype for Business on premise的语音路由配置基本一致。首先配置Teams Dial Plan, 即拨号计划，把一组命名规范化规则，它将单个用户拨打的电话号码转换为通用的格式（通常为E.164），以便进行呼叫授权和呼叫路由，通俗一点就是把用户的拨号习惯转换成一组通用的格式，例如下图把我的手机号码159757xxxxx 转成 +86159757xxxxx

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609203415.png" style="zoom:50%;" />

这有什么用呢？转成E164之后，这个呼叫就会在后台进行呼叫授权与呼叫路由（下文的语音路由配置将会使用这个转换来进行路由），我们把这个转换规则称作为Normalization Rule

那么，多条Normalization Rule 就组成了一份Dial Plan, 例如：

> 拨号计划 https://docs.microsoft.com/en-us/microsoftteams/what-are-dial-plans

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609203449.png)

通过以下界面就可以配置拨号计划，然后我们会遵从一些原则：

- 基于Teams用户的拨号习惯, 用户就是上帝，IT改变用户的拨号习惯是非常困难的，如分机号为 5xxxx , 改为 95xxxxx ，你要跟End User讲多少次呀？
- 号码转换规范要遵从E.164的号码标准，因为Teams是强制使用E.164标准，且方便日后与其它系统对接

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609161049.png)

E.164格式的话，我个人的理解很简单，凡是以 + 号开头的号码就可以认为是 E164标准，但后面带的数字又有不同的意思，举例说明一下：

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609203512.png)

### 新手入门

接下来开始真正配置，将会配置一个非常简单的拨号计划（输入的任何数字都在前面加上一个 + 号），即 12345678 ---> +12345678 

在上述界面新增加一个拨号计划，再新增一个转换规则，如下图中的If condition 与 Then do this中，输入下面的正则表达式即可做出一个非常简单的转换规则
（匹配所有数字后，在原有输入前加上一个“+”号）

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609161643.png" style="zoom:50%;" />

创建完后，点击保存后，一个简单的拨号计划即创建完，并且可以测试你创建的计划是否能用

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609164237.png)

那么刚刚我们是用高级的选项来创建转换规则（直接使用正则表达式），以下我们用友好一点的基本方法来创建。

如下图配置，意思就是匹配以021开头的11位数字，然后把第一位数字删除，并在开头增加+86
我们可以看看最后面的Test就大概会明白什么意思的。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609164031.png" style="zoom:50%;" />

### 专业的配置方法

刚刚我们简单创建了一条自动加“+”号的拨号计划，对于简单的项目你可以这样做，但你可以表现得专业一点，我们接下来创建一些更加细致的转换规则。
按下图，分别创建不同的转换规则，包括本地固话，长途固话，免付费电话，服务电话，长途手机号码，本地手机号码，国际号码，都区分出来了。
这样做的好处在于后面我们可以基于这些号码来做更加精细化的呼叫路由。例如你可以把呼叫路由的权限划分为国内呼叫用户，国际呼叫用户，本地呼叫用户
![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609165407.png)

以下是一些命令参考，复制里面的Name, Pattern, Translation 三个值可以帮助你快速创建自己的拨号计划（当然你要按需修改哦）
```
New-CsVoiceNormalizationRule -Name "CN-Internal-7xxxx" -Parent $DPParent -Pattern '^([7]\d{3})$' -Translation '+86108888$1' -InMemory 
New-CsVoiceNormalizationRule -Name "CN-Internal-4xxxx" -Parent $DPParent -Pattern '^([4]\d{3})$' -Translation '+86108888$1' -InMemory 
New-CsVoiceNormalizationRule -Name 'CN-Beijing-webexTollFree' -Parent $DPParent -Pattern '^(10800\d{7})$' -Translation '+86$1' -InMemory 
New-CsVoiceNormalizationRule -Name 'CN-Beijing-TollFree' -Parent $DPParent -Pattern '^([4|8]00\d{7})\d*$' -Translation '+86$1' -InMemory 
New-CsVoiceNormalizationRule -Name "CN-Beijing-Local" -Parent $DPParent -Pattern '^([1-9]\d{7,9})$' -Translation '+8610$1' -InMemory 
New-CsVoiceNormalizationRule -Name 'CN-Beijing-Mobile-OOA' -Parent $DPParent -Pattern '^((01[23456789]\d{9}))$' -Translation '+86$1' -InMemory 
New-CsVoiceNormalizationRule -Name 'CN-Beijing-Mobile-Local' -Parent $DPParent -Pattern '^((1[23456789]\d{9}))$' -Translation '+86$1' -InMemory 
New-CsVoiceNormalizationRule -Name 'CN-Beijing-National' -Parent $DPParent -Pattern '^0((([12]0|2[2-9])\d{8}|21\d{8,10}|[3-9][1-9]\d{8,9}|(701|90[1-9])\d{7}|80[2678]\d{5}|95\d{3}|([12]0|2[2-9])\d{3,5}|21\d{3,5}|[3-9][1-9]\d{3,6}|(701|90[1-9])\d{2,6}))\d*(\D+\d+)?$' -Translation '+86$1' -InMemory 
New-CsVoiceNormalizationRule -Name 'CN-Beijing-Service' -Parent $DPParent -Pattern '^(999|[1|9]\d{2,5})$' -Translation '+8610$1' -InMemory
New-CsVoiceNormalizationRule -Name 'CN-Beijing-International' -Parent $DPParent -Pattern '^(?:\+|00)(1|7|2[07]|3[0-46]|39\d|4[013-9]|5[1-8]|6[0-6]|8[1246]|9[0-58]|2[1235689]\d|24[013-9]|242\d|3[578]\d|42\d|5[09]\d|6[789]\d|8[035789]\d|9[679]\d)(?:0)?(\d{6,14})(\D+\d+)?$' -Translation '+$1$2' -InMemory
```

### 拨号计划的分配

最后就是把这个拨号计划分配用户了，比较简单：

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609170022.png)

分配拨号计划之后，需要等待一些时间让Teams同步配置，所以你也许不能立即可以在下图看到结果

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609223135.png)

### 发烧级的配置

请参考上一篇文章：[Microsoft Teams Voice语音落地系列-3 实战:拨号计划的配置](https://blog.51cto.com/nemotan/2383423)

接下来另一篇文章，有了拨号计划之后，就需要做一个语音路由，我将会讲一下如何使用TAC来创建语音路由。