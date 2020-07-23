---
layout:     post
title:      Windows系统镜像打包
subtitle:  
date:       2020-6-28
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Others
- 
---

## 整体流程

![auditmode configuration pass](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/images/dep-win8-l-auditmode.jpg)

![configuration passes overview](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/images/dep-win8-l-configpassesandexes.jpg)

![image-20200714085221513](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200714085221513.png)



![View full-size in a new window](https://content.spiceworksstatic.com/service.community/p/post_attachments/0000146261/524234c5/attached_file/errorkey_preview.gif)

### 制作windows应答文件

https://windowsafg.com/win10x86_x64_uefi.html

https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview#use-an-answer-file-while-installing-windows

![Servicing with Setup: Start with a new device with a USB that contains Windows Setup, your Windows image file, and an unattend.xml customization file. Apply it to new devices.](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/images/servicing_unattend.png)

## audit System - 封闭系统的自定义设置

### PowerShell 命令行

```
powershell -WindowStyle Hidden -ExecutionPolicy Unrestricted %SystemDrive%\Rigel\x64\Scripts\Provisioning\FinalSetup.ps1
```

### 为服务帐号设置空密码自动登陆

```
reg.exe add "HKLM\System\CurrentControlSet\Control\Lsa" /f /v "LimitBlankPasswordUse" /t REG_DWORD /d 0
```

### 自启动文件夹

shell:startup

### 在加载桌面前启动一个程序

[参考链接](https://stackoverflow.com/questions/5091504/application-start-before-windows-explorer)

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon

若加入notepad, 可按如下修改
C:\WINDOWS\system32\notepad.exe,C:\Windows\system32\userinit.exe
```

整个系统的启动过程

\1. HKLM\SYSTEM\CurrentControlSet\Control\Session  Manager\BootExecute. This can include instructions to schedule the  running of chkdsk but not user programs.

\2. Services start next, followed by the RunServicesOnce and RunServices registry keys (if present)

\3. User then logs on to the system

\4. HKLM\SOFTWARE\Microsoft\Windows  NT\CurrentVersion\Winlogon\UserInit. This points to the program  C:\WINDOWS\system32\userinit.exe and the entry ends with a comma. Other  programs can be started from this key by appending them and separating  them with a comma.

\5. HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell. This should contain just one entry, explorer.exe.

\6. Program entries in these 2 registry keys for ALL USERS start next:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run and \RunOnce

\7. Program entries in these 2 registry keys for CURRENT USER start next:

HKCU\Software\Microsoft\Windows\CurrentVersion\Run and \RunOnce

\8. Programs in the Startup Folders of All Users and Current User are started last of all.

https://answers.microsoft.com/en-us/windows/forum/windows_7-desktop/how-to-start-program-before-user-logon-windows-7/2bff97c4-c037-437c-9fa7-b143a3ae5189c

### 腾讯给的系统要求

![image-20200630232442082](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200630232442082.png)

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200630233505303.png" alt="image-20200630233505303" />



```
# No Lock Screen
# Disable Cortana
# wemeet first startup
# # High performance，在应答文件中增加
# disable WindowsUpdate
[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Personalization]
"NoLockScreen"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search]
"AllowCortana"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]
"Userinit"="\"C:\\Program Files (x86)\\Tencent\\WeMeetRooms Dev\\1.5.0.91\\wemeetapp.exe\",C:\\Windows\\system32\\userinit.exe,"

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Power\PowerSettings]
"ActivePowerScheme"="8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c"

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU]
"AUOptions"=dword:00000001

[HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Power]
"HibernateEnabled"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,00,00,5b,e0,00,00,5c,e0,00,00,00,00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]
"DisableCAD"=dword:00000001

# To disable the require sign-in option when Windows 10 wakes up, do the following:
powercfg /SETACVALUEINDEX SCHEME_CURRENT SUB_NONE CONSOLELOCK 0

