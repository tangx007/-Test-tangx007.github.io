---
layout:     post
title:      Microsoft Teams Voice语音落地系列-5
subtitle:  实战: Sonus语音网关配置
date:       2019-7-19
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Voice
- 
---



上一节中我们在Teams上面用命令配置好了Voice Routing Policy并分配给用户，理论上这时他的Teams UI上面的拨号盘就会出现。同时我们也建立好了PSTN Gateway,  这样子 Phone System >>> 本地语音网关的SIP Trunk 就做好了，从Teams  管理员中心上面可以看到这条SIP Trunk的状态，如下：

> 20200614 注：下图为201907月的时候截出来的，相对于现在的Voice菜单，当时的功能非常少，就只有Direct Routing and Call park两项功能，一年之后改进了非常大
>
> 这可是微软在后台不知不觉地完成的，我们可以感受到这些基于公有云的SaaS服务的更新速度比之前的SFB系统快不少。

![image-20200614100434983](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614100434983.png)

接下来这一节，我们集中精力讲述在语音网关侧的配置，这次我会选用Direct Routing的认证网关之一：Ribbon SWe Lite  进行配置 （如何安装? 请参阅文章https://blog.51cto.com/scnbwy/2385961 ，  安装过程非常简单，若需要在本地安装的话，可联系我取得镜像）

其中的内容包括建立语音网关至Phone System的SIP Trunk，信令组，语音路由，转换规则等，但让我们先来回顾一下Teams Voice的整个架构及前置条件准备：
（我始终认为集成系统的架构主导着具体配置，而具体配置只不过为系统架构服务而已，不了解整体架构只会永远的迷路）

![image-20200614100843187](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614100843187.png)

### 回顾前置条件

1)  权限与管理员准备：O365管理员/Teams管理员；SBC管理员；本地Skype管理员；网络管理员；DNS/CA管理员；
2)  许可准备：E3+Phone System Lic or E5 lic; SBC中必要的SIP Lic;；
3)  连接到SFB Online Powershell；
4)  连接到Office 365 Powershell；
5)  已经在O365注册了，并已激活的公司域名；
6)  准备好用于SBC的公网IP与公网FQDN，并做好了公网DNS A记录；
7)  准备好含SBC FQDN的公网证书；
8)  已经制作好相关的防火墙规则，同时网络管理员确认正确；
9)  使用了认证的SBC，并已升级至最新的固件版本；
10) 若对安全有更好的要求，可以在SBC中配置ACL；

现在我们需要用到第6到第10点的条件准备了，都准备好了吗？
然后大致的整体架构图就会长这样子：
1)  Teams Audio Call通过Teams送给Phone System
2)  Phone System与本地语音网关建立SIP Trunk(tls), 即所谓的MS Direct Routing
3)  Phone System就可以把Teams Audio Call送到本地语音网关
4)  语音网关再对接PSTN网络之后，就可以实现语音的落地了

![image-20200614100856220](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614100856220.png)

### 导入公网证书

**I.    首先导入公网证书到SBC当中**

在之前的章节中，我们已经申请完公网证书，并取得了cer后缀的证书文件了，现在把它导入到当时您生成CSR的电脑当中，目的是为了拿到证书的私钥 Private Key.

打开MMC, 并按以下图片打开电脑的证书管理控制台：

![image-20200614100909040](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614100909040.png)

按如下图导入从公网证书颁发机构申请下来的证书（后缀名为cer的证书）：

![image-20200614100927898](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614100927898.png)

导入成功后，你会在证书的图标上面看到一把小钥匙，即表明我们导入的证书是带有私钥的：

![image-20200614100945042](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101016351.png)

最后按如下方法把带有私钥的公网证书导出，以供后续导入到SBC当中:

![image-20200614101001374](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101016351.png)

![image-20200614101016351](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101016351.png)

最后我们还要导出所有的根证书与中间颁发机构的证书以base64的格式导出，

> 到此为止，如果同学们对数字证书加密体系不熟悉的话，*可以参考这文章：https://blog.51cto.com/ucworld/434901，文中讲的是数字证书的原理，那么公网证书也是一样的，需要两种证书组成一条证书链（根证书+中级证书+自己的数字证书）），*

如下例子说明了证书的其中一个重要作用：

> *经过4年的学习，我们终于毕业了。表明我们毕业的东西除了很多很多之外，其中一个就是我们有了一个红色的小本本-毕业证。在我们面试工作的时候，我们就会把毕业证展示给对方，并加上一句，你不相信我的能力，但是你应该相信这个学校吧。于是，他点点头说，你毕业于这所名牌学校，应该能力可以，那就接受你吧，于是我们就开始工作了。另外一个例子就是：还有许多个人和企业都信任合法的驾驶证或护照。这是因为他们都信任颁发这些证件的同一机构--政府，因而他们也就信任这些证件。*

所以如下图需要导出两张证书，最顶端的那张为根证书，第二张为中间证书颁发机构的证书；
按之前的方法把它们导出，备用。

