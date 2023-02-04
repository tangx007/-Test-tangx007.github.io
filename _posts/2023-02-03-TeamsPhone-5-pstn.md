---
layout:     post
title:      Teams phone Deployment Playbook 系列文章
subtitle:   启用PSTN语音的四个方案
date:       2023-02-03
author:  Nemo Tan
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams Voice 
- Teams Phone 
---

# 前言

PSTN - 公共电话交换网络, 通俗地说就是使用手机打电话的网络。这也是Teams在企业通讯领域的一项重要功能之一，让Teams连接到PSTN就可以让Teams客户端可以打电话（这也是以前Skype for Business / Lync用户所熟知的功能之一）。为了数字化转型，为了迁移私有云到公有云，为了IT系统的更新迭代，大部分的SfB 客户都不得不把他们的系统升级到Teams，随之而来的就是通讯系统也要同步迁移到Teams上面，这是给IT Guys带来一些挑战。

当然还有相当一部分客户是从其它的通讯系统迁移到Teams Voice方案的，通过这样的整合来精简IT系统，节省多套系统运行的维护成本。

所以，本文会介绍在Teams中接入PSTN网络来实现语音呼叫（da dian hua）的四个解决方案，供学习/围观/参考。同时，当你要部署Teams Phone的时候，也必须要考虑到它如何接入到PSTN，这也是你在Teams Phone实施项目所必需要关注的技术面。

如下图，简述了四种Teams Phone如何获得PSTN能力的解决方案，如Calling Plan, OC, Direct Routing (托管、自建)。在中国，因为受到监管政策的原因，我们大部分使用的是Direct Routing方案。

![image-20230203205505841](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203205505841.png)

# Teams Calling Plan 呼叫计划

第一个要介绍的方案是Teams Calling Plan, 如下图，如何连接到PSTN以及号码的维护等等都是由微软负责，对于Teams用户来说，只需要交了钱、分配了Calling Plan与Phone System 许可即可立即使用Teams语音。这是最简单最快捷最省心的方案，但是在中国，中国香港，中国台湾 都不适用，这也是这个方案最明显的缺点。

> 适用地区：https://learn.microsoft.com/en-us/microsoftteams/country-and-region-availability-for-audio-conferencing-and-calling-plans/country-and-region-availability-for-audio-conferencing-and-calling-plans

![image-20230203162802318](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203162802318.png)

它的部署是如何的快速的，请往下看。

首先，在M365管理中心当中，确保我们已经购买了Calling Plan许可，并且分配了给用户。

![image-20230203163338792](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203163338792.png)

回到Teams 管理员中心，进行Voice >> Phone Numbers，这里就是Calling Plan的号码管理界面，尝试新增一个号码。

![image-20230203163443377](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203163443377.png)

如下，按你所在的地区或国家选择即可，本次我们是要为用户申请号码，所以选User Subscriber。你能申请多少个电话号码取决于你有多少个Calling plan许可（例如我有20个许可，则可以申请20个Calling Plan电话号码）

![image-20230203164330560](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203164330560.png)

![image-20230203164506366](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203164506366.png)

申请完成之后，可以即时分配号码给Calling Plan用户，并使用。

你查找的用户必须是分配了Calling Plan许可的，否则你是无法分配号码给他。例如我已分配许可给AdeleV了，所以能选中并分配。

![image-20230203165028954](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203165028954.png)

到此，第一次操作的我，只用了约10分钟的时间即完成了Calling Plan号码的申请与分配，比起Direct Routing快速不知道多少倍。

![image-20230203222442008](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203222442008.png)


# Direct Routing 直接路由

第二个要介绍的就是Teams Voice最传统最复杂最灵活的方案 - Direct Routing，我们需要一台SBC（自建 或 托管）来通过SIP Trunk的方式来连接Teams Phone System，以及通过SIP Trunk / 数字中继 的方式来连接运营商的PSTN网络。

我来介绍以下这个Direct Routing架构图：

1. 最左边的Teams 客户端还是按之前的方式登陆上Teams，但他们已经被分配了Phone System 许可，因为他们需要有语音的能力。
2. 这时候，需要一台Teams认证的SBC（SIP会话边界控制器，其实充当了语音网关的角色）来作为中间人让Teams Voice落地，这个对接微软称之为 Direct Routing，本质上就是基于TLS的SIP Trunk链接。这个链路的对接需要做很多的工作，例如Teams语音部分的配置、公网证书申请、公网DNS、防火墙、SBC配置....与此同时牵涉到的管理员也就多了。
3. SBC还要对接PSTN网络，可以通过SIP或数字中继或模拟线路，这部分基本上设备厂商会协助你完成。还有，若企业内部还有其它的PBX系统要连接（Voice Apps, Legacy PBX），SBC也可以一并完成这个工作，这样SBC突然就变成一个企业的语音中枢系统，让所有的通讯系统都能互通起来了。

> 详细的Direct Routing规划与配置，请参阅我的一个系列文章：
>
> Microsoft Teams Voice语音落地系列 -汇总帖  https://blog.51cto.com/nemotan/2420875

![image-20230203162837092](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203162837092.png)

Direct Routing虽说是复杂，但也一定程度上增加了它的灵活性，Calling Plan适用于要求比较小的客户，但对于特殊的行业，特别的场景，奇怪的架构，不同的需求来说，Direct Rouing可以说能在架构图上面画一上一朵花（复杂），因为它支持了Teams Voice所有的高级功能，如本地语音流优化(LMO)、媒体旁路(Media Bypass)、分支机构存活(SBA)、经济路由、集成其它PBX，这些都是Calling Plan做不到的。

