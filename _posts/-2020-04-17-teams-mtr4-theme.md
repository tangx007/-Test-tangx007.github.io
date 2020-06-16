---
layout:     post
title:      玩转Microsoft Teams Room系列4 - 自定义主题让你的MTR会议与众不同
subtitle:  xxx
date:       2020-04-17
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Room
- 
---

> 你是如何评价这篇文章的？用一句很简单的话

在会议室里面每场会议可能会接待不同类型的用户，有普通员工，老板，客户，供应商等等，那么会议室里面的这几块显示大屏是不是可以发挥一下它的对外宣传企业文化的作用呢？

在MTR的后台设置里面，我们可以设置很多不一样的主题，同时MTR还可以自定义主题，让你的MTR会议与众不同

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image_thumb14.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image30.png)



我们可以通过XML配置文件里面的CustomThemeImageUrl参数来自定义MTR的主题，会这样几步：

1）准备好一张3840X1080 pixels，且以jpg, jpeg, png, and bmp名字的图片。如果你有专业的设计人员，可使用photoshop设计，参考 *2

2）制作一份XML文件，后述。

3）把图片与文件一同放在C:\Users\Skype\AppData\Local\Packages\Microsoft.SkypeRoomSystem_8wekyb3d8bbwe\LocalState 这个目录下面。

4）重启MTR即可。重启之后这两份文件会被删除，所以要保管好。



好，我们来一步一步看看怎么做的，先创建一个3840X1080 pixels的空白图片

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image_thumb17.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image33.png)

编辑图片

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image_thumb20.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image36.png)



接着用记事本生成一份XML文件，并把图片一并放在`C:\Users\Skype\AppData\Local\Packages\Microsoft.SkypeRoomSystem_8wekyb3d8bbwe\LocalState`

> <SkypeSettings>     <AutoScreenShare>true</AutoScreenShare>     <Theming>         <ThemeName>Custom-2</ThemeName>         <CustomThemeImageUrl>bg2.png</CustomThemeImageUrl>         <CustomThemeColor>             <RedComponent>100</RedComponent>             <GreenComponent>100</GreenComponent>             <BlueComponent>100</BlueComponent>         </CustomThemeColor>     </Theming>< /SkypeSettings>

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image_thumb23.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image39.png)



配置好并重启MTR之后，用Teamviewer远程进出看看效果：

[![8f52f913d96af7b04972d370188977a](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/8f52f913d96af7b04972d370188977a_thum.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/8f52f913d96af7b04972d370188977a9.png)

点进去主题，就可以看到多出一个自义主题了

[![0b18efccb6e4940ddb981afe2160f79](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/0b18efccb6e4940ddb981afe2160f79_thum.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/0b18efccb6e4940ddb981afe2160f798.png)

来看看实际的图片吧，家里的显示屏不够，只有外接一个单屏，但可以看到主题已经成功变了

[![image](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image_thumb25.png)](file:///C:/Users/Nemo/AppData/Local/Temp/OpenLiveWriter224483931/supfilesB4ED04/image41.png)



参考：

*1 MTR Release Notes https://docs.microsoft.com/en-us/MicrosoftTeams/rooms/rooms-release-note

*2 https://docs.microsoft.com/en-us/MicrosoftTeams/downloads/ThemingTemplateMicrosoftTeamsRooms_v2.1.psd





