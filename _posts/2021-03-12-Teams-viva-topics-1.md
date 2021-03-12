---
layout:     post
title:      初步配置Microsoft Viva Topics
subtitle:  Viva Topics
date:       2021-03-24
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Microsoft Viva
- Teams

---

上一篇文章有简单介绍整个Viva产品，今天介绍一个Viva Topics是如何设置。

可以参考官方文档: https://docs.microsoft.com/en-us/microsoft-365/knowledge/set-up-topic-experiences

以下是设置过程中一些实战截图：

因为Viva Topics是一个已经发布的产品，所以它需要许可才能使用，但是可以有一个月的25用户使用许可。

进入M365的管理后台，并按如下操作

![image-20210304122130977](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304122130977.png)

开始一个月的25个用户的免费试用

![image-20210304122311564](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304122311564.png)

购买完成许可后，可以为需要使用Topics的用户分配许可

![image-20210304125135067](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125135067.png)

在“安装”中，找到“将人员与知识相关联”，可以看到默认是尚未启动的

![image-20210304125451166](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125451166.png)

通过设置 ‎Viva Topics‎，帮助用户在 ‎Office‎ 应用、‎SharePoint‎  和搜索结果中发现感兴趣的主题。你可以控制其使用方法，以及谁可以查看和编辑主题，在哪里显示主题。‎Viva Topics‎  将遵循你现有的隐私、安全和合规设置。

![image-20210304125508616](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125508616.png)

![image-20210304125603750](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125603750.png)

谁可以查看主题

主题详细信息显示在主题页面上，在搜索结果中以及在 SharePoint 页面等内容中突出显示主题。用户只能查看有权访问的文件和页面中自己已经发现的主题。

![image-20210304125614716](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125614716.png)

![image-20210304125700868](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125700868.png)

主题中心本质上是一个 SharePoint 网站，在其中，用户可以具有贵组织相关知识的个性化视图，知识管理员可以管理主题。主题页面位于此处。

![image-20210304125749596](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125749596.png)



![image-20210304125758309](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304125758309.png)



![image-20210304130031813](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304130031813.png)

完成Viva主题中心的基本设置后，我们回到：安装 > 将人员与知识相关联 可以对刚刚设置的主题进行管理

例如主题中心的名称只会有一个，所以刚刚我设置的NemoViva就不合适了，那可以修改为以下：Contoso公司的主题中心

![image-20210304163906362](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210304163906362.png)

建立完成之后，你需要等待一段长长的时间让后台把网站创建出来，其实本质上Viva Topics是一个SharePoint网站，当你看到以下页面的时候，Topics就被成功创建出来了，它会通过AI把你感兴趣的信息显示在上面。

![image-20210311080347440](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210311080347440.png)

现在的页面是空白的，因为在组织里面我们还没有建立Viva Topics，点击上面的“新主题页面”，新建一张Viva Topics页面，里面包括主题名字，相关人员，相关文件 都可以连接到这张Topics页面

![](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210311111321821.png)

![image-20210311111555560](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210311111555560.png)

成功发布之后，主页上面就会显示出来这个主题卡面，同时这个主题相关方都会有同样的显示，从而方便大家为同一个目标或主题进行沟通与管理。

![image-20210311111803830](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210311111803830.png)

# 排错

在为员工使用Viva Topics的时候，他们必须分配Viva Topics的许可，不然会报错。

![image-20210311081105841](https://cdn.jsdelivr.net/gh/kristofftan/kristofftan.github.io/img/image-20210311081105841.png)


