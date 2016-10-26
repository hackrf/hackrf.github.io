---
ID: 919
title: "HackRF.net参加的CSDN开源夏令营正式开始报名啦"
author: scateu
date: 2014-06-16 09:18:28
post_excerpt: ""
layout: post
published: true
views:
  - "4835"
duoshuo_thread_id:
  - "1312073613704167525"
---

6个题目的描述见<https://github.com/scateu/HackRF_CSDN_SummerOfCode_2014>

# 提案名称： 移植HackRF驱动到Android设备上

## 提案内容

**提案描述：**

HackRF是一款历史上首次从软件、固件、电路原理图和PCB板图完全开源而毫无保留的软件无线电平台，可以覆盖10MHz - 6GHz，已经在kickstarter上成功融资。将会使得更多的软件工程师、Linux黑客们获得直
接操作无线电波的可能。同时，对无线信号的安全分析也将会成为人们关注的新领域。

目前HackRF必须依赖于电脑才行工作，这给用户在外场进行使用带来了极大的不便。

Youtube上已经有人在一个Linux平板电脑上跑通了HackRF的驱动和gqrx程序。
同时，Android上也有人开发了[SDR Touch](https://play.google.com/store/apps/details?id=marto.androsdr2)等程序，证明了这个方案的可行性。

因此，我们的目标是在Android设备上使用USB On-the-go的方式将HackRF驱动起来

HackRF基于libusb库实现的USB设备驱动，提供libhackrf和hackrf-utils等源代码。

并且将基于Qt的gqrx程序移植到Android平板电脑上，打包成APK

具体要求:

1. 在安卓平台上移植libhackrf和hackrf-utils
2. 利用Qt的跨平台功能将gqrx移植到Android平台
3. 将libhackrf与gqrx进行对接，打包成为apk

**项目代码地址：**
 - <https://github.com/mossmann/hackrf>
 - <http://gqrx.dk/>
 - [SDR Touch](https://play.google.com/store/apps/details?id=marto.androsdr2)

**计划：**

* 中期检查前完成HackRF和gqrx和移植
* 结题之前完成APK的打包

**联系方式：**

* email: <cuckoohello@gmail.com>

## 对学生的要求

* 了解 Android 开发流程
* 了解 libusb 或一般USB驱动程序工作原理
* 对 Linux 上的一般程序编译/CMake/Makefile有了解

## 完成标准

 + hackrf_transfer程序运行之后不报错,OTG供电工作正常
 + APK打包后的gqrx可以直接在一般Android平台上运行
 + gqrx程序在20MHz采样率的工作状态下解调FM成功

# 提案名称： 为HackRF提供一个网页版的解调前端

## 提案内容

**提案描述：**

HackRF是一款历史上首次从软件、固件、电路原理图和PCB板图完全开源而毫无保留的软件无线电平台，可以覆盖10MHz - 6GHz，已经在kickstarter上成功融资。将会使得更多的软件工程师、Linux黑客们获得直
接操作无线电波的可能。同时，对无线信号的安全分析也将会成为人们关注的新领域。

目前HackRF在计算机上的程序大部分是基于传统GUI程序提供的，例如gqrx基于Qt，SDRSharp基于C#。

我们现在想要实现一个基于浏览器/Javascript/Node.js的信号解调前端， 这样使用者在远程通过浏览器就可以直接对常见的信号进行观察，并能完成简单的WBFM NBFM AM解调的功能。

同时希望尽可能少的使用外部依赖库，尽可能优化其性能，使得能够在Raspberry Pi等嵌入式计算机上运行。这样用户就可以方便地部署天线架设更容易的场合。

具体要求:

1. 初步实现Node.js与libhackrf的集成
2. 在Node.js上初步实现FFT显示的功能
3. 使用Node.js实现瀑布图的功能
4. 基于Javascript实现FM/AM/SSB的解调功能
5. 优化网页前端，使得网页能够在iOS等移动设备的浏览器上正常运行
6. 在Raspberry Pi上优化性能
7. 发布代码,发布教程

**项目代码地址：**
 - <https://github.com/mossmann/hackrf>
 - <http://gqrx.dk/>
 - <http://websdr.org/>
 - <http://websdr.ewi.utwente.nl:8901/>
 - <https://github.com/kpreid/shinysdr>

**计划：**

* 中期检查前完成Node.js为后台的网页前端，实现FFT显示和瀑布图
* 结题之前完成FM/AM/SSB解调的功能，并实现在Raspberry Pi上部署

**联系方式：**

* email: <cuckoohello@gmail.com>


## 对学生的要求

* 熟悉 Javascript 开发
* 熟悉 Node.js 开发
* 基本了解FM解调的原理
* 在导师的指导下能理解 FFT 的基本思路，从而完成编码

## 完成标准

* 在常见浏览器上可以使用
* 可以正常观察信号的FFT和瀑布图
* 能够解调FM/AM/SSB信号
* 在Raspberry Pi上能够跑通

# 提案名称： 基于HackRF开发GPS信号仿真模拟器

## 提案内容

**提案描述：**
研究并实现基于HackRF的GPS信号仿真模拟器，使得用户在电脑上给定地理坐标之后，通过HackRF发射出各卫星对应GPS模拟信号，并能通过一般的GPS接收机得到物理验证。

GNSS-SDR是德国同学发起的使用软件无线电来对接收到的卫星定位系统信号进行处理的项目，在开发信号生成部分时可以参考。

HackRF是一款历史上首次从软件、固件、电路原理图和PCB板图完全开源而毫无保留的软件无线电平台，可以覆盖10MHz - 6GHz，已经在kickstarter上成功融资。将会使得更多的软件工程师、Linux黑客们获得直
接操作无线电波的可能。同时，对无线信号的安全分析也将会成为人们关注的新领域。

GNURadio开源软件无线电框架提供了全面的对各种通信系统常用的信号处理模块，并提供了完整的信号开发模块，并已经形成完善良好的开源生态环境。但由于长久以来没有价格平易近人的硬件外设方案，使得GNURadio的学习门槛较高。HackRF的出现将使得开源软件无线电被大众普遍认识提供了可能。


具体要求:

1. 分析GPS的信号原理
2. 使用Matlab完成信号的仿真
3. 调试基带信号，使其能够通过HackRF发射出来，并可以在手机上得到验证
4. 不依赖于Matlab完成信号的生成
5. 发布代码及教程

**项目代码地址：**
 - <https://github.com/mossmann/hackrf>
 - <http://gnss-sdr.org>

**计划：**

* 中期检查前完成Matlab生成的IQ信号仿真，通过HackRF发射后可以被一般GPS接收机解析
* 结题之前完成不依赖于Matlab的信号生成及HackRF发射

**联系方式：**

* email: <putaoshu@gmail.com>

## 对学生的要求

* 熟悉信号处理知识
* 了解GPS定位的原理
* 对射频调试有一定的概念

## 完成标准

* Matlab生成的信号可以通过软件解调
* 可以指定输入经度/纬度
* 提供一个简单的命令行参数实用程序，直接对接hackrf_transfer程序

# 提案名称： 基于HackRF进行iBeacon协议分析

## 提案内容

**提案描述：**

GNURadio开源软件无线电框架提供了全面的对各种通信系统常用的信号处理模块，并提供了完整的信号开发模块，并已经形成完善良好的开源生态环境。但由于长久以来没有价格平易近人的硬件外设方案，使得GNURadio的学习门槛较高。HackRF的出现将使得开源软件无线电被大众普遍认识提供了可能。

HackRF是一款历史上首次从软件、固件、电路原理图和PCB板图完全开源而毫无保留的软件无线电平台，可以覆盖10MHz - 6GHz，已经在kickstarter上成功融资。将会使得更多的软件工程师、Linux黑客们获得直
接操作无线电波的可能。同时，对无线信号的安全分析也将会成为人们关注的新领域。

iBeacon是苹果公司2013年9月发布的移动设备用OS（iOS7）上配备的新功能。其工作方式是，配备有 低功耗蓝牙（BLE）通信功能的设备使用BLE技术向周围发送自己特有的ID，接收到该ID的应用软件会根据该ID采取一些行动。比如，在店铺里设置iBeacon通信模块的话，便可让iPhone和iPad上运行的应用软件将顾客进入店铺这一资讯告知服务器，或者由服务器向顾客发送折扣券及进店积分。此外，还可以在家电发生故障或停止工作时使用iBeacon向应用软件发送资讯，通知家电故障。

目前GNURadio已经提供了gr-bluetooth模块，可以供参考。

具体要求:

1. 使用已有的Bluetooth USB Dongle和iPad/iPhone跑通基本的iBeacon原理，写出教程
2. 跑通现有的gr-bluetooth模块
3. 基于现有的gr-bluetooth模块对iBeacon信号进行分析处理
4. 使用gr_modtool封装成为易于使用的工具集gr-ibeacon
5. 发布代码
6. 发布教程文章

**项目代码地址：**
 - <https://github.com/mossmann/hackrf>
 - <https://github.com/greatscottgadgets/gr-bluetooth>

**计划：**

* 中期检查前完成iBeacon信号原理的分析，能生成简单的iBeacon信号被苹果设备解析
* 结题之前完成gr-ibeacon模块的编写

**联系方式：**

* email: <putaoshu@gmail.com>


## 对学生的要求

* 能够在导师的指导下理解iBeacon/Bluetooth/GMSK
* 熟悉一般的 C++ 代码编写
* 对 Linux 下的软件开发有兴趣

## 完成标准

* gr-ibeacon模块可以发射iBeacon信号，被Apple设备解析
* gr-ibeacon模块可以接收由Apple设备发射的iBeacon信号，解析出传输的信息

# 提案名称： 基于HackRF的开源伪基站检测

## 提案内容

**提案描述：**

目前不法分子使用OpenBTS项目搭建群发短信的伪基站，
本项目旨在分析伪基站的特征，使用HackRF的GSM解调功能对伪基站进行定位和捕获。

GNURadio开源软件无线电框架提供了全面的对各种通信系统常用的信号处理模块，并提供了完整的信号开发模块，并已经形成完善良好的开源生态环境。但由于长久以来没有价格平易近人的硬件外设方案，使得GNURadio的学习门槛较高。HackRF的出现将使得开源软件无线电被大众普遍认识提供了可能。

HackRF是一款历史上首次从软件、固件、电路原理图和PCB板图完全开源而毫无保留的软件无线电平台，可以覆盖10MHz - 6GHz，已经在kickstarter上成功融资。将会使得更多的软件工程师、Linux黑客们获得直
接操作无线电波的可能。同时，对无线信号的安全分析也将会成为人们关注的新领域。

具体要求:

1. 分析现有的伪基站项目代码，写出伪基站的工作方式
2. 找到伪基站的工作Pattern，给出文档
3. 使用HackRF的GSM解调功能，对伪基站发出的信号进行判别
4. 发布代码及文档

**项目代码地址：**
 - <https://github.com/mossmann/hackrf>
 - <http://openbts.org>
 - <https://svn.berlin.ccc.de/projects/airprobe/>

**计划：**

* 中期检查前完成伪基站工作原理的分析，发布分析报告
* 结题之前完成伪基站信号特征识别的验证代码

**联系方式：**

* email: <scateu@gmail.com>

## 对学生的要求

* 熟悉 GSM 通信原理
* 在导师的指导下可以完成GSM伪基站的原理分析
* 熟悉数字信号处理的代码编写

## 完成标准

* 发布伪基站工作方式分析报告
* 伪基站特征检测原理代码验证通过，能够从原理上区分正常基站与伪基站

# 提案名称： 基于HackRF的矢量信号源和矢量信号分析仪

## 提案内容

**提案描述：**

使用HackRF实现一个简单的矢量信号源和矢量信号分析仪的功能，这样就可以坐在家中享用六位数或七位数射频仪表的功能了。

HackRF是一款历史上首次从软件、固件、电路原理图和PCB板图完全开源而毫无保留的软件无线电平台，可以覆盖10MHz - 6GHz，已经在kickstarter上成功融资。将会使得更多的软件工程师、Linux黑客们获得直接操作无线电波的可能。同时，对无线信号的安全分析也将会成为人们关注的新领域。

本题目由清华大学电子系的老师指导，所以不用太担心通信原理知识的问题。

具体要求:

1. 实现PSK、QAM以及更多的矢量信号产生，如BPSK/QPSK/8PSK/16QAM/64QAM等
2. 可调整调整基带滤波器的参数、符号率
2. 实现PSK/QAM信号的星座图显示
3. 计算PSK/QAM信号的EVM
4. 使用Qt或者PyQt或者Javascript完成一个简单的GUI界面
5. 发布代码并发布文档

**项目代码地址：**
 - <https://github.com/mossmann/hackrf>

**计划：**

* 中期检查前完成信号生成和星座图显示的功能
* 结题之前完成EVM计算和GUI界面

**联系方式：**

* email: <hackrf@sina.cn>


## 对学生的要求

* 了解基本的通信原理与数字通信原理
* 对于数字信号处理的C++编程有一定的经验

## 完成标准

1. 生成的矢量信号可以通过安捷伦的射频仪表验证
2. 生成的信号功率、频率、符号率、调制方式
3. 接收的矢量信号的格式、符号率、频率，并可显示星座图
4. 接收的矢量信号EVM值可以计算得出
 

---

<div>

5月28日起，CSDN正式推出“开源夏令营” 技术公益活动。在校的学生可以参与，通过考评后可获得5000元奖金。

<!--more-->

目前正式接受同学们报名啦: <a href="http://code.csdn.net/os_camp/proposals?org_id=6">http://code.csdn.net/os_camp/proposals?org_id=6</a>

各位要参加开源夏令营，目前在大四，已经保研的同学，CSDN产品经理给出的方案是：
<blockquote>自行修改注册信息

修改入学时间为研究生入学时间

学制改为研究生学制</blockquote>
HackRF.net开源软件无线电社区将作为组织参与此次夏令营，全程参与开源项目的实施。现在已经确定了6个题目，同学们可以在6月16日–7月4日期间进行报名。选择两个感兴趣的项目主题，与导师沟通后提交开题报告。导师会进行评分，并最终选定一名学生作为项目参与者。成功参与项目的同学在7月5日–8月3日期间，按照时间表完成项目，通过中期检查和最终评定即可获得奖金。

本次活动面向开源软件、开源社区和国内高校学生，旨在引导高校学生利用其暑期空闲时间，为开源软件、开源社区做出贡献，推动国内开源软件和开源社区的发展，推动开源在高校的深入，搭建起开源社区、企业与高校学生之间的桥梁，建设高质量高水平的开源生态环境。

开源夏令营是一种“开源项目实习”，项目的实施时间大致为2个月，学生参加并成功完成后可以获得5000人民币（含税）奖金，这与学生参加暑期实习所获得的报酬相当，CSDN高校俱乐部还会为顺利完成项目的同学出具 “暑期实习证明”。
<h2 id="时间表">时间表</h2>
<ul>
	<li>5月28日 – 6月13日: 组织报名和发布提案</li>
	<li>6月16日 -  7月4日: 学生选题、提交开题报告, 导师选定学生</li>
	<li>7月5日  -  8月3日: 第一个月实习期</li>
	<li>8月4日  -  8月8日: 中期检查</li>
	<li>8月4日  -  9月7日: 第二个月实习期</li>
	<li>9月8日  – 9月12日: 最终考评</li>
</ul>
<h2 id="HackRF.net开源软件无线电社区提交的提案">HackRF.net开源软件无线电社区提交的提案</h2>
<ul>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/7">为HackRF提供一个网页版的解调前端</a> – 导师:单亚峰</li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/6">基于HackRF的矢量信号源和矢量信号分析仪</a> – 导师:李尚远</li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/5">基于HackRF进行iBeacon协议分析</a> – 导师:<a href="http://sdr-x.github.io">焦现军</a></li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/4">基于HackRF开发GPS信号仿真模拟器</a> – 导师:<a href="http://sdr-x.github.io">焦现军</a></li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/3">移植HackRF驱动到Android设备上</a> – 导师:王康</li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/2">基于HackRF的开源伪基站检测 </a>- 导师:王康</li>
</ul>
开源夏令营为学生提供了一次极佳的锻炼机会。利用暑期时间，在社区大牛的一对一辅导下实际参与项目，获得宝贵的实习经历，将对自身知识和技能提升有很大的帮助。与大牛一起参加CSDN开源夏令营，你准备好了吗？

想要了解关于“开源夏令营”的更多信息，请查看这里：<a href="http://code.csdn.net/os_camp">http://code.csdn.net/os_camp</a>

想了解更多HackRF.net开源软件无线电社区的提案项目，请查看这里：<a href="http://code.csdn.net/os_camp/proposals?org_id=6">http://code.csdn.net/os_camp/proposals?org_id=6</a>

&nbsp;
<h2>HackRF.net社区介绍</h2>
HackRF.net 开源软件无线电社区是一个致力于在国内推广和发展SDR(Software Defined Radio, 软件无线电)概念的开源社区。目标是为了让熟悉Linux的软件工程师们快速获得一个通向无线电世界的途径。

</div>
&nbsp;

&nbsp;
<div>
<h2>参与本次开源夏令营导师的简要介绍</h2>
</div>
<div>
<h3>单亚峰</h3>
清华大学自动化系毕业，<wbr />现在全路通信信号研究设计院有限公司工作。曾经担任过清华大学《<wbr />嵌入式系统》课程助教，对于node.<wbr />js和嵌入式系统都有比较深入的研究。在校期间是<a href="http://mirrors.tuna.tsinghua.edu.cn" target="_blank">mirrors<wbr />.tuna.tsinghua.edu.cn</a>开源镜像站的维护者<wbr />之一。

由于在国内安装GNURadio和HackRF开发环境时，<wbr />需要通过国际网进行git clone，网络连接非常不稳定，<wbr />于是单亚峰为HackRF及GNURadio提供了一套国内镜像<wbr />脚本，每天自动定时从上游进行镜像同步。该镜像出现之 后，为大量的需要安装GNURadio的用户解决了燃眉之急，<wbr />使得开发环境的搭建时间节省大半以上。
<ul>
	<li>Blog: <a href="http://blog.kokonur.me" target="_blank">http://blog.kokonur.me</a></li>
	<li>github:<a href="https://github.com/cuckoohello" target="_blank"> https://github.com/cuckoohello</a></li>
</ul>
提案内容:
<ul>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/7">为HackRF提供一个网页版的解调前端</a></li>
</ul>
</div>
<div>
<h3>李尚远</h3>
李尚远老师来自清华大学电子工程系，主要研究方向为光通信。<wbr />目前正在探索开源软件无线电平台在科研、教学方面的应用。
<ul>
	<li>github: <a href="https://github.com/dovecho" target="_blank">https://github.com/dovecho</a></li>
</ul>
提案内容:
<ul>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/6">基于HackRF的矢量信号源和矢量信号分析仪</a></li>
</ul>
</div>
<div>
<h3>焦现军</h3>
焦现军博士曾经在Nokia Research Center工作（现隶属微软）。为HackRF.<wbr />net社区贡献过多篇高质量的教程，例如著名的《<a href="http://sdr-x.github.io/%E5%AE%8C%E6%95%B420MHz%E5%B8%A6%E5%AE%BD%E9%85%8D%E7%BD%AELTE%E4%BF%A1%E5%8F%B7%E8%A2%ABHACKRF-19.2M%E9%87%87%E6%A0%B7%E7%8E%87%E6%88%90%E5%8A%9F%E8%A7%A3%E6%9E%90/"><wbr />使用HackRF解析TDD LTE信号</a>》便是出自焦现军博士之手。

从中学时DIY收音机开始，对无线电以及通信产生浓厚兴趣，<wbr />从读书到工作至今一直在做通信信号处理。<wbr />坚信开源软件无线电会改变无线基带芯片生态系统 完全封闭的现状，给通信领域带来深远影响。<wbr />目前正尝试做手机测开源LTE基带（基于HackRF，<wbr />已可解调LTE SIB消息，见 <a href="http://github.com/JiaoXianjun/LTE-Cell-Scanner" target="_blank">github.com/JiaoXianjun/LTE-<wbr />Cell-Scanner</a> ），欢迎加入！
<ul>
	<li>github：<a href="https://github.com/JiaoXianjun" target="_blank">https://github.com/<wbr />JiaoXianjun</a></li>
	<li>research: <a href="https://www.researchgate.net/profile/Xianjun_Jiao/publications" target="_blank">https://www.researchgate.net/<wbr />profile/Xianjun_Jiao/<wbr />publications</a></li>
	<li>博客：<a href="http://sdr-x.github.io" target="_blank">http://sdr-x.github.io</a></li>
</ul>
提案内容:
<ul>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/5" target="_blank">基于HackRF进行iBeacon协议分析</a></li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/4" target="_blank">基于HackRF开发GPS信号仿真模拟器</a></li>
</ul>
</div>
<div>
<h3>王康</h3>
清华大学工程物理系核工程与核技术专业出身。<wbr />在校期间曾任清华大学学生网管会(TUNA) 首任会长。热衷于开源事业，<wbr />曾组织过Ubuntu光盘免费发放活动。曾经给Linux Kernel提交过TDD LTE USB Dongle的驱动(见Kernel Changelog 3.10, 3.11) 。

现在在主要在国内 推广开源软件无线电， 任HackRF.net软件无线电社区的区长。
<ul>
	<li>github: <a href="https://github.com/scateu" target="_blank">https://github.com/scateu</a></li>
	<li>博客：<a href="http://scateu.me" target="_blank">http://scateu.me</a></li>
</ul>
提案内容:
<ul>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/3" target="_blank">移植HackRF驱动到Android设备上</a></li>
	<li><a href="http://code.csdn.net/os_camp/6/proposals/2" target="_blank">基于HackRF的开源伪基站检测</a></li>
</ul>
</div>
<div>
<h2>请简单介绍下HackRF.net社区的基本情况</h2>
HackRF.net软件无线电社区成立于2014年初。<wbr />在软件无线电概念在国外已经推广多年，<wbr />而目前国内现有的关于软件无线电社区还停留在对相应平台板卡的销<wbr />售促进上。

HackRF.net软件无线电社区借HackRF平台的推出，<wbr />先期为HackRF提供技术支持，长远来看，<wbr />要为国内关注软件无线电的同学们提供一个交流、互通有无的社区，<wbr />定期举办线上线下的技术沙龙活动。

</div>
<div>
<h2>能否简单介绍下HackRF.net社区的核心成员都有哪些？</h2>
目前社区已经有成员接近200人，核心成员有来自星天无 限公司、360公司、华为公司、中国移动、清华大学、北京大学、<wbr />上海海事大学、海南大学、上海大学、浙江大学、重庆邮电大学、<wbr />北京邮电大学、北京理工大学、北京交通大学、阿里集团、百度公司等单位的同学，<wbr />同时也有许多独立的安全分析员例如kevin260<wbr />0也在HackRF.net社区中。

</div>
<div>
<h2>学生在本社区中所占比例如何？<wbr />如何看待学生群体在开源项目发展中的作用？</h2>
目前学生在社区中所占的比例约为一半，<wbr />大部分为通信专业的研究生，小部分为计算系类专业的本科生。

我认为随着时代的发展，同学们对于开源理念、<wbr />开源版权协议的越来越认同。加上同学们在平时的课程设计、<wbr />大作业、毕业设计、平时的小项目中用到的开源项目越来越多，<wbr />我相信以后学生群体将会成为开源项目发展中的主流砥柱的力量。

</div>
<div>
<h2>HackRF 设备的应用场景有哪些？能否举出几个基于有趣的应用项目？</h2>
目前HackRF平台的主要应用场景有以下四个方面

</div>
<div>
<h3>认知教育</h3>
在传统的通信课程中，由于通信设备极为昂贵，<wbr />长期以来通信课程的学习只能靠同学们的“脑补”。<wbr />有了低成本的软件无线电平台之后，我们可以利用 GNURadio的强大的信号处理模块将课本上关于通信系统的描<wbr />述文字，真实地在物理世界中得到验证。<wbr />这将会给无线电的认知学习带来巨大的改善。

同时，HackRF的接口还可以直接与Matlab对接，<wbr />从而使现有的通信专业的同学们不需要再额外学习其它的编程环境，<wbr />就可以将原有的Matlab仿真数据直接发送到空中，<wbr />这将会使的无线电教育研究的效率大大提升。

</div>
<div>
<h3>原型机设计</h3>
在传统的无线电行业中，一般的开发流程是，先制定通信制式，<wbr />然后布PCB电路板并打样加工，然后编写数字信号处理程序，<wbr />最后进行调试。这期间的硬件加工周期会非常长，<wbr />并且不适用于原理样机所需要的快速迭代开发模式。

引入软件无线电平台之后，<wbr />所有的原理样机的试制不再需要进行硬件设计加工，<wbr />所有的通信算法都可以在计算机上执行。<wbr />这样会极大的加快产品的原型机定型工作。

</div>
<div>
<h3>创客团队/物联网</h3>
当前的创客创新活动主要集中于无线、传感领域。<wbr />而在这些领域的研究开发，一定离不开诸如频谱分析仪、<wbr />矢量信号源、矢量网络分析仪等仪表的支持。但是 传统的射频仪表极为昂贵，一台仪表动辄数十万元，<wbr />即便是二手的老旧仪表也要数万元。<wbr />这对于刚刚起步的创客团队来说，无疑是非常大的挑战。

HackRF软件无线电平台以其频谱覆盖广(10MHz - 6GHz)，可以覆盖物联网、无线传感、RFID、蓝牙、<wbr />433MHz、GSM、CDMA、LTE等通信制式。 HackRF将会对从事智能硬件创新的创客团队，<wbr />提供便利的调试环境。

</div>
<div>
<h3>无线安全</h3>
随着对信息安全的重视程度被提到国家战略层面上以来，<wbr />安全越来越成为各个厂商、机关、单位、公司关心的话题。<wbr />随着传统互联网安全领域的游戏规则的确定，<wbr />传统安全领域的跑马圈地活动基本结束。<wbr />无线安全领域作为一个新兴的方向，<wbr />将会是各大安全团队关注的新焦点。

在传统的无线遥控出现的时代，要想实现对无线遥控信号的分析、<wbr />破解，需要昂贵的仪器和计算资源的支持。<wbr />因此传统的无线遥控很少会去使用现代通用的安全技术，<wbr />仅仅依靠协议的不透明而不是协议的固有安全来确保系统的安全。

随着软件无线电平台的出现，<wbr />越来越多的安全分析团队将有机会对现有的无线安全系统中的薄弱环<wbr />节进行全面、长期的安全评估。

</div>
<div>
<h3>有趣的项目</h3>
以下项目的介绍及演示都可以在<a href="http://hackrf.net" target="_blank">http://hackrf.<wbr />net</a>上找到。
<ul>
	<li><a href="http://www.hackrf.net/2014/04/hackrf-replay-big-gate/" target="_blank">使用HackRF对大铁门/道闸进行重放测试</a></li>
	<li><a href="http://www.hackrf.net/2014/05/%e5%ae%8c%e6%95%b420mhz%e5%b8%a6%e5%ae%bdlte%e4%bf%a1%e5%8f%b7%e8%a2%abhackrf-20m%e9%87%87%e6%a0%b7%e7%8e%87%e6%88%90%e5%8a%9f%e8%a7%a3%e6%9e%90/" target="_blank">使用HackRF解析TDD LTE信号</a></li>
	<li><a href="http://www.hackrf.net/2014/01/wbfm%e5%8f%91%e5%b0%84/" target="_blank">使用HackRF发射FM广播</a></li>
	<li><a href="http://www.hackrf.net/2014/03/hackrf-dab-%e5%b9%bf%e6%92%ad%e5%8f%91%e5%b0%84-hackrf-dab-transmit/" target="_blank">使用HackRF发射DAB数字音频广播</a></li>
	<li><a href="http://www.hackrf.net/2014/03/%e7%94%a8hackrf%e5%92%8cgnuradio%e6%9d%a5%e5%ae%9e%e7%8e%b0%e5%af%b9%e9%81%a5%e6%8e%a7%e5%b0%8f%e8%bd%a6%e7%9a%84%e6%8e%a7%e5%88%b6/" target="_blank">使用HackRF对遥控小车的控制信号进行分析重建</a></li>
	<li><a href="http://www.hackrf.net/2014/05/hackrf-wireless-mic/" target="_blank">使用HackRF对会议室无线麦克风进行监听及插入重放</a></li>
	<li>使用HackRF监听业余无线电通联</li>
	<li><a href="http://www.hackrf.net/2014/03/hackrf-one-trunking-radio/" target="_blank">使用HackRF与Node.js解调数字集群对讲机</a></li>
	<li><a href="http://www.hackrf.net/2014/05/%e4%bd%bf%e7%94%a8hackrf%e5%b7%a1%e8%a7%86%e9%92%93%e9%b1%bc%e5%b2%9b%ef%bc%88ads-b-out%e5%ae%9e%e9%aa%8c%ef%bc%89/" target="_blank">使用HackRF分析模拟ADS-B飞航雷达信号</a></li>
	<li><a href="http://www.hackrf.net/2014/05/warfare/" target="_blank">使用HackRF进行电子战模拟训练</a></li>
</ul>
</div>
<div>
<h2>目前，HackRF在国内外的发展情况如何？<wbr />两者之间有什么不同？</h2>
HackRF最初是由Michael Ossmann在Kickstarter上以短短35天的时间预<wbr />计出去2000多块，<wbr />以其价格优势和支持GNURadio的特点，<wbr />获得了全球范围内的很大的关注。

但是由于生产控制的原因，已经连续跳票达半年之久。<wbr />于是国内的北京星天无限科技有限公司 利用多年的射频设计控制经验，迅速地在国内实现供货，<wbr />目前已经在淘宝上公开发售。<wbr />并对每块出厂的板卡使用安捷伦公司的通信专业级仪表进行了射频指<wbr />标检定。 保障了板卡的射频指标的稳定。

与此同时，星天无限一直在支持HackRF.<wbr />net软件无线电社区的发展，<wbr />为社区的线下活动提供活动经费支持，并为有志于为HackRF.<wbr />net社区提交教程、代码贡献的同学提供开源折扣。

从 今年3月起，<wbr />HackRF已经或将要在多个开源会议上作为主题演讲，<wbr />以提升大家对软件无线电概念、<wbr />GNURadio及HackRF的了解:
<ul>
	<li>HFD2014 硬件自由日清华站 @清华</li>
	<li>OSTC 开源技术大会 @CSDN</li>
	<li>Ubuntu Kylin 14.04 发布会 中国科学院大学站</li>
	<li>Ubuntu Kylin 14.04 发布会 中南大学 国防科大站 @长沙</li>
	<li>GNOME Asia 2014 &amp; FUDCON 2014 @北航</li>
	<li>阿里集团无线创新大会 @杭州</li>
	<li>COSCUP 2014 台湾开源人年会 @台北</li>
</ul>
由于HackRF在国内到货的速度超过了国外， 为国内的开发者、爱好者提供了宝贵的时间优势，<wbr />目前国内同学们提供的教程、Slide、<wbr />代码已经在Kickstarter、Twitter、<wbr />邮件列表里受 到国外的开发者的关注，<wbr />为提升国内开源社区的形象起到了一定的提升作用。

此外，HackRF.net开源软件无线电社区提供的入门指南、<wbr />操作教程、<wbr />通信原理介绍降低了大家在Linux环境上部署使用GNURad<wbr />io以及HackRF环境的门槛，<wbr />从客观上也促进了Linux在通信研究领域的使用。

</div>
<div>
<h2>对本次开源夏令营的学生开发成果有何期待？</h2>
希望本次开源夏令营的同学们，能够在这一次的活动中，<wbr />首先是自己能够学到新的知识，<wbr />对开源项目的整个生命周期有一个深刻的认识。

同时，本次HackRF.<wbr />net软件无线电社区提交的六个题目涵盖了应用普及、系统移植、<wbr />信号处理、无线安全等几个方面。<wbr />我们期望在同学们的努力工作和导师们的帮助之下，<wbr />这些项目能够使得开源软件无线电的应用被进一步地丰富，<wbr />降低用户的上手门槛。

最重要的一点，我认为对于一个开源项目来说，<wbr />最大的荣誉是有Pull Request，也就不只是作者在对项目进行维护。<wbr />要想达成这一目标，需要同学们在项目代码的可维护性、<wbr />项目的中英文文档的书写、项目的教程及宣传文章等 方面下一些功夫。我们希望这次做出的项目，能够长久地“活”<wbr />下去，不断地有新的同学加入到这些项目中来。

让学通信的同学发挥通信特长，<wbr />让Linux用户们发挥系统部署的特长，<wbr />让安全圈的大黑阔们发挥安全分析的特长，<wbr />最终实现互通有无共同进步的目标。

</div>
