---
layout:     post
title:      微软 Teams Meeting 系列(2) 
subtitle:  如何充分利用MS Teams Meeting
date:       2020-01-07
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- Teams会议系列文章
---

> 本文是基于MS Ignite 2019 MTG10 Get the most out of Meetings with Microsoft Teams 的课程视频整理而成的。

基于公有云的线上会议平台在最近两到三年内真是爆发了，据本人所了解的，从最早的微软的Lync Meeting, Cisco Webex, Skype for Business Meeting, 到现在的MS Teams  Meeting, Zoom Meeting,  阿里的钉钉，华为的Welink，科天云，腾讯云会议，小鱼易联等等各种各样的云会议平台遍地开花，百家争鸣，这些云会议平台的涌现都反映了大家在这个时代对于沟通效率，沟通成本，多地协作的需求是如此之大，同时竞争对手多了，才会有前进的动力，为大家带来更加好的会议产品。

在本系列当中，会详细介绍微软的Teams这款云协作平台，从之前的OCS->Lync->Skype for Business  到现在的Teams,  会议的音视频质量都得到非常大的提升（从多个不同的客户当中反馈过来），同时也增加了很多实用的沟通协作功能，本人认为在一场高效的线上云会议当中，与会者之间的协作程度与互动程度决定了这场会议是否有价值的一个重要标准，例如流畅的视频（不要幻想高清，带宽扛不住），清晰的音频（引入先进的音频编码：SILK），共享桌面，共享白板，背景模糊（AI），实时字幕(AI)，会议录制(MS Steam)，这一系列的功能给用户带来的是一场开得非常舒服，省心，再省心的会议。

所有这些就是本文所要介绍的Microsoft Teams Meeting, 充分利用它，为你带来前所未有的会议协作体验。

在MS Teams Meeting 中，会分为四种会议类型，分别为

- **事前已计划好的结构化会议**，利用Teams日历，Outlook日历事先预约好企业内部与外部参会者，推荐使用这种结构化的会议，它可以有完整的会议生命周期以帮助你召开高效的会议（会前，会中，会后）

- **临时会议**，也就是即兴会议，Ad Hoc Meeting：它可以在Teams日历中的Meet Now实现，它可以立即组织一场Teams会议而无须预约，但你需要手工增加与会者，不利用你把会议信息分发出去，特别是人数比较多的会议，不建议使用这种方式。

