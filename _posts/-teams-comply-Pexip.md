---
layout:     post
title:      xxx
subtitle:  xxx
date:       2020-5-20
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Teams-Meeting
- 
---



<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701121455365.png" alt="image-20200701121455365" style="zoom:50%;" />

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701123709680.png" alt="image-20200701123709680" style="zoom: 67%;" />

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701121706513.png" alt="image-20200701121706513" style="zoom:50%;" />

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701121849356.png" alt="image-20200701121849356" style="zoom:50%;" />

#### To complete setup, run the following entries in Windows PowerShell

##### Prerequisite for running the scripts

The [Skype for Business Online, Windows PowerShell Module](https://www.microsoft.com/en-us/download/details.aspx?id=39366) must be installed. 
    You can check if this is installed by running the following

```powershell
If (Get-Module -ListAvailable -Name SkypeOnlineConnector) {
    'Skype for Business Online, Windows PowerShell Module is installed'
} Else {
    'Please install the Skype for Business Online, Windows PowerShell Module'
}
```

![image-20200701123023997](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701123023997.png)

##### Step 1: Connect to SfB online powershell to enable interop for Teams users

Warning: Be sure to change the domain below to your Office 365 domain!

```powershell
Import-Module SkypeOnlineConnector
$sfbSession = New-CsOnlineSession -OverrideAdminDomain "yourcompanyname.onmicrosoft.com"
Import-PSSession $sfbSession
```

![image-20200701123059841](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701123059841.png)

##### Step 2: Create new video interop service provider with your Trusted App ID set as trusted.

```powershell
New-CsVideoInteropServiceProvider -Name Pexip -TenantKey "teams@ucssi.onpexip.com" -InstructionUri "https://pexip.me/teams/ucssi.onpexip.com/{ConfId}" -AllowAppGuestJoinsAsAuthenticated $true -AadApplicationIds "923c9f0a-b883-4efe-af73-54810da59f83"
```

##### Step 3: Grant interop for all users or specific individuals.

Grant interop for all users in the tenant (recommended):

```powershell
Grant-CsTeamsVideoInteropServicePolicy -PolicyName PexipServiceProviderEnabled -Global
```

or just for individuals (for pilot testing):

```powershell
Grant-CsTeamsVideoInteropServicePolicy -PolicyName PexipServiceProviderEnabled -Identity "tangx@contoso.com"
```

![image-20200701123226566](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701123226566.png)



<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200701123322721.png" alt="image-20200701123322721" style="zoom:50%;" />