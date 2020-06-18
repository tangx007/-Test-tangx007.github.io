---
layout:     post
title:      使用 Powershell 创建注册表键值
subtitle:  
date:       2020-6-1
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- Powershell
- 
---

非常简单，修改标黄部分即可，直接上图：

```
New-Item -Path HKEY_CURRENT_USER\Software\Microsoft\Office\14.0\Common -Name BlockHTTPImages -Value 1 –Force
```

![](https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/4ffcc49706fdceca1a43f81583637d07.png)

参考：

Use PowerShell to Easily Create New Registry Keys https://devblogs.microsoft.com/scripting/use-powershell-to-easily-create-new-registry-keys/



------

欢迎添加我的微信，分享您的见解与我的解决方案哦，让我们共同探索。

<img src="https://cdn.jsdelivr.net/gh/tangx007/tangx007.github.io/img/nemo-qrcode.jpg" style="zoom:50%;" />



