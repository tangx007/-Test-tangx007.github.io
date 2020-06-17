---
layout:     post
title:      玩转Microsoft Teams Room系列(3)
subtitle:  手动升级MTR版本4.4.25.0
date:       2020-4-9
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

> 使用本文的方法，可以让你快速把MTR升级到最新版本

之前的章节当中， 我们已经可以成功把MTR安装在Intel NUC当中，但在最近MTR版本升级到了4.4.25.0，在官网上面也只能下载到这个版本的软件，请参考*1下载。所以如果你手上的MTR系统是旧版本要升级，或需要解决问题使用新版本的话，我们需要做一次手动升级MTR版本的操作，但这并不是一次简单的软件升级，过程还是比较让人感动。

用得好好的MTR为什么要升级？如果你手上的MTR是很久之前的4.0.27.xxxx MTR版本的话，我们在项目上面测试上只能拿到Exchange日程但是登陆不上Teams（微软做了版本限制？）但现在客户需要Teams能登陆上MTR的话，也就只有两个选择：

- 重做MTR安装镜像来重新安装，推倒重来，请参考《如何把MTR安装在NUC主机中》 *3
- 本文的方法，手动升级MTR版本到最新

Option1不太现实，我们选择option2来更新现有的MTR，在官网上面只有一笔带过的更新说明 *2, 就这样一段脚本：

```
Add-AppxPackage -Update -ForceApplicationShutdown -Path '\\<share>\$oem$\$1\Rigel\x64\Ship\AppPackages\*\*.appx' -DependencyPath (Get-ChildItem '\\<share>\$oem$\$1\Rigel\x64\Ship\AppPackages\*\Dependencies\x64\*.appx' | Foreach-Object {$_.FullName})
```

登陆到admin帐号下面，把最新的MSI下载回来 *1，安装好，修改好<share>里面的路径之后，运行命令，直接报错：

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ugimage_thumb1.png)

后面仔细研究了一下整个MTR的安装脚本，MTR在OS里面分了两个默认的帐号（一个是skype帐号，用于启动MTR；一个是admin帐号，用于做操作系统的日常管理），发现原来MTR App并不是直接在admin帐号下面更新，而是需要在skype帐号下面进行，问题又来了skype帐号在MTR里面做了好多限制（包括禁用桌面，禁用运行脚本，登陆自动启动MTR APP等）你是没有办法登陆到skype帐号下面进行MTR的更新操作，所以我们需要在admin 帐号下面来把MTR app安装到skype帐号下面….这文章为大家跳过很多升级过程中的坑，因为在微软的官方文章中对这个升级的说明少之又少 *2。如果你按照微软文章中方法更新MTR的话必定出错，列一些坑出来：

- 微软的官方文章中对这个升级的说明少之又少 *2 （也就算了，我可以开O365 Case）
- 开了O365 Case后, 支持人员说这是硬件厂家自改的操作系统问题，不是Teams Room的问题（好吧，我找MTR硬件厂家）
- 开了MTR硬件厂家Case后，支持人员又说不能直接升级，需要用我们的原厂镜像，重装原厂镜像后，MTR版本没有升级上去还是在4.0.27.xxx。
- 微软文档里面提供的命令不太准确，所以自力更生研究出以下方法。

以下是我们测试过已成功升级的方法，分享给大家使用。首先我们需要登陆到admin帐号，并把这个MTR应用安装在Skype这个帐号下面：

1. 下载前安装最新的MTR到默认安装目录，参考*1下载。
2. 登陆到admin，创建一个PowerShell脚本如下，并保存为C:\InstallSkype.ps1

```
$temp = 'C:\Program Files (x86)\Skype Room System Deployment Kit\$oem$\$1'
Add-AppxPackage -ForceApplicationShutdown -Path $temp\Rigel\x64\Ship\AppPackages\*\*.appx -DependencyPath (Get-ChildItem $temp\Rigel\x64\Ship\AppPackages\*\Dependencies\x64\*.appx | Foreach-Object {$_.FullName})
```

3. 用管理员模式打开PowerShell ISE, 并执行以下脚本，用于把MTR安装到skype帐号下面

```
$creds = New-Object System.Management.Automation.PSCredential ("Skype", (new-object System.Security.SecureString))
reg.exe add "HKLM\System\CurrentControlSet\Control\Lsa" /f /v "LimitBlankPasswordUse" /t REG_DWORD /d 0
start-process -FilePath powershell.exe -wait -Credential $creds -ArgumentList @("-executionpolicy","Unrestricted","C:\InstallSkype.ps1")
reg.exe add "HKLM\System\CurrentControlSet\Control\Lsa" /f /v "LimitBlankPasswordUse" /t REG_DWORD /d 1
```

4）安装好之后，重启电脑，新增了开机动画，新版本的MTR来了（4.4.25.0）

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ugimage_thumb6.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ugimage_thumb3.png)

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ugimage_thumb8.png)

以下为一些安装记录，供参考，先查看一下安装前的MTR版本为43420

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ugimage_thumb31.png)

如果这时你直接使用微软网站上面的操作来执行更新MTR的命令时，就会报以下错误，原因是由于其实MTR APP是安装在skype user下面，而不是admin user下面，所以报错是正常的。

![2da19931001627501e8f1db77afd16f](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ug2da19931001627501e8f1db77afd16f_thum.png)

接着如果你使用上文中我的方案，就可以把app安装在skype user下面了，从而顺利地升级MTR.

运行完Step1~3之后，我们来检查一下安装好的版本，44250出来了，同时在PackageUserInformation可以看到是安装在skype user下面了。

```
Get-AppxPackage -AllUsers | ?{$_.PackageFamilyName -like '*room*'}
```

![image](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/ugimage_thumb11.png)

### 参考

*1 MTR部署工具包 https://go.microsoft.com/fwlink/?linkid=851168

*2 To update MTR using PowerShell https://docs.microsoft.com/zh-cn/MicrosoftTeams/rooms/rooms-operations#to-update-using-powershell

*2 The PowerShell command to update MTR software is not work #4475 https://github.com/MicrosoftDocs/OfficeDocs-SkypeForBusiness/issues/4475

*3 玩转Microsoft Teams Room系列2 - 如何把MTR安装在NUC主机中 https://blog.51cto.com/nemotan/2485928