![image-20200614101036978](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101036978.png)

打开网关的管理界面，导入根证书与中间颁发机构的证书到SBC当中：

![image-20200614101052516](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101052516.png)

打开之前导出的公网根证书，并导入，如下：

![image-20200614101105443](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101105443.png)

最后导入所购买的公网证书到SBC当中，检查，完成

![image-20200614101120946](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101120946.png)

最后，刷新SBC管理界面即可发现代表安全的“小绿锁”出现了：

![image-20200614101133534](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101133534.png)

**II.   导入公网根证书：Baltimore**
接下来，我们需要添加微软的根证书到SBC  以确保SBC信任来自MS Direct Routing的数据。因为MS Direct  Routing所使用的DNS域名为sip.pstnhub.microsoft.com 便可以推断出其所使用的根证书为Baltimore根证书

下载https://cacert.omniroot.com/bc2025.pem，最后按之前方法导入到SBC当中

附：根证书信息：
Baltimore Cyber Baltimore CyberTrust Root with Serial Number: 02 00 00 b9 and SHA
fingerprint: d4:de:20:d0:5e:66:fc: 53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74

![image-20200614101149183](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101149183.png)

最后完成了证书的导入到SBC的工作之后，就可以开始配置语音网关了。

### 配置SBC

**III.  配置Node-Level Settings**
下图的四个信息一定要写对，包括主机名，DNS（一定要能解释到公网DNS），时区与时间服务器

![image-20200614101215547](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101215547.png)

**IV.   配置Node Interface**
检查我们用于映射到公网的物理网卡

![image-20200614101231805](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101231805.png)

**V.    配置SIP Profile**
接下来这一步很重要，通过配置SIP Profile可以让SBC在SIP消息头中带有SBC FQDN的信息，让MS Phone System可以在公网中找到SBC，并向它发送回包。

Session Times: Enable
FQDN in From Header: Static
FQDN in Contact Header: Static
Static Host: teams.xxxx.com 即SBC的FQDN
Origin Field Username: teams.xxxx.com 即SBC的FQDN

![image-20200614101254624](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101254624.png)

还简单的方法是直接把PSTN Gateway FQDN配置在Domain下面，这样就不用做SIP Profile配置了，特别是一台网关对应多个租户的情况下，如下解释：

![image-20200614101315544](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101315544.png)

**VI.   配置SBC TLS Profile**
因为整个Direct Routing的对接过程需要加密，所以需配置一个Phone System TLS Profile, 稍后要使用：

![image-20200614101335078](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101335078.png)

**VII.  配置Media Crypto Profile**
接下来配置SBC与Phone System之间的媒体流加密机制，统一使用AES_CM_128_HMAC_SHA1_80，如下图

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101335078.png)

**VIII. 配置Media List**
接着，配置一个媒体列表用于告诉SIP Trunk（Direct Routing） 所使用的媒体编码与加密类型的：
这里就需要用到刚刚我们配置的Media Crypto Profile了

![image-20200614101434752](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101434752.png)

**IX.   配置SIP Server Table**
按照如下配置，创建一个SIP Server  Table, 并增加三台Phone  System服务器：sip.pstnhub.microsoft.com；sip2.pstnhub.microsoft.com；sip3.pstnhub.microsoft.com

![image-20200614102826878](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614102826878.png)

![image-20200614102904540](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614102904540.png)

**X.    配置号码转换规则 Transformation Rules**
然后到这里为止，有必要讲一讲Ribbon SWe Lite/SBC1k/SBC2k的一些基本的呼叫流程/工作原理了，分为三大块：信令组Signaling Group; 呼叫路由表  Call Routing Table; 号码转换规则 Transformation Table;

Teams呼叫来到了语音网关之后是如何被处理，如何被路由的呢？这样都反映在下面的图片里面，而且也被反映在接下来的配置里面，具体的流向如下：
1）  在语音网关里面会建立两个信令组，分别为Teams信令组与PSTN信令组。它的作用就是根据来源IP，端口号，协议（如：52.114.7.24:5061 TLS）来选择不同的信令组。我的理解就是把对应的信令组激活。
2）  当激活了Teams信令组后，会选择对应的呼叫路由表（如：From Teams 的路由表）
3）  呼叫进入了From Teams路由表之后，后面还有多个呼叫路由的条目/子路由（如：To PSTN）, 在子路由里面会指定呼叫的下一跳信令组是哪里（从命名可知下一条SG为PSTN信令组）
4）  现在问题来了，凭什么条件可以执行子路由呢？这里有个前提条件就是呼叫必须符合号码转换规则Transformation Table里面规则才能会被路由的，不然就再去匹配下一条子路由，直到匹配为止（即你的主叫是什么，被叫是什么都需要规定好）
5）  在匹配的同时，Transformation还要做一个工作，就是主被叫的号码转换（为什么？因为Teams Phone  System使用E.164标准，但中国的PSTN拨号计划不使用E.164, 这样的话，我们就需要把  E.164前面的“+”号给去掉，同时对号码做点小修改了）
6）  第四步与第五步完成后，信令送回给路由组处理，并送给下一跳信令组。
7）  信令送给目的的信令给（如：PSTN信令组），整个流程完成。

