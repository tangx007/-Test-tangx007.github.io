---
layout:     post
title:      使用 tel, callto, sip 协议快速调用MS Teams客户端
subtitle:  
date:       2020-01-11
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-other
- 
---

当Microsoft Teams 成为了你的沟通，协作，通讯的核心生产工具时（特别是从Skype for business迁移过来的重度用户），我们需要方便快速地联系到对方，以下提供了一个比较实用的使用场景：

在很多时候，我们都会收到类似以下签名档的邮件，然后只需要点击一下即可以联系到对方，这种功能叫Click to call, Click to tel….

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb18.png)

接着，Teams Client就调用起来，并提示是否拨打电话了；如果你点击的是 sip:xxxxxxx@xxxxx.com 的话，就会调用Chat窗口进行聊天；

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb7.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb10.png)

方法如下：在Default Apps –> Choose default apps by protocol

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb14.png)

设置callto 协议的默认调用程序

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/sipimage_thumb15.png)

设置sip, tel协议的默认调用程序，即可；

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image_thumb17.png)



