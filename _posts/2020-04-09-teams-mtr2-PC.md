---
layout:     post
title:      玩转Microsoft Teams Room系列(2)
subtitle:  如何把MTR安装在NUC主机中
date:       2020-04-09
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

> 本文介绍如何使用CreateSrsMedia.ps1来创建MTR安装镜像并安装

在上 一节当中，我们使用了一个方便的PowerShell 脚本SkypeRoomProvisioningScript.ps1 创建了两个MS Teams Room 会议室帐号（其实就Exchange资源帐号），接下来这一节我会介绍一下把MTRAPP 安装到Intel NUC小主机当中，先介绍一下这个架构：

核心是6-Intel NUC PC，它通过HDMI或Type-C口连接会议室的显示大屏（视频输出），用一个USB口连接桌面上的罗技tp100 (2) 用于MTR的会议控制平板也是MTR的核心组件，用一个USB口连接罗技的CC5000e，其中包括了speaker, microphone, cam, 集线器；在NUC PC当中，会安装一个微软打包好的windows10操作系统，里面已经配置并安装好了MTR App了。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb24.png)

通过上图的架构做好，理好所有的线材之后，在会议室的桌面上就只有一个Teams控制平板与一个MIC，非常的简洁与易用。

整个MTR应用程序其实就是一个微软打包好的集成了MS Teams Room 应用程序的操作系统镜像，所以这一节的重点就是如何把这个启动U盘制作出来，并安装在Intel NUC主机当中。

我只是想简单一点把这个事情做完，因为做完之后你会发现其实是非常简单的一件事情，我把坑都帮大家踩完了。对于制作的安装镜像来说，MTR对于OS的版本有严格的要求：参考 *1

这次，我会使用最新的版本：MTR 4.2.4.0, 那么它对应支持的OS Build为18362.356，如下图：

![image_thumb39](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb39_thumb3.png)

如果你使用的windows 镜像版本没有完全匹配以上OS Build的话（OS Build = 18362.356 or 17134.191），在U盘的制作过程中就会出错，如下：

![image_thumb40](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb40_thumb5.png)

那么如何找到这个具体的windows版本下载呢？其实就是windows10 1903版本在2019年9月的时候的一个更新（如何下载windows iso？ 请参考 *2 )

![image_thumb41](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb41_thumb3.png)

下载完这个重要的windows镜像之后，我们就可以开始制作了：

1）准备一个32G的U盘（USB 存储上的任何现有文件都将丢失）

2）准备windows镜像文件，文件名为cn_windows_10_business_editions_version_1903_updated_sept_2019_x64_dvd_2f5281e1

3）下载这个安装脚本 CreateSrsMedia.ps1 ，请参考 *3，它会自动帮你完成以下任务：

- Download the latest MSI installer for Microsoft Teams Rooms. 
- Determine the build of Windows that the user must supply. The most recently  released versions may or may not be tested and supported for use with Microsoft  Teams Rooms devices. 
- Download necessary supporting components. 
- Assemble the needed components on the installation media.

4）运行这个脚本，注意这个运行路径不能有空格

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb9.png)

留意一下，它会把U盘制作成一个UEFI启动盘，所以这里有一个坑就是你的Intel NUC里面的BIOS需要调成UEFI的启动模式，不然就启动不了，一般新的BIOS都是这种模式的了。

就制作完了？这么简单？没错，简单吧….

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb191.png)

之后，我们开始把Intel NUC小主机

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb12.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb13.png)

接入刚刚制作好的镜像U盘，并开机（我用的是一个128G的SSD移动硬盘）

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb14.png)

开机按F2进入BIOS，调成UEFI启用并把U盘启动改成第一启动。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb15.png)

保存退出后，会直接从U盘启动并开始自动安装

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb16.png)

经过约半小时的安装之后，MTR就自动安装出来了

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb17.png)

配置会议室帐号（请参考《玩转Microsoft Teams Room系列1 - 部署前置条件与帐号创建》*4）

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb18.png)

以下这一步将会是没有仔细看 *4 的同学的大坑，如果你没有使用认证的MS Teams Room设备，这一步过不去哈

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb19.png)

例如本文一样，我使用的是罗技的TP100会议控制屏, 这一块MTR认证设置，通过一根 USB-TypeC线连接

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb20.png)

针对罗技的TP100来说，又来一个坑，安装好之后你需要进行admin 帐号把以下DisplayLink驱动安装好，不然TP100会无显。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb21.png)

调整一下显示布局，同时把TP100设置成主要显示器，例如下图中的第二个显示器需要调成主显。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb22.png)

发现很多驱动没有被安装出来？Intel Driver Support Assistant 了解一下，下载安装快速解决这个驱动问题。

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb7.png)

大功告成~！

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb10.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/mtrpcimage_thumb11.png)

### 参考

*1 [Microsoft Teams Rooms app version support](https://docs.microsoft.com/zh-cn/MicrosoftTeams/rooms/rooms-lifecycle-support) 
 *2 [September 10, 2019—KB4515384 (OS Build 18362.356)](https://support.microsoft.com/en-us/help/4515384/windows-10-update-kb4515384) 
 *2 cn_windows_10_business_editions_version_1903_updated_sept_2019_x64_dvd_2f5281e1.iso 
 ed2k://|file|cn_windows_10_business_editions_version_1903_updated_sept_2019_x64_dvd_2f5281e1.iso|5231140864|B1D5C4C401036B0B1EBA64476A95F338|/
 *3 [CreateSrsMedia 脚本](https://go.microsoft.com/fwlink/?linkid=867842) 

*4 玩转Microsoft Teams Room系列1 - 部署前置条件与帐号创建 https://blog.51cto.com/nemotan/2480467

*5 罗技Tap100 https://www.logitech.com.cn/zh-cn/product/microsoft-rooms

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />



