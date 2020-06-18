---
layout:     post
title:      Microsoft Teams Voice语音落地系列-4 
subtitle:  实战-Teams语音路由规划与配置
date:       2019-6-23
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

这一节继续我们的Teams语音路由配置，首先要简单讲一下配置的逻辑，不然就会知其然不知其所以然了。

以下是Teams/Skype 的语音路由逻辑图：

1. 用户拨打了一个美国号码，通过Dial Plan转换成 +1 800 642 7676

2. Teams判断是否有Voice Routing Policy分配到该用户, 以下简称VRP

3. 若有分配特定的VRP，则会被应用到对应的VRP策略里面。

4. 在VRP里面会含有一组PSTN Usage, VRP会根据Callee Number给呼叫打上一个标记，就是PSTN Usage。所以你完全可以把PSTN Usage理解为一个标记即可，没有实质性的作用。

5. 第五步就比较重要了，这里会应用上一组/条语音路由 Voice Routing，它会根据Callee  Number来判断是否路由到相应的语音网关上面。同时每一条Voice Routing都关联着一条/组PSTN  Usage，也就是说这通呼叫之前被打上了一个标记PSTN Usage_To China, 那么这通呼叫就只能使用对应的Voice  Routing进行路由了。

   > 若你只有一个语音网关，一个地方的用户，这个理解不了也无所谓，但如果你有多个地方的用户，多条PSTN线路，多个语音网关的话，吃透这个逻辑非常有必要

6. 最后，Voice Routing会直接把呼叫通过Direct Routing链路送达到你的本地语音网关上面。

![image-20200613161409130](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161409130.png)

用以下这张图来实际说明一下，一个VRP下面挂着一个或多个PSTN Usage，在PSTN Usage里面会被关联着多条Voice Route, 它会使用正则表达式来判定这通呼叫会被路由到哪个语音网关上面，所以逻辑路径是这样子的：

```
Call --> Voice Routing Policy ---> PSTN Usage ---> Voice Route --> PSTN Gateway
```

但我们配置的顺序却是反过来的：

```
配置PSTN Gateway ---> 配置PSTN Usage ---> 配置Voice Route, 并关联到PSTN Usage上面 ----> 配置VRP, 并添加PSTN Usage到VRP上面 ----> 把VRP分配给用户
```

![image-20200613161429464](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161445617.png)

首先新建一个PSTN Gateway, 这里面需要用到之前前置条件准备的材料：

- SBC公网FQDN 
- 相应的信令端口，如5061 
- SIP并发数，看你买了多少路SIP Session Lic，如果你做测试的话，就无所谓

如下命令：

```
$FQDN = "teams-test.ucssi.com"
$SipSignallingPort = "5061"
$MaxConcurrentSessions = "100"
New-CsOnlinePSTNGateway -Identity $FQDN -Enabled $true -SipSignallingPort $SipSignallingPort -MaxConcurrentSessions $MaxConcurrentSessions -FailoverTimeSeconds 30 -ForwardCallHistory $true
```

![image-20200613161445617](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161445617.png)

接着创建PSTN Usage,  需要注意它不能新建只能在Global下面不断地增加，同时它只是一个标识，没有实际意义，以下我做了几条不同的PSTN Usage,  后面会分别对应不同的Voice Route。为了简单起见，我们只创建 AllCalls那一条即可。

```
Set-CsOnlinePstnUsage -Identity Global -Usage @{Add="CN-Shanghai-AllCalls"}
```

![image-20200616092655257](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200616092655257.png)

如下命令可以查询PSTN Usage列表：

```
Get-CsOnlinePstnUsage | select usage -ExpandProperty usage
```

![image-20200613161523600](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161523600.png)

接下来创建Voice Route, 它关联着不同的PSTN Usage与PSTN Gateway。我认为的最佳做法是VR与PSTN Usage一一对就起来。

在多地区，多SBC，细分权限的场景中，一个Voice Routing可以对应多个PSTN Usage，这样可以复用到不同的VRP上面

```
$FQDN = “teams-test.ucssi.com”
New-CsOnlineVoiceRoute -Name "CN-Shanghai-All" -Priority 0 -OnlinePstnUsages "CN-Shanghai-AllCalls" -OnlinePstnGatewayList $FQDN -NumberPattern '^\+(\d{*})'
```

![image-20200613161538852](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161538852.png)

做了这么多工作，就是来最后创建Voice Routing Policy的，只要简单的为新建的VRP指定一个PSTN Usage即可，但其中的逻辑一定要搞清楚哦，如下：

```
#新建Voice Routing Policy, 并指定PSTN Usage
#首先增加默认Global的，再增加用户级别的
New-CsOnlineVoiceRoutingPolicy -Identity "CN-Shanghai-All" -OnlinePstnUsages @{Add="CN-Shanghai-AllCalls"} 
```

![image-20200613161551597](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161607188.png)

最后一步工作就是为用户分配VRP策略了

- 使用Set-CsUser为用户分配URI, 启用EV, 启用Voice Mail (注意这里的命令是Set-CsUser，而不是Set-CsOnlineUser)
- 若你要查询Teams用户的属性，请使用Get-CsOnlineUser命令。
- 打开EV，需要事先分配好Phone System Lic，你准备了吗？
- 最后，你就可以按如下命令分配VRP了，过几分钟就可以查询到成功分配VRP了。

```
#注意：需要用xxxx@contoso.com
#查询属性使用：Get-CsOnlineUser才能查到，而不能用Get-CsUser
#修改属性使用：Set-CsUser
#打开EV，需要有Phone System Lic
$user = "tangx@ucssi.com"
Set-CsUser $user -OnPremLineURI tel:+86116
Set-CsUser $user -EnterpriseVoiceEnabled $true -HostedVoiceMail $true

#分配VRP给用户
#只有分配好VRP后，混合部署的话要等差不多24小时，才会有拨号盘出来
Grant-CsonlineVoiceRoutingPolicy -PolicyName "Tag:CN-Shanghai-All" -Identity $user
```

![image-20200613161607188](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161607188.png)

最后，我们就可以在Teams上面看到Teams Dial  Pad的出现，也就意味着在Teams端的配置完成了，所有这些操作我们都可以申请一个国际版的Office 365进行测试，就算没有Phone  System许可也是可以的，因为大不了启用不了EV，不影响我们实战操作，最新的效果如下

![image-20200613161630015](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613161630015.png)

接下来章节，我们就开始配置本地的语音网关与Teams Direct Routing的链路

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />