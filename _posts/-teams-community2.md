---
layout:     post
title:      Microsoft Teams 大中华技术社区活动
subtitle:  四月份的功能更新
date:       2020-4-27
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

首先是，在Teams的日历中创建Teams会议后，会议组织者可以通过会议选项来为PSTN与会者单独更改是否使用等候大厅的功能。我认为这是一项安全更新，因为默认的会议策略是所有与会者都可以跳过等候大厅直接进入会议室，有了这项更新之后，Teams会议就可以控制来自电话接入到会议室的与会都是权限（当然前提是这个Teams会议是有电话接入能力的，就是分配了Audio Conf许可）

![image-20200615135536026](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200615135536026.png)

接下来是在会议选项中可以单独设置PSTN与会者进入/离开会议的语音广播，当  PSTN与者者加入或离开会议时，会播放一个通知声音，这也应该是一个安全改进，增加会议的安全性以及灵活性，因为当我们精力集中在会议中时，会不知道有准进来会议室了，特别是如果把会议ID与会议接入号泄露了，其它人就可以直接进入会议室，通过这个广播以及刚刚说到的等候大厅来对这些PSTN与会者进行控制的话，可以大大提高会议的安全性。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com1111)

然后就是 Teams的本地音频流优化 – 这是一个在Teams语音方面的一个重大更新，它可以大大减少Teams在传输媒体路径。

我们来设想一个场景：一家大型企业，已经建设好一套整体的电话系统，现在想使用Teams来进行语音落地（就是通过Teams打电话或与本地的电话系统互通），这样的话Teams的媒体流会流经这样几个系统：客户端 > Teams > 认证的SBC > 本地电话系统 >  PSTN网络，这样的路径比较长从而导致语音质量会有所下降，那么这次的更新引入：Local Media  Optimization的技术来缩短这个媒体流到最小的路径，如客户端 >本地电话系统 >  PSTN网络，这样的话可以大大增加Teams在电话语音上面的质量，提高用户的满意度。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com222)

会议组织者将能够结束会议，以便所有参与者正式结束会议

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com333)

强制让外部与会者进入等候大厅

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com444)

接下来就是Teams会议可是以自定义背景，在疫情期间在家办公更加凸显它的作用。在召开远程视频会议的时候，家里的各种背景/隐私都会毫无保留地出现在会议室里面

其实还可以自定义背景的，就不展开了，可以参考我之前写的一个短文

> 在Teams Meeting中自定义你的背景（不挑硬件）
>
> https://blog.51cto.com/nemotan/2488666

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com555)

然后就是 Teams的本地音频流优化 – 这是一个在Teams语音方面的一个重大更新，它可以大大减少Teams在传输媒体路径。

我们来设想一个场景：一家大型企业，已经建设好一套整体的电话系统，现在想使用Teams来进行语音落地（就是通过Teams打电话或与本地的电话系统互通），这样的话Teams的媒体流会流经这样几个系统：客户端 > Teams > 认证的SBC > 本地电话系统 >  PSTN网络，这样的路径比较长从而导致语音质量会有所下降，那么这次的更新引入：Local Media  Optimization的技术来缩短这个媒体流到最小的路径，如客户端 >本地电话系统 >  PSTN网络，这样的话可以大大增加Teams在电话语音上面的质量，提高用户的满意度。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com666)

最后一个更新是：Policy Package 

如果你的组织有很多不同角色的用户群体，你可能会为不同类型的用户组分配不同的策略（例如消息策略，会议策略，应用安装策略，呼叫策略…）日程月累下来这个工作量非常大，这次的更新中Teams的管理后台引入了 Policy Packages  把各种策略打成一个包，我们可以为不同角色的用户群体定制不同的策略包（例如针对研发部门，我们可以在策略包中创建一个禁止云录制的会议策略，同时创建一个启用“编辑已发消息”的消息策略，最后分配到研发组上面），以后我们只需要管理这个包中的策略就可以应用到所有研发用户上面了。

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com777)

分用户分配策略包

![image](C:\Users\Nemo\Documents\GitHub\tangx007\img\com888)

Roadmap链接

https://www.microsoft.com/en-us/microsoft-365/roadmap?filters=Microsoft%20Teams%2CLaunched%2CRolling%20out

> 目前Teams社区有来自日本，新加坡以及美国的朋友，社区每月会组织进行Teams相关技术分享，同时每周五也会有由微软Office365高级产品经理Ares Chen进行的“Teams Friday下午茶”技术讲座分享，当然有疑问或建议也会有专家及时解答！
>
> *可以通过以下链接填写相关信息，我们会自动将你加入社区*
>
> *https://aka.ms/jointeamsdevcommunity*
>
> *可以通过以下链接收看“Teams Friday下午茶”技术分享*
>
> *https://aka.ms/teamsfriday*



