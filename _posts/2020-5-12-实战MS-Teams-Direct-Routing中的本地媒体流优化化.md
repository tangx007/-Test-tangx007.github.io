---
layout:     post
title:      实战MS Teams Direct Routing
subtitle:   Local Media Optimization本地媒体流优化
date:       2020-5-12
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Teams-Dirct-Routing
    - 本地媒体流优化
---
# 【未完成】实战MS Teams Direct Routing中的本地媒体流优化

在之一篇文章当中，简单介绍了MS Teams DR Local Media Optimization （LMO） 本地媒体流优化的两种主要场景与两个LMO模式，我们来回顾一下：

1. 场景一，**【单台Central SBC的场景】**
   企业的内网Teams用户想使用媒体旁路技术，但是他们又不可以直接访问SBC的公网IP。取而代之就是使用SBC的内网IP，如下图中的192.168.5.5，我们称这个IP为SBC的Optimal IP，LMO会基于Teams Client所在的位置来确定这个Optimal IP,  并把它写在SIP信令当中（SDP），从而让客户端直接访问SBC内网IP。

2. 场景二，**【Proxy SBC的场景】**
   在企业中，并不是所有地区的SBC都有能力与MS Phone  System对接，在其它地区的Teams用户若想进行本地的语音呼叫时，就会使用当地的SBC（Downstream SBC）与Proxy  SBC（就是场景一中的Central SBC）对接，使用了本地媒体流优化后，*路径会缩短成 Teams Client >> Vietnam SBC >> PSTN*

3. 模式一，**Always bypass mode** : 无论Teams Client 位于哪个地区/子网（后台已定义好的子网段），都会跳过Phone System 与 Proxy SBC而直接由当地的SBC出局到PSTN；

   *路径是这样的 Teams Client >> Vietnam SBC >> PSTN*

4. 模式二，**Only for local user mode**：若Vietnam配置了这种模式，则只有VN子网的用户媒体流会直接流经VN SBC，其他情况（在企业外部，在Indonesia子网，在SPG子网）的用户媒体流都会通过Proxy SBC的

   *VN子网的媒体流路径： VN Teams Client  >> Vietnam SBC >> PSTN*

```sequence
# 例如：
VN内网用户 --> VN子网: media flow
```

   *IDN子网的媒体流路径： IDN Teams Client  >> SPG SBC >> Vietnam SBC >> PSTN*

   *SPG子网的媒体流路径： SPG Teams Client  >> SPG SBC >> Vietnam SBC >> PSTN*


```sequence
# 例如：
外网用户 --> SPG子网: media flow
SPG子网 --> VN子网:
```
```sequence
# 例如：
IDN用户 --> SPG子网: media flow
SPG子网 --> VN子网:
```

## Teams上面的站点逻辑

Teams的每一通呼叫都会记录机器的子网IP并通过REST API发送到Teams来进行LMO优化的，有了子网IP就可以确定出当前的子网站点名字（如SPG, VN, IDN站点），最后面Teams会在发送到Direct Routing的SIP SDP信令中包含以下 X-MS Headers，有了这些信令包头之后本地的SBC就可以针对这些信息做路由处理了。

来，再简单一点就是Teams Client告诉Phone System我在哪里，Phone System再告诉本地SBC它在哪里，然后SBC针对这些信息做路由

| Header name | Values | Comments |
|:------------|:-------|:-------|
| X-MS-UserLocation | internal/external | Indicates if user is internal or external |
| Request-URI	INVITE sip: +84439263000@VNsbc.contoso.com SIP /2.0 | SBC FQDN | The FQDN which is targeted for the call even if the SBC is not directly connected to Direct Routing |
| X-MS-MediaPath | Example: proxysbc.contoso.com, VNsbc.contoso.com | Order of SBCs that should be used for Media path between the user and target SBC. The final SBC is always last |
| X-MS-UserSite | usersiteID | String defined by tenant administrator |

