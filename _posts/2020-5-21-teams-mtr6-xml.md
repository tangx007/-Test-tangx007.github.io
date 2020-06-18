---
layout:     post
title:      玩转Microsoft Teams Room系列(6) 
subtitle:  通过XML批量配置 Microsoft Teams Room
date:       2020-5-21
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

> ***运筹帷幄之中，决胜千里之外*** - 西汉·司马迁《史记·高祖本纪》

### 使用场景

之前一篇文章介绍了通过XML文件来自定义MTR的背景图片，其实我们通过XML文件其实还可以做更多在MTR会议室的日常运维任务。

想像一下这样一个场景：当你需要为分布在不同地方的多间MTR会议室进行配置或管理的时候，例如:

- 变更MTR会议室的帐号？
- 在改密码之后需要更新MTR设备的密码？
- 为了安全原因需要隐藏会议主题？
- 会议室新购多一块会议交互大屏，变成了双屏会议室？我们需要打开MTR的双屏显示开关
- 自定义MTR的主题图片？参考这篇：[自定义主题让你的MTR会议与众不同](https://blog.51cto.com/nemotan/2488229)

### 具体配置步骤

这些操作我们都可以通过XML配置文件的方式远程推送给MTR设备来实现（用共享目录的方式来推，用组策略来推，用SCCM来推，都行，只要你能够把SkypeSettings.XML这份XML文件放在MTR的指定目录即可）

> 什么是MTR? Microsoft Teams Room

1. 首先需要先创建一份名字为SkypeSettings.XML的文件。

2. 接着就可以去修改SkypeSettings.XML里面的内容,例如以下代码配置了三个设置（自动屏幕共享，隐藏会议主题，MTR帐号）。

   > PS. 当你要运维多个MTR会议室时，你不需要把所有参数都放在XML里面，只需要把要改的放进来即可，这样就可以有针对性地去维护这些会议室。

   

   ```xml
    <SkypeSettings>
        <AutoScreenShare>true</AutoScreenShare>
        <HideMeetingName>true</HideMeetingName>
        <UserAccount>
            <SkypeSignInAddress>RanierConf@contoso.com</SkypeSignInAddress>
            <ExchangeAddress>RanierConf@contoso.com</ExchangeAddress>
            <ModernAuthEnabled>false</ModernAuthEnabled>
            <DomainUsername>Seattle\RanierConf</DomainUsername>
            <Password>password</Password>
        </UserAccount>
    </SkypeSettings>
   ```


3. 然后用你熟悉的方法把XML文件推送到MTR的指定目录，如下

    ```powershell
    C:\Users\Skype\AppData\Local\Packages\Microsoft.SkypeRoomSystem_8wekyb3d8bbwe\LocalState
    ```

    以下我们也可使用PowerShell来推送这份XML文件，如下

    ```
    $movefile = "<path>";
    $targetDevice = "\\<Device fqdn>\Users\Skype\AppData\Local\Packages\Microsoft.SkypeRoomSystem_8wekyb3d8bbwe\LocalState\SkypeSettings.xml"; 
    Copy-Item $movefile $targetDevice
    ```

4. 最后把MTR重启即可生效，同时系统会把这份XML给自动删除掉的(不用命令就手动重启吧~！)

   ```powershell
   invoke-command { Shutdown /r /t 0 } -ComputerName <Device fqdn>
   ```
5. 如下图参考

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200521161050.png)

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200521161144.png)

### 通过PowerShell脚本自动生成XML

放大招时间到了，其实我们可以使用GitHub上面一位国外牛人的PowerShell来自动创建这份SkypeSettings.xml

> 下载这个命令脚本[New-MtrConfigrationFile.ps1](https://gist.github.com/jhochwald/7407bc208da4c68cb466ad911a1b68b7)，按照以下来生成这份XML

```
PS D:\> .\New-MtrConfigrationFile.ps1
PS D:\> New-MtrConfigrationFile -HideMeetingName $true -Path D:\                               

PS D:\> dir Skype*                                                                             
Directory: D:\
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        2020/3/19     11:09          35611 SkypeRoomProvisioningScript.ps1
-a----        2020/5/21     17:02             98 SkypeSettings.xml

# 我列举一些常用的例子：
# 自定义主题
New-MtrConfigrationFile -ThemeName Custom -CustomThemeImageUrl 'wallpaper.jpg' -RedComponent 100 -BlueComponent 100 -GreenComponent 100 -Path D:\
# 配置MTR帐号
New-MtrConfigrationFile -UserAccount -SkypeSignInAddress 'RanierConf@contoso.com' -ExchangeAddress 'RanierConf@contoso.com' -DomainUsername 'Seattle\RanierConf' -Password 'password' -ConfigureDomain 'domain1, domain2' -Path D:\
# 设置双屏显示
New-MtrConfigrationFile -DualScreenMode $True -Path D:\
# 设置隐藏会议主题
New-MtrConfigrationFile -HideMeetingName $True -Path D:\
```

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/20200521170444.png)

