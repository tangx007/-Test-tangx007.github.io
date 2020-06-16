---
layout:     post
title:      Microsoft Teams Voice语音落地系列-1 
subtitle:  语音架构简述
date:       2019-6-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

> **本系列从51cto迁移过来，为武汉抗疫致敬~！**

在阅读本文之前，相信大家已经对Microsoft Teams这个产品有所了解或已经在用了，可以参考[@王远：Teams的前世今生](https://blog.51cto.com/scnbwy/2375777?from=timeline)的文章，大概的产品迭代如下图，可以看出微软的重心已经慢慢地跟着Satya的战略：mobile-first, cloud-first 转移到云端。直到现在的Microsoft Teams已经是一个纯云产品，同时也将未来几年时间替代Skype for  Business。

![image-20200613151153486](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151153486.png)

我在2019年初的时候，就直接把自己的Skype for Business帐号迁移到Teams上面使用，已经深深感受到Teams的各种功能（包括 消息，会议，呼叫与协作）比 Skype for Business实在太优秀了，而且音视频质量也有明显的提高。

但从SFB迁移到Teams 或 新用户直接使用Teams的时候，作为一款通讯与协作产品，都会有同样的疑问：
1）迁移到Teams之后，我还能像SFB那样打电话吗？
2）能跟公司现有的PBX互通吗？
3）能通过会议接入号加入Teams会议吗？
4）在Teams会议中能把手机用户拉进会议室吗？

我把这类问题归纳为Microsoft Teams语音落地（就是Teams的电话功能），所以本系列文章主要围绕这个话题展开的，同时这项技术也是客户从SFB迁移到Teams之前必须考虑的事情之一，不然迁上去后大家都不能打电话，这问题就大了。

首先要介绍Microsoft Phone System, 它是位于O365中的一套电话系统，使用它可以让Skype for  Business online 或者 Teams可以获得与本地SBC建立SIP  Trunk的能力，这样的话，Teams上面的呼叫就有机会路由到本地SBC了。

![image-20200613151549886](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151549886.png)

> 再引申一下：会话边界控制器 (SBC) 也许是  语音网关，因为本文并不是在讲通讯技术，所以简单理解为连接运营商PSTN网络的一个设备即可（通过SIP Trunk &  PRI都可以，但一般的企业都会用PRI-E1的方式来对接PSTN），那么最常见的SBC 或 语音网关 就是SFB时代的Sonus/Ribbon 与 AudioCodes 这两家的设备了，Sonus SBC1k/2k, SWe Lite, SWe Core, AudioCodes M1KB, v-SBC, E-SBC… 

到此为止，我们在O365上面有Phone System , 企业本地上面有SBC，那用什么方式连接起来呢？就是刚刚上文说到的SIP Trunk了，但微软给它起了个名字，也是本系列的主角：
Microsoft Teams Direct Routing，是能允许您将SBC连接Microsoft Phone System的一项技术。

那么通过Direct Routing, 我们可以直接让你的Teams用户能打电话出去，电话又能打进来，同时Teams也能与你本地的PBX系统互通了，说更简单点就是：我的Teams可以打电话啦。

现在不是流行BYOD吗？那么Direct Routing就是Bring Your Own SIP Trunk (BYOS)，下面我们来看看几种使用Microsoft Phone System的场景：

1）使用SBC前置：Upstream；如下图，本地的SBC与Phone System建立了Direct Routing的连接（SIP  Trunk），同时SBC也连接着本地PBX与本地PSTN网络。这样就可实现Teams  Voice中最主要，最基本的功能：电话的打进打出与本地PBX互联。

![image-20200613151619330](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151619330.png)

下图的红色与黑色分别说明了信令与媒体的流向，这种没有媒体旁路的方法让媒体流都从Phone System上面绕一圈再回来，而没有直接流向SBC。

不知道大家有没有发现这样的媒体流向的问题？红色的媒体流经过了两次Internet网络，语音质量可想而知了吧？

![image-20200613151641898](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151641898.png)

但是当启用了媒体旁路后，媒体流就直接在内网流向SBC，大大提升了语音的质量和外网的不稳定性，非常高兴的是最近微软已经把Media Bypass在主流的认证SBC上面都支持了：https://docs.microsoft.com/en-us/MicrosoftTeams/direct-routing-border-controllers

![image-20200613151746831](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151746831.png)

从下图可以看出，主流的两家认证SBC厂家都支持媒体旁路了：

![image-20200613151715066](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151715066.png)

2）使用Phone System + CCE+SBC的方案，可以使Skype for Business Online 用户实现电话的打进打出（即本地语音落地）。

以下的Cloud PBX已改名为Phone System，从下图可以看出要实现SFB云端用户的语音落地要增加一个CCE的角色（边缘服务+媒体中介），相对于Teams的Direct Routing来说就是麻烦了一点点, 增加了本地的运维压力

![image-20200613151801369](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151801369.png)

重要的东西来了：SFB online要语音落地，必须要有CCE (一套云连接器) ，同时不能使用Direct Routing。如果强制要用SFB Online来使用Direct Routing的话，会出现有时无法打电话的问题，所以信令类似于以下：
Teams---> Phone System --> Local SBC
SFB Online ---> Local CCE/Onprem SFB ---> Local SBC
请参考这篇文章: https://msunified.net/2018/05/27/microsoft-teams-direct-routing-explained/

3）SFB 本地环境与SFB Online的混合部署，跟上面的CCE方案类似，只不过把CCE换成本地的SFB, 从而为云端SFB用户提供PSTN的能力。同时，因为本地SFB的存在，可以提供基本上全功能服务。缺点就是本地运维压力是最重的。

![image-20200613151813304](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200613151813304.png)

最后汇总一下本节

1) 介绍了Teams与SFB online语音落地的几种架构。
2) 对于Teams来说，媒体旁路的重要性。
3) 简单介绍了Microsoft Phone System 与 Direct Routing , 其实就是SIP Trunk

未来的章节中，将会深入介绍Teams Direct Routing规划与配置过程中的前置准备，Teams Voice Routing配置，SBC配置，测试等。