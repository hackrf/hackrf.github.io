---
ID: 348
title: " HackRF One 解调数字对讲机"
author: scateu
date: 2014-03-18 12:39:49
post_excerpt: ""
layout: post
published: true
views:
  - "10031"
duoshuo_thread_id:
  - "1312073613704167472"
---
<a href="http://openmhz.com">OpenMHz.com </a>给我们演示了如何使用HackRF One来解调数字对讲机。

数字对讲机的制式是Motorola SmartNet II，作者将HackRF One 放置于华盛顿特区，监听火警/紧急情况/城市服务等无线电服务，并使用Node.js实现了一个WEB前端。

以下为我们的翻译，原文见<a href="http://openmhz.com/about">http://openmhz.com/about</a>

&nbsp;
<div id="about-carousel-callout">
<h3>监听华盛顿特区的火警/紧急情况/城市服务</h3>
</div>
<h1>无线电通信</h1>
本系统监听华盛顿特区的火警/紧急情况/城市服务的无线电系统。 华府警察使用了一个有语音加密信道的独立系统。全市的通话组有时被用在系统之间进行通信。

无线电系统使用一组固定的信道来发送和接受通信。常规的无线电系统通过使用不同的频率实现各个部门独立地通信，也就是说火警和邮政服务一定会使用不同的信道。这种无线电系统是摩托罗拉SmartNet II 集群系统，集群系统比传统的无线电系统更加先进。当他们想要互相通信时，每个通话组会在他们共享的一组信道中被集群系统临时分配到一个信道而不是为每个通话组永久保留一个固定的信道。这就意味着只用很少的几个信道，许多不同的通话组便可以独立地进行通信。

不同的人群有他们各自的通话组，有些甚至可能有几个通话组，这样他们就可以分别进行不同类型的通信。系统中每个数字都代表一个通话组Radio Reference可以搞清楚谁在用哪一个谈话组做什么。
<h2>一些有趣的通话组</h2>
<div>
<div>
<h4>DCFD 01 Dispatch</h4>
这是火灾和紧急事件的通话组。

</div>
<div>
<h4>DCFD 02 Main</h4>
协调消防作业的主要通话组。

</div>
</div>
<h2>用语</h2>
系统中有几个惯例。火警和紧急事件通话组中，作战单位是用他们的类别和数字来呼叫的。火警中, 单位类似一般是Engine或Truck, 也有一些特殊的单位，如Foam(泡沫) 或Tower truck(塔车). 紧急情况中，单位似乎一般用Medic来指代。

&nbsp;
<h1>技术细节</h1>
网站虽然简单，但是后台有不少东西可讲。 在这里我非常大概地讲一讲它是如何工作的，但是你如果需要更进一步的细节，欢迎给我邮件。

无线电信号使用了 HackRF 软件无线电平台进行接收。软件无线电接收一段宽带的无线电频谱并将它送到电脑上进行解码。用这种方法，我们就有可能接收所有数字对讲机发射出来的信号，并连续地解调它们。如果没有软件无线电平台，每个单独的数字对讲机频道都需要用一块单独的无线电接收机。

在一个集群系统里, 会保留一个无线电频道，用于管理通话组无线信道的分配。当某人想要说话, 他们先向控制频道发出消息。这个系统然后就给他们指派一个频道，然后在控制信道中发送一个"频道授予消息"。这就使得通话者知道应该使用哪个频道进行发射，并且这个通话组里的所有人就知道应该监听这个频道。

为了监听所有的发射, 本系统将一直监听并解码控制信道。当一个信道被授予某个通话组，本系统就创建一个监听进程。这个进程就会开始处理并解码与这个信道有关的这部分的无线电频谱。事实上，HackRF也一直在接收着所有的的频谱。

当通话组的会话结束之后，控制信道上并没有消息被发出。所以，监听进程将一直监听这部分频谱，当连续5秒没有活动的时候，它就会停止录制并将它上传到Web服务器。

监听和录制过程是用我公寓里的笔记本完成的，使用了一个不怎么样的天线。网站部署在 magical cloud 里我的VPS.

Web服务器很简单。用Node.js写的。音频存成WAV文件，并用MongoDB进行索引。服务器监控着目录里一旦有新文件，就把它们移走，并添加到MongoDB里。并且如果有新的无线电语音被加入，网站就会用Socket.io来更新所有当前打开的浏览器客户端。
<h2> 参考</h2>
<ul>
	<li><a href="https://github.com/robotastic/smartnet-recorder">https://github.com/robotastic/smartnet-recorder</a></li>
	<li><a href="https://github.com/robotastic/smartnet-player">https://github.com/robotastic/smartnet-player</a></li>
	<li><a href="http://lukeberndt.com/2013/using-a-hackrf-to-capture-an-entire-radio-system/">http://lukeberndt.com/2013/using-a-hackrf-to-capture-an-entire-radio-system/</a></li>
	<li><a href="https://github.com/robotastic/smartnet-scanner">https://github.com/robotastic/smartnet-scanner</a></li>
</ul>
&nbsp;