### XML参数列表

- AutoScreenShare
- HideMeetingName
   If true, meeting names are hidden.
- UserAccount
   Container for credentials parameters. The sign in address, Exchange address, or email address are usually the same, such as RanierConf@contoso.com.
- IsTeamsDefaultClient
   Is Microsoft Teams the Default for new Meetings
- BluetoothAdvertisementEnabled
   Support local Bluetooth beakoning
- SkypeMeetingsEnabled
   Support Skype for Business Meetings
- TeamsMeetingsEnabled
   Support Microsoft Terams Meetings
- DualScreenMode
   If true, dual screen mode is enabled. Otherwise the device uses single screen mode.
- SendLogs
   Configure the "Give Feedback" and "Report Issue"
- Devices
   The connected audio device names in the child elements are the same values listed in the Device Manager app.
   The configuration can contain a device that does not presently exist on the system, such as an A/V device not currently connected to the console.
   The configuration would be retained for the respective device.
- ThemeName
   Used to identify the theme on the client. The Theme Name options are Default, one of the provided preset themes, or Custom.
   Custom theme names always use the name Custom.
   The client UI can be set at the console to the Default or one of the presets, but use of a custom theme must be set remotely by an Administrator.
   Preset themes include:
   Default
   Blue Wave
   Digital Forest
   Dreamcatcher
   Limeade
   Pixel Perfect
   Roadmap
   Sunset
   To disable the current theme, use "No Theme" for the ThemeName.
- SkypeSignInAddress
   The sign in name for the Skype for Business or Teams device account.
- ExchangeAddress
   The sign in name for the Exchange device account.
- ExchangeAddress
   The domain and user name of the device, for example Seattle\RanierConf.
- Password
   The password parameter is the same password used for the Skype for Business device account sign-in.
- ConfigureDomain
   You can list several domains, separated by commas.
   Use one long string here!!!
- EmailAddressForLogsAndFeedback
   Sets an optional email address that logs can be sent to when the "Give Feedback" window appears.
- SendLogsAndFeedback
   If true, logs are sent to the admin. If false, only feedback is sent to the admin (and not logs).
- MicrophoneForCommunication
   Sets the microphone used as the recording device in a conference.
- SpeakerForCommunication
   Device to be used as speaker for the conference. This setting is used to set the speaker device used in a call.
- DefaultSpeaker
   Device to be used to play the audio from an HDMI ingest source.
- ContentCameraId
   Define the instance path for the camera configured in room to share analog whiteboard content in a meeting.
   See https://docs.microsoft.com/en-us/MicrosoftTeams/room-systems/xml-config-file#locate-the-content-camera-usb-instance-path
   Please note: We replace "&" with "&amp;" for you!
   Just use the String you copied from the Device Manager/Imaging devices/Properties/Details/Device instance path
- ContentCameraInverted
   Specify if the content camera is physically installed upside down. For content cameras that support automatic rotation, specify false.
- ContentCameraEnhancement
   When set to true (the default), the content camera image is digitally enhanced: the whiteboard edge is detected and an appropriate zoom is selected, ink lines are enhanced, and the person writing on the whiteboard is made transparent.
   Set to false if you intend to send a raw video feed to meeting participants for spaces where a whiteboard is not drawn on with a pen and instead the camera is used to show sticky notes, posters, or other media.
.PARAMETER CustomThemeImageUrl
   Required for a custom theme, use file name only.
- RedComponent
   Represents the red color component.
- GreenComponent
   Represents the green color component.
- BlueComponent
   Represents the blue color component.

---

参考：

[Manage a Microsoft Teams Rooms console settings remotely with an XML configuration file](https://docs.microsoft.com/zh-cn/MicrosoftTeams/rooms/xml-config-file) 

New-MtrConfigrationFile.ps1](https://gist.github.com/jhochwald/7407bc208da4c68cb466ad911a1b68b7)



------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />