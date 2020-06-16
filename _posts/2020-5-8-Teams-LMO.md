---
layout:     post
title:      浅谈MS Teams Direct Routing中的本地媒体流优化技术
subtitle:  
date:       2020-5-8
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

### 回顾Direct Routing相关概念

这是一项新的Direct Routing相关的技术：Local Media Optimization，本地媒体流优化，因为它跟另一些技术相关联所以在讲Local Media Optimization之前，我们先来回顾一些知识点：

- **Microsoft Teams Direct Routing:** 直接路由, 是指 SBC 将来自MS Phone System 的Teams Call 路由到PSTN网络的一条SIP中继，所以说Direct Routing本质来说就是一条SIP Trunk，DR只不过是微软给的一个产品名字。虽然企业中的MS Teams Client可以通过Microsoft Calling Plan 来呼叫到PSTN网络（这是另一种M365提供的SaaS电话服务），但大部份企业更大可能会选择自己现有的PSTN运营商以及使用自己的直线号码DID进行语音落地，以提供更多的附加价值（例如降低成本、提高可用性或维持现有的运营商合同）这也是为什么需要Direct Routing的一些原因。

  ![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb46.png)

- **Media Bypass:** 媒体旁路，它提供把Teams Call中的媒体流保持在本地网络的能力，而不是将其发送到M365云上，以便提高呼叫的可靠性与高质量。

  ![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb56.png)

### 关于语音质量的挑战

MS Teams在推出Direct Routing的时候已经预想到会有这样两个关于语音质量的挑战：

第一，让Teams的媒体流保持在Teams Client与本地SBC之间，而不是把媒体流直接送到M365云上面。这种做法，我们称之为“媒体旁路 Media Bypass”，它对于缩短呼叫建立时间、通过避免媒体流向M365, 提高呼叫的可靠性起到至关重要的作用。从下图的红线（媒体流向）可以看出，启用媒体旁路之后，媒体流会跳过M365直接流向本地的SBC，从而大大提高呼叫质量与可靠性。

![Shows signaling and media flow with media bypass](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/direct-routing-media-bypass-2.png)

若没有媒体旁路的情况是

![Shows signaling and media flow without media bypass](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/direct-routing-media-bypass-1.png)

第二，微软要求媒体旁路需要使用ICE Lite，而ICE Lite 这项技术需要为SBC分配一个没有做NAT的公网IP，同时内网的Teams Client可以访问SBC的公网IP（简单来说就是需要把SBC暴露在公网上面）因为安全性问题，这对于大部分企业来说是做不到的，为了解决这个问题，微软推出了一个全新的媒体旁路架构：Local Media Optimization，本地媒体流优化。

> ICE Lite是什么？ICE的目的就是为了发现哪一对候选地址的 组合可以工作,并且通过系统的方法对所有组合进行测试(用一种精心挑选的顺序).

### [Local Media Optimization](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-media-optimization) (LMO)

本地媒体流优化，它是基于在媒体旁路上面的新能力，主要用来解决上述两个挑战，并且带来两个主要的作用：

\- 能够为Teams Call 提供最优的媒体路径连接到的本地SBC（就是媒体旁路的核心作用）

\- 允许SBC可以使用内网IP地址，并且通过NAT的方式与M365对接，让它们之间的媒体流可以驻留在企业内网而不需要流经M365（重点~！）

### LMO的使用场景

在之前，拥有多个站点的企业在规划他们的Direct Routing的时候会有这样两个选择：

1）为不同的站点分别使用独立的本地SBC，再通过Direct Routing来连接MS Phone System，这种方法可以使用传统的媒体旁路来把媒体保持在本地，但缺点是你要配置，监控，维护多个站点的设备与链路，例如：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb24.png)

2）在企业的中央站点只配置一台中央SBC来与MS Phone System 对接，其它站点的SBC与这台中央SBC对接，那些用户的媒体流都是先通过中央SBC再流向各自的SBC，优点是你可以不用维护太多的SBC，但缺点明显是呼叫质量一定不会太好的。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb32.png)

以下是本地媒体流优化两个典型应用场景：

A）【单台Central SBC的场景】企业的内网Teams用户想使用媒体旁路技术，但是他们又不可以直接访问SBC的公网IP。取而代之就是使用SBC的内网IP，LMO会基于Teams Client所在的位置来确定这个Optimal IP, 并把它写在SIP信令当中（SDP）

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb8.png)

B）【Proxy SBC的场景】在企业中，并不是所有地区的SBC都有能力与MS Phone System对接，在其它地区的Teams用户若想进行本地的语音呼叫时，就会使用当地的SBC（我们称之为Downstream SBC）与Proxy SBC对接，它的媒体流路径就会变得非常的长，例如下图架构中有三个国家（Singapore, Vietnam, Indonesia）只有SGP Site使用Direct Routing与MS Phone System对接，SGP SBC这时作为Proxy SBC.

当在Vietnam Teams Client想要进行语音呼叫时，之前的媒体流路径是 Teams Client >> Phone System >> Proxy SBC >> Vietnam SBC >> PSTN

而LMO可以解决这个问题，呼叫信令会通过Proxy SBC传送到本地SBC，但媒体流会保持在当地，这样就允许企业可以只有一台Proxy SBC 与MS Phone System对接，其它本地的SBC与Proxy SBC对接即可，降低维护成本之外还不影响呼叫质量。

媒体流路径会缩短成 Teams Client >> Vietnam SBC >> PSTN

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb15.png)

### LMO的两种模式

最后，我们来介绍一下本地媒体流优化的两种模式：Always bypass mode 与 Only for local user mode，合理使用这两种模式可以更好地优化Teams的呼叫路径：

- **Always bypass mode** : 无论Teams Client 位于哪个地区/子网（当然是后台定义好的子网网段）都会跳过MS Phone System 与本地的SBC，媒体流直接通过当地的SBC出局到PSTN网络；

路径是这样的 Teams Client >> Vietnam SBC >> PSTN

所以这种模式适用于：当Vietnam与Indonesia子网之间的网络是高速稳定的，那么Vietnam与Indonesia的Teams用户都可以使用这种模式。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb35.png)

- **Only for local user mode**：若Vietnam配置这种模式，则只有VN子网的用户媒体流会直接流经VN SBC，其他情况（在企业外部，在Indonesia子网，在SGP子网）的用户媒体流都会通过Proxy SBC的

VN子网的媒体流路径： VN Teams Client >> Vietnam SBC >> PSTN

IDN子网的媒体流路径： IDN Teams Client >> SGP SBC >> Vietnam SBC >> PSTN

SGP 子网的媒体流路径： SGP Teams Client >> SGP SBC >> Vietnam SBC >> PSTN

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb23.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb2.png)

最后，重点是所用到的SBC必须经过认证的SBC，以及搭配在Teams后台的配置才能实现，具体请参考 [Configure Local Media Optimization for Direct Routing](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-media-optimization-configure#outbound-calls-and-the-user-is-in-the-same-location-as-the-sbc-with-always-bypass) 

在下一篇文章中，我会讲述【单台Central SBC的场景】下的具体配置过程，这个场景我个人认为是比较有现实价值，而且是比较容易落地的方案。

在参考奥科的配置文档之后，认真思考【Proxy SBC的场景】这个场景的一些问题：

1）从理论上来看，的确是优化不少媒体流的路径，但配置难度比较大（Teams上面的站点信息配置，两台SBC之间的对接配置，Proxy SBC与Downstream SBC的LMO配置），相比【单台Central SBC的场景】来看，里面的每一台SBC的配置基本上类似的，维护起来比较简单。

2）分支站点完全可以直接使用认证SBC与MS Phone System对接，各自的站点走各自的SBC出局，各不影响。

3）如果使用【Proxy SBC的场景】的话，就是所有鸡蛋放在一个篮子里面，一旦Proxy SBC出现故障，所有用户的语音服务都受到影响。

4）必须使用【Proxy SBC的场景】的情况是分支站点缺乏公网IP资源 或 没有本地的Internet 那就需要用Proxy SBC来解决，只是现实的情况不是特别可能吧？

### 相关参考

- [Local Media Optimization for Direct Routing now available](https://www.linkedin.com/pulse/local-media-optimization-direct-routing-now-available-muravlyannikov/) 
- [Local Media Optimization for Direct Routing](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-media-optimization) 
- [关于ICE的比较好解释](https://www.cnblogs.com/pannengzhi/p/5061674.html) 