# Disable Left & Right ALT keys and F4
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,04,00,00,00,00,00,3e,00,00,00,38,00,00,00,38,e0,00,00,00,00

```

### 启用守护进程后的Windows的后门

![image-20200703152218029](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200703152218029.png)





## [使用syspre通用化镜像](https://docs.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation)

```
cd C:\Windows\System32\Sysprep\

C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:"c:\wemeet\autounattend.xml"
```

cd C:\Windows\System32\Sysprep\

C:\Windows\System32\Sysprep\sysprep.exe /oobe /shutdown /unattend:"c:\wemeet\autounattend.xml"

### 问题列表 

- [x] syspre OOBE之后需要无人值守安装；使用syspre参数解决；

- [x] oobe时，提示要输入语言信息：解决：因为镜像使用中文，但应答中用英文

- [x] 无法使用配置好的帐号test自动登陆

- [x] 弹出桌面后，过30秒才能调用出腾讯会议；修改注册表的userinit值解决；改进：直接在shell中运行

- [x] 不要自动锁屏；使用注册表的方式解决

- [x] 两个Tencent帐号，不能自动登陆

- [x] 禁用win key, 与组合键

- [x] 进入到Tencent用户的时候，需要输入密码；使用无密码登陆解决。

- [x] 登陆后左上角可以切换新桌面退出腾讯rooms。

  -》这个问题在zoom rooms上面也有，先记录起来。

  -》在shell中运行，就不会有这个功能。

- [x] 管理员账户也会自动进入腾讯rooms界面导致无法正常设置。-》增加kill.bat解决

- [x] 腾讯rooms切换到管理员后重新登陆要密码才能进入。-》使用无密码登陆解决。

- [x] 显卡要切换管理员账户后重新启动系统才能正常显示显卡驱动。-》等待显示驱动给过来测试。
  -》已测试可用

- [x] 在从管理员(不注销管理员用户的情况)界面切换到Tencent 用户时腾讯rooms启动后会自动关闭。
  -》由于wemeetlaunch.exe不能同时存在于两个帐号当中导致，通过kill.bat与wemeetapp.ps1守护进程解决
  -》守护进程未验证成功，需要手动复制到startup
  
  -》守护进程的kill exploer需要定期执行，万一重开
  
  -》守护进程要与腾讯会议同时打开~！-》在shell中启动守护进程，解决
  
- [ ] 腾讯rooms的bug，在开始会议后上方菜单栏可以往下拉并停在下拉的位置导致无法看到静音、退出视频会议的菜单栏。

- [x] 狂按登陆按键会导致腾讯会议软件Crash

  -》因为在后台不停打开登陆窗口

  -》导入守护进程

- [ ] 封装OS如何与鸿合大屏（IFTP）做绑定？

- [x] 后期的Room软件，需要做一个管理设置功能，当前帐号需要注销-》已排上计划

- [x] Sysprep shutdown 之后的wim再打包出来ISO，安装的OS没有跳过OOBE界面

  -》因为重做了ISO之后，没有了应答文件，所以没有跳过oobe

  -》使用wim的方式

- [x] ISO的交付方式有问题，是否用母盘的方式交付？硬盘的方式

- [ ] [重要] 禁用ALT+F4 -》硬件限制，无法

- [x] 恢复出厂设置-》通过clonezilla解决。

- [ ] 若腾讯会议软件需要升级，这个过程是如何进行的？

- [ ] 需要把Tencent帐号改为user权限，现在是admin权限

- [ ] 避免消息通知，•禁用推送通知

- [ ] Windows更新，输入密码，唤起小娜，休眠。

- [ ] •PC睡眠唤醒无需输入密码

- [ ] 功能测试增加多次重启与开关机后的系统启动时间，不得大于20秒

- [ ] 大屏设备需要加入域测试功能

- [ ] 正式的安装软件必须要有一个固定的安装路径，不能像现在以版本号为安装文件夹名字

- [ ] 这个腾讯必须要在出厂前把正式的安装软件给到我们。

- [ ] 1.完成Windows激活，需要确认一下

- [ ] PPT Slide1中的Rooms设置操作系统，
   - 这个设定是由Tencnet Rooms软件来做？还是由大屏的封装系统来做？
   - 是否按Zoom的设置来参考？若没有UI默认把windows进行设置的话，公开下载的用户安装后是不是会有问题？
   - 替换Windows启动页面，这个也是由Tencent Rooms软件来改吗？具体改哪个位置？还是通过大屏的开机动画来替换？

- [ ] 正式的安装软件必须要有一个固定的安装路径，现在是以版本号为安装文件夹名字的。

- [ ] 是否需要最终用户来完成这个Tencent Rooms的安装流程？还是大屏封装系统可以完成静默安装。

- [ ] 请确认腾讯是否提供标准的Rooms开机指导？

- [ ] 请确认退出Rooms是否把当前帐号（Tencent）注销？

- [ ] 从不关闭显示器

## 配置好的封装系统之后，如何分发到其它设备上面？

### Create a  Bootable windows U key via CreateMedia.ps1

首先把所有相关文件放在wemeet文件夹下面，方便日后调用

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701094007146.png" alt="image-20200701094007146" style="zoom:33%;" />



使用CreateMedia.ps1后会自动生成SRSv2文件夹，之后一定会报错说操作系统版本号不对，所以要改一下以下json中的版本限制

> D:\SRSv2\Skype Room System Deployment Kit\\$oem$\\$1\\Rigel\x64\Scripts\Provisioning

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701093657955.png" alt="image-20200701093657955" style="zoom:33%;" />

CreateMedia.ps1成功制作出启动盘后，进行U盘并把wemeet文件夹复制到 \\$oem$\\$1，因为这个位置是系统安装出来后的C盘根目录

同时把wemeet文件夹里面的autounattend.xml复制到U盘根目录，目的是让系统安装过程调用这些应答文件与安装文件

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701091802277.png" alt="image-20200701091802277" style="zoom:50%;" />

把驱动文件全部复制到启动u盘的 \AutoUnattend_Files\DistShare\Out-of-Box Drivers 目录下面，类似这样

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200707234759752.png" alt="image-20200707234759752" style="zoom:50%;" />

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200707234844352.png" alt="image-20200707234844352" style="zoom: 25%;" />

从U盘启动后，它就会自动把windows安装，配置用户，设置等，最后会自动关机（进入封装状态）

一块可以用于生产的母盘制作完成

![image-20200707234403835](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200707234403835.png)

### [Create WinPE](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive)

刚刚用一只U盘把系统的母盘做出来，现在再找另一只U盘制作winpe启动盘

1. 下载[win PE add on](http://210.78.94.23:83/2Q2WC72340E4B88740C2B159354CC3EB407EC81CCD8B_unknown_90A72F0E4917B661E0EFDBEAEBCFC2E9C2DCFEE7_3/download.microsoft.com/download/D/7/E/D7E22261-D0B3-4ED6-8151-5E002C7F823D/adkwinpeaddons/adkwinpesetup.exe) ，并安装
   

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200707131808273.png" alt="image-20200707131808273" style="zoom: 33%;" />

2. Start the **Deployment and Imaging Tools Environment** as an administrator.

3. copype amd64 D:\WinPE

   ![image-20200707153547947](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200707153547947.png)

4. Create bootable media

   使用下面的命令即可把win pe ISO做出来

   ```
   # 安装在U盘中
   MakeWinPEMedia /UFD D:\WinPE P:
   # 安装在ISO中
   MakeWinPEMedia /ISO D:\WinPE D:\WinPE_amd64.iso
   ```

然后用U盘进入winpe界面，用于把刚刚做出来的母盘里面的操作系统捕获出来

下图是在VM上面挂载了ISO启动，所以选CDROM启动

![image-20200708083435498](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200708083435498.png)

![image-20200708083609344](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200708083609344.png)

### Capture  Image under WinPE

> 需要使用：[Capture and apply a Windows image](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/capture-and-apply-windows-using-a-single-wim)	
>
> 有用的文章：https://www.anoopcnair.com/sysprep-capture-windows-10-image-using-dism/

![Diagram showing a new computer with an empty hard drive, plus a single .wim image file, expands to become multiple configured partitions](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/images/dep-adk-partitions-uefi-overview-capture-windows.jpg)

```
# 捕获自定义好的系统
1）Boot the device using Windows PE.
2）不断地尝试用找到当前硬盘的盘符（如以下D盘），用dir验证，再用以下命令优化
DISM /image:D:\ /optimize-image /boot
3）在局域网中共享一个路径出来，并共享一个文件夹出来
3）把D盘的系统捕获到temp文件夹
Dism /Capture-Image /ImageFile:"\\192.168.116.1\temp\Wemeet-OS.wim" /compress:maximum /CaptureDir:D:\ /Name:wemeet
#应用Image
Apply the image
https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/capture-and-apply-windows-using-a-single-wim#apply-the-image
```

![image-20200708084803505](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200708084803505.png)

![image-20200708084654710](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200708084654710.png)



### [Mount WIM to change Image](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview#modify-an-existing-installation)

[DISM Best Practice](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/deployment-image-servicing-and-management--dism--best-practices) 

![Modify an image offline: Start with an image file (either .wim or .ffu format). Mount the file using DISM. It appears as a group of folders. Modify it using DISM, adding drivers, languages, and more. Use DISM to unmount and commit the changes back to the original image file. Apply it to new devices.](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/images/servicing_mount.png)

```
	
Dism /Mount-Image /ImageFile:"C:\images\CustomImage.wim" /Index:1 /MountDir:C:\mount

Copy CustomAnswerFile.xml C:\mount\Windows\Panther\unattend.xml

Dism /Unmount-Image /MountDir:C:\mount /Commit

Test
```

### [From WIM to ISO: OSCDIMG](https://www.tenforums.com/tutorials/72031-create-windows-10-iso-image-existing-installation.html?__cf_chl_jschl_tk__=9b242aab78b104083aa8cadb717624f1551a4c4f-1594132354-0-AYIYqfXBe7EjSFnKaKGjOzGx7p5tgBANCeW64ftn5I8oPi_1EqlcwZ_MN1xKlBOicDDsKPKxoUCngcp_R8YNYTyXqbajsMH_T2bI_Cdbx0vDLX1yJI-lFm9ZA9jYd-WmsxW0L1HhpW7xWvJIqJyUS2vOElpXxGjLgymgSBUAZjJweYfraH4yKd33webSxMw3hOnEuo9guM1iVI4BmHnhcUyobNTDTfPsiZ896oBR1h_QfgakAKdPqMnAmEstJo1Qa2wl4GZQPivPkSZUk_kIlF0lG_xlY4oTrl7agLIopbAaglNvPPJhvhgnYSqD4BzLw5AvB-7eJG2Fy3m_3Vr-e-ssFfU-Dsm-YTt6faD84z8r#Part5)

***目的：便于在VM中调试母盘的安装***  把wemeet文件夹整合到ISO中，可以直接挂载在VM中安装母盘，而不需要U盘来安装

***输出物：定制化的ISO*** 使用OSCDIMG工具来封装可启动ISO。

***步骤：*** 解压ISO -》复制wemeet, autoattend.xml -》OSCDIMG封装 -》ISO启动即可

1. 先安装windows ADK
2. 解压ISO出来，如 C:\迅雷下载\IoT-from-jason
3. 打开 Deployment and Imaging Tools Environment 命令行
4. 替换文件夹路径，如步骤二，并执行如下：

```
copy c:\迅雷下载\IoT-from-jason\$oem$\$1\wemeet\autounattend.xml c:\迅雷下载\IoT-from-jason\ /y