![image-20200512122432360](https://github.com/tangx007/tangx007.github.io/raw/master/_posts/%E5%AE%9E%E6%88%98MS-Teams-Direct-Routing%E4%B8%AD%E7%9A%84%E6%9C%AC%E5%9C%B0%E5%AA%92%E4%BD%93%E6%B5%81%E4%BC%98%E5%8C%96/image-20200512122432360.png)

## 配置步骤

所以针对上面的逻辑，配置本地媒体流优化需要有以下这些步骤（你可以使用Teams Admin Portal 或者 Powershell）

### 首先，我们先登陆到Teams Powershell：

```
#解决mfa不弹出认证页面的问题
#解决lyncdiscover在内网不解释的问题。OverrideAdminDomain
$String = "password"
$username = "user@contoso.onmicrosoft.com"
$TenantDomain = "contoso.onmicrosoft.com"
Import-Module SkypeOnlineConnector;$PWord = ConvertTo-SecureString -String $String -AsPlainText -Force;
$Cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $username, $PWord;
$CSSession=New-CsOnlineSession -credential $Cred -OverrideAdminDomain $TenantDomain
Import-PSSession $cssession -AllowClobber
```
### 接下来，我们根据刚刚说到的站点架构图来配置Teams

LMO有两场景两模式，但实际的项目中我们很多会用到 Central SBC + Always by pass的组合，而且是比较容易落地的方案，原因为以下：

- 如果用Proxy SBC的方案，子站点的SBC都需要用认证的SBC （除非是全新的部署，不然很难说服客户重新使用其它SBC）
- 从理论上来看，Proxy SBC的方案的确是优化了不了媒体流的路径，但配置难度比较大（Teams上面的站点信息配置，两台SBC之间的对接配置，Proxy SBC的配置，等）。
- 分支站点完成可以直接使用认证SBC与MS Phone System对接，各自的站点走各自的SBC出局，各不影响。
- 如果使用【Proxy SBC的场景】的话，就是所有鸡蛋放在一个篮子里面，一旦Proxy SBC中断，所有用户的语音服务都受到影响。
- 必须使用【Proxy SBC的场景】的情况是分支站点缺乏公网IP资源 或 没有本地的Internet 那就需要用Proxy SBC来解决了，只是现实情况是不是特别可能吧？

## 那说了那么多，我们用以下命令来配置 Central SBC的方案吧，非常简单：

```powershell
# Step1, 首先配置信任IP，代表每一台Proxy SBC or Central SBC的公网IP，告诉Phone System需要做LMO的SBC IP是多少。
New-CsTenantTrustedIPAddress -IPAddress 172.16.240.133 -MaskBits 32 -Description "Singapore site trusted IP"

# Step2, 定义Region -> 定义NetworkSite,并关联Region -> 定义子网,并关联到NetworkSite
# 就是192.168.3.0子网属于Singapore子网，并属于APAC Region
New-CsTenantNetworkRegion -NetworkRegionID "APAC"  
New-CsTenantNetworkSite -NetworkSiteID "Singapore" -NetworkRegionID "APAC"
New-CsTenantNetworkSubnet -SubnetID 192.168.3.0 -MaskBits 24 -NetworkSiteID “Singapore”

# Step3, 最后在Teams上面创建Proxy SBC的网关，并指定它的站点名称为Singapore, 打开媒体旁路，设置模式为Always
New-CSOnlinePSTNGateway -Identity “proxysbc.contoso.com” 
Set-CSOnlinePSTNGateway -Identity “proxysbc.contoso.com” -GatewaySiteID “Singapore” -MediaBypass $true -BypassMode “Always” -ProxySBC $null

```

最后就是长成以下架构图：

![image-20200512124823433](_posts/实战MS-Teams-Direct-Routing中的本地媒体流优化/image-20200512124823433.png)

效果就是每一通Teams Direct Routing 呼叫都会带有X-MS包关传递给SBC来做路由处理：

![image-20200512122432360](C:\Users\Nemo\Documents\GitHub\tangx007\_posts\实战MS Teams Direct Routing中的本地媒体流优化\image-20200512122432360.png)



> # 【未完】在SBC侧的配置如何？
