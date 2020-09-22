---
layout:     post
title:      MOS值
subtitle:  xxx
date:       2020-9-16
author:  Nemo
header-img: img/post-bg-universe.jpg
catalog: true
tags:

- 平均主观意见分
- 定义-故事即场景(Story-aa-Scenario)-终端-应用-服务
---

# 什么MOS(平均主观意见分)

## 定义是什么

记录受众意见的平均值来评估音频或视频的质量，是一个了解与优化语音质量的一个工具，它提供了5个级别的评分，便于我们来评价语音/音频的质量情况。它可以帮助设备制造商来定义产品的“好”与”坏“

正如SLA一样

当我们进行语音通讯出现问题的时候，大家都可能会用一些方式来描述当前的问题，例如以下：

- 你的声音听起来很生硬
- 几乎听不见你说的话
- 你的声音卡住了
- 声音有点混乱
- 我没有听清楚你说话

所以我们需要用一种量化的方法来描述这现现象

MOS3.6是一个什么情况？如何形象化？

<img src="C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916062722045.png" alt="image-20200916062722045" style="zoom:50%;" />

拿汽车的燃油经济性来与MOS评分作为对比：

如何理解？？？

![image-20200916070750579](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916070750579.png)

最后，我们来听听不同MOS评分下的音频质量是如何的：

MOS-DIFF-Video_2020-09-16_070037



## MOS的计算方法

分类：主观MOS与客观MOS

建议的计算主观MOS值的方法是，例如，通过待测试的设备来播放一段语音，四个不同的被测试人员会为这段语音进行评分，然后做一个平均计算之后，得出大概的主观MOS评分。

![image-20200916064142638](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916064142638.png)



## 微软是如何定义的？



# MOS值的分类

客观MOS，主观MOS，综合MOS

### 客观MOS

通过ITU-T P.800的一项国际标准来定义的，具体分为两个标准:

- PESQ-P862.3: http://www.itu.int/rec/T-REC-PS862.3/en
- POLQA-P.863.1: http://www.itu.int/rec/T-REC-P.863.1/en

POLQA是专门设计来评价IP语音的，在它的计算标准里面有两个比较重要的指标是Frame Repeats（帧重复）与Delay Jumps（延迟跳跃，掉包），在基于网络传输的语音通讯中这两指标好坏，非常影响语音通讯的质量。现在一般的客户MOS测试都会使用POLQA来进行的。

在过去的一些美好时光里面，我们打一通电话，你将拥有从A到B端所有的铜缆，你的通话有固定的延迟，没有所谓的Frame Repeats（因为以前的是模拟电话，现在基本上是数字电话），你们通话的MOS分数基本上可以达到4.0以上。

![](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916071538368.png)

### 主观MOS

![image-20200916083832209](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916083832209.png)



如果进行客观MOS评分

实际的对话场景是这样的：xxxx

![image-20200916073302909](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916073302909.png)

在实验里面，通过分析机器。。。

![image-20200916073516124](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916073516124.png)

最后，对比来自A与B两端的两段音频来

![image-20200916073440602](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916073440602.png)

# MOS在实际场景中的作用

音频与视频设备上的应用

什么叫音频带宽？如何解释？NB,WB,SWB,FB

带宽是如何影响MOS评分的？带宽，编码，MOS





![image-20200916075703425](C:\Users\Nemo\AppData\Roaming\Typora\typora-user-images\image-20200916075703425.png)

# MOS是如何应用在终端中的

# 我们如何来把MOS作为服务标准

# 链接

Recommendation P.800.1 (07/16)：https://www.itu.int/rec/T-REC-P.800.1-201607-I/en