那么，流程与配置是反着来的，所以我们首先要开始配置号码转换规则Transformation Table

![image-20200614101540049](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101540049.png)

理解好了整个流程，就可以开始配置号码转换规则，首先建立两条空的规则：
1)  From PSTN to Teams
2)  From Teams to PSTN
然后才开始具体写里面的规则，以下做了一条简单的正则转换：把被叫的“+”号去掉

![image-20200614101555316](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101555316.png)

因为在上一节中，我们已经把Teams的主叫与被叫都定义为E164标准的，例如当你呼叫一个上海的号码时，会是这样的：
Teams LineURI - 主叫：+86 21 87654321 
被拨打的号码 - 被叫：+86 21 32132132
但出局到PSTN时，运营商网络是不认这种格式的，所以需要做一些主被叫转换，列举如下：

![image-20200614101617491](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101617491.png)![image-20200614101639703](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200614101639703.png)

**XI.   配置语音路由 Call Route Table**
有了号码转换规则，接着开始配置语音路由 1）From Teams 2）From PSTN
![image-20200614101723883](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101723883.png)

在From Teams语音路由里面配置子路由 To PSTN, 同时指定匹配的Transformation Rule与下一跳的信令组SG, 如下图：

![image-20200614101743486](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101743486.png)

同理，在From PSTN语音路由里面配置子路由 To Teams, 同时指定匹配的Transformation Rule与下一跳的信令组SG, 如下图：

![image-20200614101808245](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20200614101808245.png)

**XII.  配置SIP Trunk: SBC >>> Phone System**

因为SIP Trunk（Direct Routing）是双向的，所以以下步骤是关键：
配置SBC信令组SIP Signaling Group, 它会监听来自MS Phone System的的数据流量，并且向Phone System发送数据，如下图配置信令组：

1）  在SG中指定当这条信令组被激活后的语音路由；因为现在是配置Teams信令组，所以选择From Teams (有没有发现命名对于配置是多么的重要)
2）  在SIP Profiles里面选择 “Teams Direct Routing”
3）  在SIP Server Table选择之前创建的Teams Phone System
4）  选择信令与媒体的私网IP，一般为SBC的IP
5）  输入SBC对应的公网IP
6）  进行NAT, 把SBC发布到外网
7）  选择Teams信令组的端口，协议，TLS配置
8）  输入联盟IP地址，为Teams Phone System的几个公网IP

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/DR0571ced43bfe74e3cedc2c4ef356792a.png)

### SIP Option测试

D.    如何使用SIP Option 测试**

至此，本地的SBC与O365 Phone System对接的SIP trunk就做好了，如下图可以看出与O365 Phone  System的信令组状态通了，同时在Counters里面可以看到SIP Option有正常的Incoming &  Outgoing与2xx Incoming & Outgoing, 说明与Phone  System的状态检测正常通过了，那么至少可以证明SBC与Phone  System有数据交互，并且一旦信令能落地到SBC，基本上语音方面没有什么是做不到的。

![Microsoft Teams Voice语音落地系列-5 实战: Sonus语音网关配置](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/dr5c464f3db2cb63330d503f2740bb07f7.png)

Phone System与SBC正是使用SIP Option这种心跳检测机制来判断链路的状态的：

![Microsoft Teams Voice语音落地系列-5 实战: Sonus语音网关配置](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/dr22223cf98220034bddbf1306431ace87e2ff.png)

**6.    实际测试结果**

以下是我在测试环境中用软终端Eyebeam连接SBC, 然后Eyebeam呼叫Teams的效果：

![Microsoft Teams Voice语音落地系列-5 实战: Sonus语音网关配置](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/dr22223cf98220034bddbf1306431ace87e2ff.png)

Teams呼叫Eyebeam的效果：

![Microsoft Teams Voice语音落地系列-5 实战: Sonus语音网关配置](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/dr3333c11187e82d101ad9bf322305ad97ec4d.png)

当然，最重要的还是Teams与手机通讯啦，如下：

![Microsoft Teams Voice语音落地系列-5 实战: Sonus语音网关配置](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/dr44449abf8a359b79a127d823d55fe8a61bd6.png)

最后，本系列终于写完了，主要就是实现到使用Teams语音落地的本地语音网关，然后我们就可以像Skype那样打电话了，这个功能点很重要，主要是当我们从Skype for  Business上面迁移到Teams的时候，我们的语音也必须确保在Teams上面能用，特别是在中国，UC系统无法拨打外线是比较难以想像的。事实在本系列虽然是Step by  Step地讲述每一个配置步骤，但是也只能起到抛砖引玉的作用，也只能把简单的配置路径点明一下，实际的项目当中会有更多其它的东西需要考虑的，感谢各位。