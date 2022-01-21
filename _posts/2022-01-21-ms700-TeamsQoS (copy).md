### 【输出文章】 configure QoS port range and DSCP markings

什么是QoS? Quality of Service， 这是一种网络层面的数据包“插队“技术，也可以理解为”绿色通道“ & ”优先“。具个例子，如高速公路的应急车道，原则上只能由军用车，医疗车等车辆通行，这样可以保证在高速上堵车时，这些特别用途的车辆也能快速通行。那么QoS也就是类似这种功能，我们可以为特别用途的网络流量通过打标签的方式，优化使用网络设备。

![Illustration of QoS queues and bandwidth division.](file:///C:/Users/Nemo/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

对于Teams Voice/Audio/Video这些实时通讯应用来说（其实包括现在流行的视频会议软件/腾讯会议/Zoom/Webex等等），它们的数据流量对于网络层的掉包/抖动/延迟都是非常敏感的，从而会影响到通讯过程中用户体验。

 

当你的Teams用户特别多，使用量特别大的时候，而没有部署QoS，就可能会出现以下质量故障了：

·    ‎抖动（Jitter） – 以不同速率到达的媒体数据包，这可能导致呼叫中缺少单词或音节‎

·    ‎数据包丢失（Packet Loss） - 数据包丢失，这也可能导致语音质量降低和难以理解的语音‎

·    ‎延迟往返时间 （RTT） – 媒体数据包需要很长时间才能到达目的地，这会导致对话中的双方之间出现明显的延迟，并导致人们相互交谈‎

而对于网络质量不敏感的应用程序（如邮件，网页，下载，等等）来说，就算网络不好的时候，在用户体验方面也不会有太大的影响，除非实在是太慢太卡的情况，但至少是能收到邮件，看到网页，程序能下载回来。所以对于Teams这些实时通讯软件来说，我们如何来提高它的网络容错性，网络优先级对于企业的网络管理员来说确实一项挑战。

当然，最直截了当的方法是增加出口带宽来解决，但是贵。那么便宜而长效的解决方案便是导入QoS，之后才是考虑增加带宽。

\# 如何部署QoS呢？

为了让你辛辛苦苦建设的QoS体系可以更加有效果，需要有三个地方来实施QoS，分别是

1、 位置O365上面的Teams服务。

2、 企业所有的网络设备，至少是有Teams流量的网络设备。

3、 所有安装了Teams客户端的电脑。

![Diagram  Description automatically generated](file:///C:/Users/Nemo/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

 

对于第一点，我们需要在“TAC》会议》会议设置》网络“ 中把QoS打开即可，如下图：

![Graphical user interface, application  Description automatically generated](file:///C:/Users/Nemo/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

 

对于第二点，所有的网络设备配置QoS。

首先需要介绍一下QoS Queue，当所有的网络流量都经过一台交换机的时候，它们都会在同一个列队里面按照FIFO原则（First in, first out）来进出交换设备，包括音视频这些实时通讯流量，如果你的流量太大而出口带宽不足时，它们就会Get Stuck了。所以我们可以在网络设置上面创建多个列队，为它们分配优化级，标签，带宽大小等参数，这样通过调整队列的优化级来达到“插队“的能力，常用的方法如Cisco’s priority queuing & Class-Based Weighted Fair Queueing (CBWFQ) & weighted random early detection (WRED).

接着就是标签（DSCP），每一个实时流量的数据包头都会打上一个DSCP来区分流量用途，例如DSCP 46属于音频，DSCP 34属于视频，DSCP 18属于共享数据，这样就很容易区分出来哪个DSCP需要被优先了。最后，请找网络管理员实施QoS吧。

 

对于第三点，如何为Teams客户端配置QoS呢？

说白了，就是为每个Teams的音视频流打上一个DSCP标签。

Teams客户端的QoS使用Windows Group Policy Objects（组策略） 与Port-based Access Control Lists（基于端口的准入列表）来标识哪些音视频流是需要被网络设备优先通过的。

![A screenshot of a computer screen  Description automatically generated with medium confidence](file:///C:/Users/Nemo/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

以下是在客户端上使用GPO的方式为Teams流量打标签

New-NetQosPolicy -Name "Teams Audio" -AppPathNameMatchCondition "Teams.exe" -IPProtocolMatchCondition Both -IPSrcPortStartMatchCondition 50000 -IPSrcPortEndMatchCondition 50019 -DSCPAction 46 -NetworkProfile All

New-NetQosPolicy -Name "Teams Video" -AppPathNameMatchCondition "Teams.exe" -IPProtocolMatchCondition Both -IPSrcPortStartMatchCondition 50020 -IPSrcPortEndMatchCondition 50039 -DSCPAction 34 -NetworkProfile All

New-NetQosPolicy -Name "Teams Sharing" -AppPathNameMatchCondition "Teams.exe" -IPProtocolMatchCondition Both -IPSrcPortStartMatchCondition 50040 -IPSrcPortEndMatchCondition 50059 -DSCPAction 18 -NetworkProfile All

![Graphical user interface  Description automatically generated](file:///C:/Users/Nemo/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

如何验证运行完这些命令后DSCP真是打上了吗？非常简单，重启Teams并开启Teams会议，打开wireshark，并过滤udp流量，如下图：

你会发现从本地：192.168.96.163到Teams服务器52.112.x.x的数据包中就会多出来DSCP标签，值为46，即从上表显示的音频的流量了。

![Graphical user interface, text, application, email  Description automatically generated](file:///C:/Users/Nemo/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

最后，我们的网络管理员只需要在交换设备上面正确配置好QoS队列，并监测到这个队列确实有流量通过，就可以大功造成了。