- **基于Teams频道的会议**：结构化会议的另一种形式，参与者默认为Teams频道中的成员，用于实时与频道成员进行协作与沟通。在我们的大中华区Teams社区月底会议中，就是使用了这种会议形式召开的，可以看看我之前组织的一场[会议录像](https://www.bilibili.com/video/BV1qA411t7be/)

- **线上直播**，Live Events, 这是一种特别的大型会议：与会者之间是没有即时的互动的，只能通过Q&A功能向演讲者提问，适用于培训，演讲，发布产品，宣讲等，可达1万人参与。

[![image](https://s1.51cto.com/images/blog/202001/07/495e5dfc908a9c37691227de54cc4374.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/3cc52b8166964cfa4743ba0e62edabaf.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

刚刚讲到事前已计划好的结构化会议时，提到会议的生命周期 Teams Meeting Lifecycle , 它分为：

- **会议前**，可以同与会者进行会前的沟通与协作，进行会议演示文档的准备等；同时我们还可以把相应的Teams Room 会议室预约上，实现对实体会议室的事前占用。
- **会议中**，使用Teams Meeting 中的各种协作功能进行流畅的会议，如桌面共享，微软白板，实时字幕，不同终端的接入（电脑，APP，PSTN接入等），会议笔记…
- **会议后**，大家回顾会议记录，在Stream 中在线查看会议录像而无须担心录像掉失（这个工作原理在后续的Blog中会有详述），通过关键字搜索特定的会议内容。

[![image](https://s1.51cto.com/images/blog/202001/07/2b589d9c405e81783006fe7fd3ac70cf.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/1f1b6e34394eea00c4cf1793ed3ca931.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

以下是一个关于会议前的沟通与准备，当预约好一场Teams Meeting 后，各参会者其实就可以利用Chat, Files, Notes, Whiteboard开始事先沟通，把该说的说，该准备的文档上传，审核，讨论

可能到开始会议的时候，大家就是确认一下没有问题就可以结束会议，非常高效。

[![image](https://s1.51cto.com/images/blog/202001/07/79aa0cd417140427571507fa9e5cb46d.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/72a8d18372d9c60ed75db1063662ec6a.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

以下会介绍一些在会议开始前（Pre joint meeting stage）准备的实用功能

进行一次test call，以确保扬声器与麦克风的工作是正常的：

[![image](https://s1.51cto.com/images/blog/202001/07/3343934ade4c4b4a562036c1e357410c.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/bbb70e16d17840b6704de759213c3531.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

在加入会议前，还可以打开一个非常高大尚的功能：背景模糊 blur，当你不想展示当前会议的背景时，这项功能非常实现，例如在家开会，在咖啡厅，在高铁上，等等。

[![image](https://s1.51cto.com/images/blog/202001/07/5f264aa869a81f82729b1efe979588bc.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/8b9e535b52b4bbae17e150976bd463f1.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

在Ignite2019大会上面，微软的工程师也为我们展示了另一项基于blur的内测功能：设定你的背景，在一次与微软同事的Teams Meeting当中我已经体验过这一神奇的功能

[![image](https://s1.51cto.com/images/blog/202001/07/7215eadac86e81807c3893c0fc561571.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/62e9a22aad32547277d928d6cc5a9a8e.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

接着，为大家展示也另一项跟AI有关系的功能：live caption 实时字幕，对于我们这些英语学不好的人来说是大大福音，以后再也不怕与老外开会了。

[![SNAGHTML205884](https://s1.51cto.com/images/blog/202001/07/c6d889b20bdf5a69144fbffae177dccf.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/81bb4c4a7f92afc4149922a9ab43ce5b.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

基本功能：share ppt

[![image](https://s1.51cto.com/images/blog/202001/07/f7db7c4ca46a7f0b7882674ca58ded33.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/384a78cc502d54b59516aefee14e8182.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

[![image](https://s1.51cto.com/images/blog/202001/07/0fdfacf63bbb3130a6bf696f309082f2.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/8b113068a37b374e2ed5c0748ab668e3.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

非常实用的功能：会议录像，同时它在把录像存放在Microsoft Steams 上面而不是在用户的本地硬盘，这样会议结束后，所以与会者都会收到会议录制完成的通知，而不用主持人把录像传播给他们，本人是认为很方便，很实用的功能。

[![image](https://s1.51cto.com/images/blog/202001/07/77f47eb5d0248a5ce1f22e2f5400ce0a.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/5e45ac399bc81b2c61ca99d1fc1c36d3.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

在上文中提到其中一种会议类型：基于Teams频道的会议，它是如何被召开的呢，如下：

[![image](https://s1.51cto.com/images/blog/202001/07/640a4038ca9dada13aa2559d2bff86d0.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/f4f475084b86603a0d6940516eafd0d5.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

[![image](https://s1.51cto.com/images/blog/202001/07/272049f602ad1ad226f1d702d70e6291.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/86dd3daf122eee66e5e0cdfcce46c103.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

因为Teams Meeting都是基于互联网的，如果没有互联网的话，如果加入Teams Meeting呢？答案是使用拨入式电话会议（audio  conferencing），一旦管理员为你分配了audio conferencing  许可并完成配置后，你组织的会议就会多出以下图片中的信息（包括了接入号与会议室ID），与会者通过拨打这个电话号码就可进入这个Teams  Meeting 了（之前接触过SFB的同学应该是非常熟悉不过的）

[![image](https://s1.51cto.com/images/blog/202001/07/3b55c1e0bca27f78dd3b1e965066a519.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)](https://s1.51cto.com/images/blog/202001/07/4e59331beae99434ff5103a9d06d6e9d.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

最后，介绍了一些主要的Teams Meeting 的概念与核心功能后，是不是对于它有一些初步的认识呢？用起来呢，才能感觉到它是如何为了提升协作效率而生的。

以下列举了一些为什么要使用Teams Meeting 的理由，供参考。

[![image](https://s1.51cto.com/images/blog/202001/07/72732c5c13f1924589970759f7b9c1aa.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)(https://s1.51cto.com/images/blog/202001/07/cdb4b477032ae6b9d0e1fd182e4176a1.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)