---
layout:     post
title:      Microsoft Teams Room 恢复出厂设置
subtitle:  
date:       2021-01-04
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

> 在阅读这篇文章之前，您应该已经有一套或多套MTR系统了，由于各种原因您可能需要重置手上的设备到出厂设备（已加入 Active Directory 域，或定义或推送自定义策略设置，或安装了任何第三方软件应用程序的系统），但MTR不像一些设备那样可以一键还原到出厂，所以这篇文章来指导大家来执行一次MTR的系统还原操作。

总体上的步骤如下：

1. 从 Microsoft 下载最新的 Microsoft Teams Room 软件部署工具包。
2. 执行 工具包中的重置PowerShell 脚本以准备进行还原出厂配置。
3. 启动标准的 Windows 10 重置过程进行还原。

# 下载最新的MTR工具包

直接使用以下链接下载MTR工具包，文件名为 SRSDeploymentKit-xxxxxx.msi
https://go.microsoft.com/fwlink/?linkid=851168

![image-20210104141229182](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104141229182.png)

复制到C盘并解压缩MSI文件

```
mkdir c:\temp
cd \temp
msiexec /a "c:\SRSDeploymentKit-4.7.15.0.msi" /qb TARGETDIR="c:\temp\"
```

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104142452766.png" alt="image-20210104142452766" style="zoom:50%;" />

# 执行MTR的重置PowerShell 脚本

进入MTR工具包的目录之后，就可以执行RecoveryTool.ps1脚本进行还原前的准备配置

```
cd '.\Skype Room System Deployment Kit\'
Set-ExecutionPolicy Unrestricted -Force 
.\RecoveryTool.ps1
```

![image-20210104142909640](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104142909640.png)

这里选择【2】Reset进行还原的配置

![image-20210104165609024](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165609024.png)

![image-20210104165639942](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165639942.png)

![image-20210104165709174](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165709174.png)

# 进行标准的Windows重置

- ‎运行以下‎[‎ReAgentC 命令‎](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/reagentc-command-line-options)‎以启用 Windows 恢复环境 （RE）

![image-20210104150029532](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104150029532.png)

- 进行PC重置，运行systemreset并选择Remove everything

  ![image-20210104165820717](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165820717.png)

- 选择**Just remove my files**.

  ![image-20210104165858536](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165858536.png)

- 选择Reset

![image-20210104165917673](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165917673.png)

![image-20210104165930966](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104165930966.png)

- ‎整个重置过程可能需要大约 30 分钟，并将重新启动几次，然后停止在 Windows 设置过程的语言选择菜单。

  <img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/image-20210104150642526.png" alt="image-20210104150642526" style="zoom:50%;" />

  


最后这台MTR就可以成功还原成出厂设置了。

------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom: 33%;" />