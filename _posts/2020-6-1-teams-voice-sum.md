---
layout:     post
title:      Microsoft Teams Voice语音落地系列 -汇总帖
subtitle:  
date:       2020-6-1
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---

简单汇总一下这个Teams语音落地系列各篇文章，本系列的目的旨在实现Teams的电话功能这一重要的功能是如何从准备，配置到实测成功的手把手指导，如果您可以成功实现其实是非常简单的一件事情，但因为涉及多个系统的配置，联调与排错, 所以一定程度上说是比较麻烦的，所有非常有必要通过本系列文章为各位一次过讲明白

### Microsoft Teams Voice语音落地系列-1 架构简述: https://blog.51cto.com/nemotan/2377504

以下是本系列所用到的整体架构图

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/sum1111b38109da339c85a2e672ac7df855d6a3.png)

 

### Microsoft Teams Voice语音落地系列-2 实战-前置条件准备：https://blog.51cto.com/nemotan/2378389

还记得有哪些吗？非常重要哦

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/sum222d2acecc7fba56d87f4176645c17d3819.png)

### Microsoft Teams Voice语音落地系列-3 实战:拨号计划的配置:https://blog.51cto.com/nemotan/2383423

介绍了在Teams上面如何把用户输入的号码转成E.164标准的方法，也是很重要的哦

![image-20200618091719923](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200618091719923.png)

### Microsoft Teams Voice语音落地系列-4 实战:Teams语音路由规划与配置: https://blog.51cto.com/nemotan/2384879

通过语音路由，呼叫就可以正确地从Teams呼叫到本地的语音网关出局了

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/sum333e002678c7009f06d048e6b6f4dbc0108.png)

### Microsoft Teams Voice语音落地系列-5 实战: Sonus语音网关配置：https://blog.51cto.com/nemotan/2409081

最后，通过配置本地的语音网关并测试，搞定~！

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/sum44441b88ebc196fddd5a5760d118cdb6e958.png)

### Microsoft Teams Voice语音落地系列-4-1实战-GUI管理Teams DR  https://tangx007.github.io/2019/12/09/teams-voice4-1-DR-GUI/

因为Teams的语音落地一直是通过命令来完成并没有一个友好的配置界面来完成这项目任务，所以国外牛人就自己开发了一个软件来实现了，这文章会跟大家介绍一下这个软件

![image-20200614094437688](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614094437688.png)

### Microsoft Teams Voice语音落地系列-6 实战-自动助理实现免费的 IVR https://tangx007.github.io/2020/03/23/teams-voice6-aa/

Teams AA 的出现 (Cloud auto attendant)，终于可以原生地实现“二次播号”，结合之前详细讲述的Teams Direct Routing 就可以实现这样一个非常重要的场景：

1. 客户A拨打Contoso.com 的总机 +86 21 12345678，这个呼叫通过PSTN来到企业的SBC。
2. SBC通过Direct Routing 把这个总机号码路由给Teams。
3. 因为总机号码已经事先分配给Teams AA , 所以欢迎语音播放出来：“欢迎致电Contoso 信息技术有限公司, 请直播分机号，查号请播0。
4. 客户输入 4321分机号，4321的Teams用户振铃，双方接通。

如下图：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ivr2222)

没有 IVR 场景的话，企业是很难把电话系统整体搬到Teams 上面来的，所以说这项功能的实现是Teams 项目完美与否的重要条件之一哦

### Microsoft Teams Voice语音落地系列-4-外传1 实战-使用Teams后台管理中心TAC配置拨号计划 https://tangx007.github.io/2020/06/09/Teams-voice4-2-DialPlans/

在大约一年前，我写过关于Teams语音落地的系列文章，详细把整个架构与配置过程都过了一遍，之前看过我的文章的同学会发现为什么配置这个本来就复杂的拨号计划与语音路由需要用到命令，为什么没有一个友好的配置界面来完成这些任务。

直到最近，这些配置终于是可以通过TAC界面完成了，这文章将会带着大家来完成这一部份的配置。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200609161049.png)

### Microsoft Teams Voice语音落地系列-4-外传2 实战-使用Teams后台管理中心TAC配置语音路由策略 https://tangx007.github.io/2020/06/10/Teams-voice4-3-VoiceRoutingPolicy/

