layout:     post
title:      玩转Microsoft Teams Room系列(5)
subtitle:   云视频会议室的10大最佳实践（Microsoft Teams Room 篇）
date:       2020-03-11
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---
> 使用最佳实践可以指导大家在规划MTR会议室时避免一些不必要的坑
>
> 本文参考：MS Ignite 2019 Tap into better meetings THR2114

曾经遇到一个客户在对他的会议室做改造的项目，他们的会议室非常简单，一张桌子，几张凳子，一台投影仪，没有任何吸音的装饰，会议室空间比较空旷；因为准备要上市了，老板需要把这些传统会议室改造成为可以提供远程会议能力的现代化会议室，说白了就是用来撑场面用的，例如 *1 文章中的那种MTR。

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/p33389e15565a25eb3fe8ec36fac791978a7.png)

刚刚开始的时候我们提供了一整套的会议室改造方案（云会议平台选型 + 音视频终端 + 会议室改造标准），但这个项目在后面却变成了一个转手音视频终端的项目，跟在电脑城买一个麦克风与喇叭 没有两样，客户按照自己的思路选购了麦克风与扬声器，这些专业设备虽然发挥了它们的作用，但是一些小细节上面还是有不尽人意的地方。

这种转手设备的项目，对于整体方案来说是没有什么参考意义的，但是却让我感受到在现代化的会议室当中，还是需要遵从一些最佳实践或者标准化的Check list来规划一间或多间会议室。

以下十条来自 Frost & Sullivan 2018 的关于云视频会议室的10大最佳实践，可以为大家在新建或改造现有会议室成为现代化/数字化会议的时候，提供一个比较好的标准化会议室建设的参考：

我会拿微软的Microsoft Teams Room 系统来举例说明这些最佳实践，并且在他们的认证设备上面是如何做到这些最佳实践的。

### Installation and Ease of use （容易安装与容易使用）

\- 这套 VC 系统需要易于安装，无需昂贵的专有设备和复杂的配置即可使用，最好当然是‘开包即用’，其实在苹果手机的包装上面就可以体现到这种极致的过程；

\- 然后，用户必须能够轻松地从一台会议控制器上面开始会议。MTR设备支持一键入会，一键开会。

\- 并从任何设备共享内容。在MTR上面，你只需要把设备的HDMI线接入你的电脑，即可把桌面投屏到会议室的大屏上面，同时也一并共享至Teams Meeting当中。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bp1111image6_thumb14.png)

### 摄像机的广角范围 Field of view 

在会议室里面的与会人员越多，摄像机离他们就会越远，所以需要采用合适的广角范围的摄像机来匹配不同大小的会议室。

例如，在小型会议室当中，因为与会者都离摄像头比较近，所以需要选择一个超广角摄像头以确保远端的参会人员可以看到所有的现场与会者。（反之，就有部分看不到）

再例如，在大型会议室当中，因为与会者都离摄像头比较远，所以需要选择一个一般广角镜头的摄像头即可，同时因为是空间大，与会者人数当然也多，所以这个摄像头应该要有多倍变焦，语音跟踪，预置位 这些功能。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage11_thumb4.png)

以下是一种比较特别的场景，类似于作战会议室，它配置了两台大尺寸的交互显示大屏，所以这种组合是放在大会议里面用的吗？no no no, 当它搭配了一个超广角镜头之后，这种组合就立马可以应用在作战会议室当中了。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage20_thumb4.png)

### 自动摄像头控制 Camera tracking 


在大型会议室中，仅仅安装覆盖会议室所有参与者的一般广角的摄像头是不够的（正如第二点所说），因为远程参与者察觉不到发言人的一些细微动作，所以启用自动对焦技术，使摄像机可以自动对发言进行对焦，让画面视野始终清晰呈现各位与会人员。

说白了这里面需要一台PTZ摄像头，什么是PTZ? Pan/Tilt/Zoom 的简写，代表云台全方位（左右/上下）移动及镜头变倍、变焦控制。通常提到这个词，可以通俗的理解为云台控制。

### 摄像头的位置 Camera placement 


摄像头的放置位置最理想的状态是尽量与眼部位置水平，但这是理想状态；这时可以使用 PTZ 摄像机 ，有助于微调视角。

当使用双显示器时，将相机置于两个屏幕之间的眼部水平，以实现最佳的视觉位置，如下：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage25_thumb4.png)

再来一张关于摄像头放置位置的图片：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage30_thumb4.png)

### 对声音的处理 Audio


对于声音的处理，是所以做会议室设备厂家考虑的重要焦点，优质高保真的的声音传输，会给整场会议带来非常好的会议体验，就不多说了。主要有这样一些技术：AI自动跟踪取景，背景噪声抑制等；

降噪地毯和墙面消音板进一步消除环境噪音对会议的影响；

### Display Size （交互大屏的大小）


\- 显示屏的尺寸需要够大，至少55寸以上，可确保会议中的每个人都能轻松在屏幕上看到共享内容。以下是一个尺寸与会议室大小的对比比例，可以按需参考。

\- 显示器必须与桌子和整体房间大小成比例。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage34_thumb4.png)

\- 双显示器允许一个屏幕显示远程参与者，而另一个屏幕可以专用于内容共享（这个需要会议室里选用的云会议系统有关，Microsoft Teams Room 就可以做到这种两屏显示），所以离屏幕最远的人也可以轻松地看到内容。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage44_thumb4.png)

### Content Sharing （具有屏幕共享功能）


这一点类似于第一点，可以使用MTR系统提供的HDMI IN功能，快速把主持人的桌面投屏到Teams Meeting 与 本地的显示大屏上面。

### Connectivity （各种设备的线缆连接要美观，简洁）


可以想像得到，在一个MTR视频会议室的桌面上会有这样一些设备（麦克风，扬声器，HDMI线，VGA线，各种IT外设）然后还有线缆是连接到显示屏，会议电脑主机上面，然后就是各种混乱，不整齐，难于维护。有一个比较好的解决方法是使用hub的方式，分别是display hub 与 table hub , 它们会把显示器，扬声器，摄像头，会议主机的所有线缆都汇聚成一根网线，让显示大屏与会议桌之前的线缆走线显示更加美观，如下图。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/bpimage49_thumb4.png)

### Room Design（会议室大小）


其实我觉得这段英文描述针对这个最佳实践没有什么意义，但也列出来供参考，因为一般会议室也不会做成三角形的，一般都是长方形或正方形的会议，所以不合逻辑；

对于会议室大小这个概念反而要说说的，可以分为大，中，小，微型的会议室，在这些不同类型的会议当中部署MTR系统都是可以的，因为MTR适合于不同类型的会议室使用，统一使用了MTR之后，可以让大家有统一的入会体验，而不是每到一间会议室都要熟悉一次设备的使用。

![See the source image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/RE3l6Yy)

### Lighting （减少光线对视频质量的影响）


大多数现代会议空间都带有窗户和自然光。 即使在大多数照明条件下，采用最新技术的相机也可以通过优化面部的光平衡并渲染自然的肤色来提供清晰度。 面对走廊的玻璃窗会议室应磨砂，以减少干扰

### 参考链接

*1 微软 Teams Meeting 系列(4) 把MS Teams Room 成为企业的生产力工具 https://blog.51cto.com/nemotan/2469498

*2 Microsoft Teams Meeting 系列 汇总帖 https://blog.51cto.com/nemotan

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />