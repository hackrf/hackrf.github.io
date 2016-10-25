---
ID: 342
title: "gnuradio-companion搭载hackrf one制作FM收音机带频谱仪(二)"
author: paud
date: 2014-03-17 22:17:43
post_excerpt: ""
layout: post
published: true
views:
  - "5813"
duoshuo_thread_id:
  - "1312073613704167471"
---
接着上一讲，我们这次来实现一个带调节器的FM收音机，同时理解一下radio frequency, intermidea frequency, baseband frequecy等的含义，其实很多我也不懂，请懂的大牛来给我们科普一下通信方面的知识，谢谢了！

首先增加5个 slider，关于找零件，上一节已经说过了，按下图所示进行设置，这次我们要引入变量概念。variable,程序员熟悉的。

![]({{ site.imageurl }}/2014/03/2014-03-17-214154的屏幕截图.png)

由于时间关系，以下摘自网络：

<strong>双击第一个WX GUI Slider</strong>

将ID改为freq
<pre>默认值（Default Value）为上面的您喜欢听的调频广播电台的频率（比如我喜欢听的105.2，我这里的默认值就是105.2e6）
最小值（Minimum）为76e6
最大值（Maximum）为108e6</pre>
<strong>双击第二个WX GUI Slider</strong>，将ID改为rf_gain；默认值为10；最大值为30。
<strong>双击第三个WX GUI Slider</strong>，将ID改为if_gain；默认值为20；最大值为30。
<strong>双击第四个WX GUI Slider</strong>，将ID改为bb_gain；默认值为20；最大值为30。
<strong>双击第五个WX GUI Slider</strong>，将ID改为sample_rate；默认值为2.048e6；最小值为1.6e6；最大值为3.2e6。

需要注意的是，这里我们增加的WX GUI Slider的ID可以作为变量名使用，可以让各个功能模块直接调用，所以直接将ID写在恰当的地方就可以了，但显示出来的是变量的值而不是变量名：

<strong>双击上面我们已经正在使用的RTL-SDR Source</strong>
<pre>将采样率从2.048e6改为sample_rate
将Ch0: Frequently(Hz)从105.2e6改为freq
将Ch0: RF Gain(dB)改为rf_gain
将Ch0: IF Gain(dB)改为if_gain
将Ch0: BB Gain(dB)改为bb_gain</pre>
<strong>双击上面我们已经正在使用的WX GUI FFT Sink</strong>
<pre>将采样率从2.048e6改为sample_rate
将Baseband Freq改为freq</pre>
在无线通讯系统中，根据频率，可以分成射频、中频和基带信号。射频重要用于信号在空间的传输，基带信号是基站等数字设备可以处理的信号，中频是从射频变化到基带信号的过渡频率。以前的系统一般是从射频直接变到基带。现在的新的系统是 射频-&gt;中频Intermediate Frequency-&gt;基带 称为两次变频。

射频：&gt; 500M Hz

中频：50MHz ~ 500MHz

基带：&lt; 50MHz

![]({{ site.imageurl }}/2014/03/2014-03-17-214018的屏幕截图.png)

这样我们就可以随时更换频率调台了，还可以随时调整射频中频以及基带频率，让播放更清晰。<span style="line-height: 1.5em">
</span>

你会发现你调频率的时候，声音会跟着变粗或变细，因为采样之后，wbfm也需要更新，将变量引入wbfm接收器，声音就不会再改变了。

未完待续……

to be continue…

下一节本菜教大家实现《窃听风云》中一些有趣的幻想。

本文版权归本人QQ:35769382和hackrf.net所有，转载请注明出处，本讲到此为止，谢谢大家，再会！
