---
layout:     post
title:      Microsoft Teams Voice语音落地系列-4-1 
subtitle:  实战-GUI管理Teams DR
date:       2019-12-09
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

当你开始阅读本文的时候，我认为大家应该是向着Direct Routing这个主题而来的。那么什么是Microsoft Teams Direct Routing?  简单一点来说就是建立在Microsoft Teams Phone System 与企业本地的语音设备（如CUCM, Avaya PBX,  IMS, AudioCodes, Ribbon, Sonus….）的一条加密的SIP Trunk, 通过这条SIP  Trunk，让您的Teams获得打电话的能力 或 与您企业本地的电话系统互通的能力，如下拓扑：

> 参考：Microsoft Teams Voice语音落地系列-1 架构简述

![image-20200614093825366](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614093825366.png)

但是非常可惜的是，在Teams侧配置Direct Routing 与 相关的Voice Policy,  Dialplan的时候是没有图形化界面，需要完全使用命令来进来配置，使到本来很简单的一件事情变得异常的复杂；了解SFB语音相关的同学应该也知道配置Skype Voice Route的时候还是比较麻烦，更何况是使用命令来配置了。

> PS. 之前我有写一篇使用命令配置的文章
>
> Microsoft Teams Voice语音落地系列-3 实战:拨号计划的配置:https://blog.51cto.com/nemotan/2383423
>
> Microsoft Teams Voice语音落地系列-4 实战:Teams语音路由规划与配置: https://blog.51cto.com/nemotan/2384879

最近在项目上发现了国外的大牛做的第三方图形配置工具，分享给大家参考：

#### [Microsoft Teams Direct Routing Tool](https://www.myskypelab.com/2019/02/microsoft-teams-direct-routing-tool.html)

1）下载-Powershell运行-连接到O365

2）输入O365用户名与密码

![image-20200614093853014](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/dr0000000000938055013325e765f774e833286b3849.png)

![image-20200614093937815](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614093937815.png)

运行后的图形化界面如下：

![image-20200614094002028](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094037473.png)

接下来，我们来把整个配置过程都过一次，配置逻辑如下：

> 参考：Microsoft Teams Voice语音落地系列-4 实战:Teams语音路由规划与配置: https://blog.51cto.com/nemotan/2384879

配置PSTN Gateway ---> 配置PSTN Usage ---> 配置Voice Route, 并关联到PSTN Usage上面 ----> 配置VRP, 并添加PSTN Usage到VRP上面 ----> 把VRP分配给用户

1）点击Gateways
2）点击add
3）输入您之前已经规划好的网关域名（不要告诉我，你是临时想出来的哦…）
4）输入您之前已经规划好的网关的端口号
5）完成

![image-20200614094037473](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094037473.png)

输入以下PSTN Gateway的技术参数，点击OK后就完成PSTN Gateway的创建了。

![image-20200614094105858](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200614094105858.png)

配置PSTN Usage 

1）点击add usage

2）创建add a new usage

![image-20200614094314674](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094314674.png)

配置Voice Route, 并关联到PSTN Usage上面

1）点击需要编辑的PSTN Usage

2）编辑

3）增加Voice Route

![image-20200614094437688](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094437688.png)

1）默认的Voice Route匹配规则不一定合适，所以要改改

2）按需修改Voice Route的号码正则表达式

3）增加这一条Voice Route对应的PSTN Gateway, 就是使用哪一个网关出局。

> 注：Number Pattern为^(\+\d+)$

![image-20200614095137389](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614095137389.png)

配置VRP, 并添加PSTN Usage到VRP上面

1）新建一条VRP

![image-20200614094612489](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094612489.png)

1）选中对应的VRP

2）关联对应的Usage

3）选中，并OK

![image-20200614094641373](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094641373.png)

1）最后，还可以通过以下来测试一下，你做的VRP是否可用

![image-20200614095408304](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614095408304.png)

最后，我们需要把VRP分配给用户，但是我发现这个工具并没有这个功能，所以就只能使用命令了

- 使用Set-CsUser为用户分配URI, 启用EV, 启用Voice Mail (注意这里的命令是Set-CsUser，而不是Set-CsOnlineUser)
- 若你要查询Teams用户的属性，请使用Get-CsOnlineUser命令。
- 打开EV，需要事先分配好Phone System Lic，你准备了吗？
- 最后，你就可以按如下命令分配VRP了，过几分钟就可以查询到成功分配VRP了。

```
#注意：需要用xxxx@contoso.com
#查询属性使用：Get-CsOnlineUser才能查到，而不能用Get-CsUser
#修改属性使用：Set-CsUser
#打开EV，需要有Phone System Lic
$user = "tangx@contoso.com"
Set-CsUser $user -OnPremLineURI tel:+86116
Set-CsUser $user -EnterpriseVoiceEnabled $true -HostedVoiceMail $true

#分配VRP给用户
#只有分配好VRP后，混合部署的话要等差不多24小时，才会有拨号盘出来
Grant-CsonlineVoiceRoutingPolicy -PolicyName "Tag:CN-Shanghai-All" -Identity $user
```

![image-20200614095444060](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200614095444060.png)

本篇文章旨在说明如何使用GUI的方式来配置DR, 那么更新深入的技术原理，可以参考以下文章：

- [Microsoft Teams Voice语音落地系列 -汇总帖 置顶](https://blog.51cto.com/nemotan/2420875)
- https://blog.51cto.com/nemotan/2420875