oscdimg.exe -m -o -u2 -udfver102 -bootdata:2#p0,e,bC:\迅雷下载\IoT-from-jason\boot\etfsboot.com#pEF,e,bC:\迅雷下载\IoT-from-jason\efi\microsoft\boot\efisys.bin C:\迅雷下载\IoT-from-jason\ c:\wemeetOS-IOT.iso
```

5. 原理是使用oscdimg来把启动文件，ISO文件夹，封装到一个ISO当中。

### From IoT ISO to U Disk: Tencent Room Setting

***目的：安装系统*** 使用U盘来安装一个干净的Windows IoT系统，并使用应答文件来自动化安装系统，设置系统，安装腾讯会议软件等一系列定制化要求的设定。

***输出物：母盘*** 使用sysprep来封装已定制化的操作系统，关机，可作为量产期的母盘使用。若再开机，则会跑一次OOBE过程最后自动登陆并进入Tencent会议

***步骤：***U盘 -》复制目录 -》复制驱动 -》复制autounattend.xml

1. 使用Win8USB把ISO安装到U盘，制作成可启动的操作系统U盘

2.  把以下文件复制到U盘根目录

   <img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200713095227241.png" alt="image-20200713095227241" style="zoom:50%;" />

   3.复制驱动程序到以下目录

   <img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200713095344605.png" alt="image-20200713095344605" style="zoom:50%;" />

   4.把wemeet文件夹复制到$oem$\\$1\ 目录下面，同时把autounattend.xml复制到u盘根目录

   <img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200713095515235.png" alt="image-20200713095515235" style="zoom:50%;" />
   
   
   
### Recovery partition using Clonezilla

1) 在ops上面创建一个fat32 20G的分区

2）将clonezilla解压缩到FAT32分区的根目录下。
链接： https://pan.baidu.com/s/1JGikeEt9T6sJt79Sf_A1Hw
提取码：ojt8  

![image-20200720110433349](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200720110433349.png)

3）这两份文件替换 D:\EFI\boot里面的文件

4）以上步骤完成后，可在开机POST LOGO处下按CTRL+F3实现备份/ CTRL+F4还原功能。

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200720110808541.png" alt="image-20200720110808541" style="zoom:33%;" />

5）把创建分区放在应答里面，把clonezilla的复制放在init.bat init1中

- 



常规的做法

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200720100631188.png" alt="image-20200720100631188" style="zoom:70%;" />



## 全闭环式会议室系统体验

预约-

如何预约会议？企微二维码，微信二维码，手机验证码，

> 优化建议：现在是先要打开腾讯会议后，用企业微信登陆，再打开腾讯会议Room才能登陆上企业微信，建议在Room上面增加二维码登陆

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629101045084.png" alt="image-20200629101045084" style="zoom:33%;" />

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629102557245.png" alt="image-20200629102557245" style="zoom:33%;" />

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629102645342.png" alt="image-20200629102645342" style="zoom:33%;" />



会议中的体验

> 没有背景模糊，跟电脑版有差别
>
> 视频质量不好
>
> 在画廊视图下，无法单独固定其中一路视频
>
> 在演讲者视图下，无法选定一路视频固定
>
> 

![image-20200629104133689](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629104133689.png)

会议中可以监视网络质量

![image-20200629104510745](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629104510745.png)

在会议中，可以邀请入会

![image-20200629104848572](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629104848572.png)

生成出来的便是腾讯会议标准的会议转发界面

- 可以通过电话入会
- 可以通过小程序入会
- 直接加入会议（从企微与微信中直接调用，其它）

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200629110047463.png" alt="image-20200629110047463" style="zoom:25%;" />