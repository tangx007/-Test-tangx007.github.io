---
layout:     post
title:      Microsoft Teams Voice语音落地系列-4-外传2
subtitle:  实战-使用Teams后台管理中心TAC配置语音路由策略
date:       2020-6-10
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

> 语音路由策略是整个Teams语音落地的关键部分，一旦分配了这个策略，Teams用户的拨号盘就会显示出来供用户使用

对上一节中，我们通过Teams管理中心把拨号计划创建出来，并分配给用户。这一节我们还是使用管理界面来进行Teams语音路由配置，首先要简单讲一下配置的逻辑：

1）  用户拨打了一个美国号码，通过Dial Plan转换成 +1 800 642 7676

2）  Teams判断是否有Voice Routing Policy分配到该用户, 以下简称VRP

3）  若有分配特定的VRP，则会被应用到对应的VRP策略里面

4）  在VRP里面会含有一组PSTN Usage, VRP会根据Callee Number给呼叫打上一个标记，就是PSTN Usage。所以你完全可以把PSTN Usage理解为一个标记即可，没有实质性的作用。

5）  第五步就比较重要了，这里会应用上一组/条语音路由 Voice Routing，它会根据Callee  Number来判断是否路由到相应的语音网关上面。同时每一条Voice Routing都关联着一条/组PSTN  Usage，也就是说这通呼叫之前被打上了一个标记PSTN Usage_To China, 那么这通呼叫就只能使用对应的Voice  Routing进行路由了。

（若你只有一个语音网关，一个地方的用户，这个理解不了也无所谓，但如果你有多个地方的用户，多条PSTN线路，多个语音网关的话，吃透这个逻辑非常有必要）

6）  最后，Voice Routing会直接把呼叫通过Direct Routing链路送达到你的本地语音网关上面。

![image-20200610195604346](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200610195604346.png)

一个VRP下面可以挂着一个或多个PSTN Usage，在PSTN Usage里面会被关联着多条Voice Route, 它会使用正则表达式来判定这通呼叫会被路由到哪个语音网关上面（参考上述第三，第四步），所以逻辑路径是这样子的：

> Call --> Voice Routing Policy ---> PSTN Usage ---> Voice Route --> PSTN Gateway

首先新建一个PSTN Gateway（这里需要用到之前定义好的SBC FQDN, 也就是你之前申请证书的主体名字），其实也就是一条基于TLS加密的SIP Trunk，微软把它定义为Teams Direct Routing

![image-20200610201754301](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200610201754301.png)

最后创建出来的Direct Routing链路就会是这样子：

![image-20200611112111521](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611112111521.png)



创建一条北京本地固话的语音路由CN-Beijing-Local，Dialed number pattern为^\+8610([1-9]\d{7,9})

![image-20200611114143821](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611114143821.png)

然后在这条语音路由中选择对应的落地SBC

> 展开一下，如果企业在全国各地都有SBC，为了做经济路由，你可以为不同区别的呼叫分配不同的SBC从而实现电话费用节约的目的

![image-20200611113114323](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611113114323.png)

接着创建PSTN Usage,  需要注意它不能新建，只能在Global下面不断地增加，同时它只是一个标识，用于标识这条路由的用途。为了简单起见，我们只创建 CN-Beijing-Local一条即可，即标识了这是一条北京本地电话的路由

![image-20200611112703047](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611112703047.png)

按着这个做法，我把所有的路由都做出来了，请参考

![image-20200611115535482](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611115535482.png)

做了这么多工作，就是来最后创建Voice Routing Policy的，只要简单的为新建的VRP指定PSTN Usage即可，但其中的逻辑一定要搞清楚哦，如下：

> 例如，下图中的VRP它含有全部PSTN Usage，说明这条路由策略可以打电话到任何地方
>
> 如果你想做一条只能打国内电话的VRP呢？简单把CN-Beijing-International删除即可
>
> 所以VPR就是用于做语音权限的控制的

![image-20200611115740970](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611115740970.png)

接着为用户分配VRP策略：

![image-20200611115838759](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611115838759.png)

最后我们来为用户分配Phone System 许可，打开企业语音，分配电话号码，这三个任务都需要使用命令来完成

登陆Teams Powershell

```
$String = "yourpassword"
$username = "tangx@contosso.com"
$TenantDomain = "contoso.onmicrosoft.com"
Import-Module SkypeOnlineConnector;$PWord = ConvertTo-SecureString -String $String -AsPlainText -Force;
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $username, $PWord;
$CSSession=New-CsOnlineSession -credential $Cred
Import-PSSession $cssession -AllowClobber
```

分配许可

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611134103450.png" alt="image-20200611134103450" style="zoom: 33%;" />

打开企业语音，分配电话号码

```
$user = "tangx@contoso.com"
Set-CsUser $user -OnPremLineURI 'tel:+861088888888'
Set-CsUser $user -EnterpriseVoiceEnabled $true
```

最后，我们就可以在Teams上面看到Teams Dial  Pad的出现，也就意味着在Teams端的配置完成了，所有这些操作我们都可以申请一个国际版的Office 365进行测试，就算没有Phone  System许可也是可以的，因为大不了启用不了EV，不影响我们实战操作，最终的效果如下：

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200611134153596.png" alt="image-20200611134153596" style="zoom:50%;" />