![image-20230203162846855](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203162846855.png)

# Operation Connect 运营商连接

接着是另一种实现Teams语音的方式 - Operation Connect 运营商连接 (OC)，它跟Direct Routing的架构非常类似，对比一下前后两张架构图，本质上都是通过一台SBC来链接Teams与PSTN网络而已。

但对于OC来说，有以下这些点跟DR是不一样的：

- SBC改由Operator Managed了，也就是维护主体是由运营商负责。
- 对于客户/用户来说，SBC变成了一种服务，也是由运营商负责。
- 同样地，与PSTN的连接、电话号码的管理、语音质量的管理也由运营商负责。
- 最后，对于Teams的终端用户来说，他们只需要在Teams管理员中心分配电话号码即可以立即使用，就像Calling Plan那样简便。也就是说电话号码的维护（增加/删除/分配）都可以在Teams管理中心进行。
- 最后，Teams管理员需要首先向受支持的OC供应商提交申请，才能使用这项目服务。这对于那些不支持Calling Plan的国家/地区来说，也是一个不错的选择。

![image-20230203162813824](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203162813824.png)

（我没有实际申请过OC）但在这里演示一下大概的申请流程：

进入Operator Connect, 例如选中香港，它就支持以下运营商的OC服务，看看有没有在合作中的运营商，就可以去找销售聊了。

![image-20230204113121487](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230204113121487.png)

例如，点击进入TATA的申请

![image-20230204113210293](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230204113210293.png)

![image-20230204113247905](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230204113247905.png)

再次进入Phone Numbers页面，选中Region为香港，你会发现没有Operator = Microsoft (因为不支持Calling Plan)，所以对于香港，你只能使用OC服务了。

能不能这么说呢：OC = Calling Plan + Direct Routing

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230204113423568.png" alt="image-20230204113423568" style="zoom:67%;" />

# Microsoft Teams Phone Mobile 

最后要介绍的是基于Operator Connect的扩展版本 - Teams Phone Mobile，如果你来看以下官方介绍架构图的话，基本看不出一个所以然，所以你基本理解不了这是什么东西，为什么我会说是OC的扩展版本，那么以下图片仅供参考：



![image-20230203162828678](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203162828678.png)

为什么Teams Phone Mobile 是OC的扩展版本呢？

- 首先，它可以让公司所有的手机电话号码（SIM-enabled phone numbers）作为Teams的电话号码，即它与原生的手机整合了，Teams号码与手机号码同号。
- 要做到上述，必须要运营商的支持，也就是符合Operator Connect计划的要求，本质上就是运营商把相应的手机号码关联到SIM卡与Teams上面了。
- 所以后台架构还是基于OC的模式，与OC的不同在于，它使用的号码是公司所有的手机电话号码（SIM-enabled phone numbers）
- 优点：无论如何就能找到你、统一了所有办公设备的电话号码、提高员工的响应速度、更加安全与合规（因为手机的呼叫记录都保存在Teams上了）
- 但目前只支持四家运营的四个国家：美国、加拿大、瑞士、瑞典。

> With Teams Phone Mobile, a user’s SIM-enabled phone number is also their Teams phone number. 
>
> https://cloudpartners.transform.microsoft.com/practices/microsoft-365-for-operators/teams-phone-mobile
>
> https://learn.microsoft.com/en-us/microsoftteams/operator-connect-mobile-plan#benefits

![image-20230203162733054](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203162733054.png)

# 总结

以上简单介绍四种不同的方案来启用Teams语音呼叫能力，最简单最容易的是Calling Plan, 最灵活最复杂的是Direct Routing，次之 OC。

对于Teams，本质上使用SBC让Teams语音落地到PSTN，再基于实现方法的不同，使用技术的不同出现现时这四种方案。本文旨在让大家可以对于这个技术有一个全面的了解，以及其基本的概念，并没有深入探讨特别具体的内容。仅供参考。

![image-20230203221559585](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20230203221559585.png)

# 参考

- Teams Calling Plan

https://docs.microsoft.com/en-us/MicrosoftTeams/calling-plans-for-office-365

- Country and region availability for Audio Conferencing and Calling Plans

https://learn.microsoft.com/en-us/microsoftteams/country-and-region-availability-for-audio-conferencing-and-calling-plans/country-and-region-availability-for-audio-conferencing-and-calling-plans

- Teams Phone Mobile

https://cloudpartners.transform.microsoft.com/practices/microsoft-365-for-operators/teams-phone-mobile

https://learn.microsoft.com/en-us/microsoftteams/operator-connect-mobile-plan#benefits

https://learn.microsoft.com/en-us/microsoftteams/operator-connect-mobile-plan

https://learn.microsoft.com/en-us/microsoftteams/operator-connect-mobile-configure


------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />

------

微软最有价值专家是微软公司授予第三方技术专业人士的一个全球奖项。27年来，世界各地的技术社区领导者，因其在线上和线下的技术社区中分享专业知识和经验而获得此奖项。

MVP是经过严格挑选的专家团队，他们代表着技术最精湛且最具智慧的人，是对社区投入极大的热情并乐于助人的专家。MVP致力于通过演讲、论坛问答、创建网站、撰写博客、分享视频、开源项目、组织会议等方式来帮助他人，并最大程度地帮助微软技术社区用户使用Microsoft技术。
更多详情请登录官方网站：https://mvp.microsoft.com/zh-cn

<img src="https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi.png" alt="MVP_Logo_Horizontal_Preferred_Cyan300_CMYK_300ppi" style="zoom: 25%;" />

