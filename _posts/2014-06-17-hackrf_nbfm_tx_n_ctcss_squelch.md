---
ID: 924
title: "HackRF/GNURadio发射手台/对讲机的NBFM(窄带调频)信号并进行亚音频静噪实验"
author: scateu
date: 2014-06-17 01:15:37
post_excerpt: ""
layout: post
published: true
views:
  - "11084"
duoshuo_thread_id:
  - "1312073613704167526"
---
在大家已经知道了WBFM(宽带调频)也就是普通收音机广播的原理(<a href="http://www.hackrf.net/2014/01/wbfm%E5%8F%91%E5%B0%84/">教程</a>)，并且也学会使用gnuradio-companion搭建<a title="gnuradio-companion搭载hackrf one制作FM收音机带频谱仪(一)" href="http://www.hackrf.net/2014/03/gnuradio-companion%e6%90%ad%e8%bd%bdhackrf-one%e5%88%b6%e4%bd%9cfm%e6%94%b6%e9%9f%b3%e6%9c%ba%e5%b8%a6%e9%a2%91%e8%b0%b1%e4%bb%aa%e4%b8%80/">WBFM接收框图</a>之后，这一次，我们来玩一玩对讲机/手台所使用的NBFM(窄带调频)。<!--more-->

&nbsp;

NBFM和WBFM最大的区别是Max Deviation(最大频偏)不同。
<ul>
	<li>NBFM Max Deviation = 5kHz</li>
	<li>WBFM Max Deviation = 75kHz</li>
</ul>
直接反映到频谱上，WBFM的频谱比NBFM要宽，同时NBFM(对讲机)的声音带宽会被进一步压低，而WBFM(普通广播)的音质就会听起来好得多。

&nbsp;

闲言少叙，直接上框图。在WBFM的框图基础上，其它的部分不需要改动，只把WBFM Transmit模块换成NBFM Transmit即可。

![]({{ site.imageurl }}/2014/06/nbfm_tx.png)
以上框图把jen_ai_marre.wav文件(双声道, 16bit量化, 44.1kHz采样率)以NBFM方式发射到441.175MHz上。此时拿起一只对讲机，调谐到上述频点，便可以听到音乐了。   <a href="https://github.com/scateu/HackRF_Examples/blob/master/nbfm_tx/nbfm_tx_hackrf.grc">grc框图下载</a>

![]({{ site.imageurl }}/2014/06/audio_nbfm.png)

顺便提一句，实测发现，以NBFM方式发射的覆盖范围比WBFM更远，想一想便能理解，窄带宽可以使得功率更加密集，发射效率更高。

&nbsp;
<h2>亚音频</h2>
经常玩手台/对讲机的同学们可能会听说过“亚音频”这个概念。大家在使用对讲机的时候，经常会出现同一个频点有多组人在同时讲话的情况。同一个通话组的人，可以使用同一个亚音频，然后在手台上开启亚音频静噪控制(CTCSS)，然后就可以有效地将没有开启亚音频的对讲机信号屏蔽掉了。

来自百科的解释如下，供大家参考理解:

<em>CTCSS (Continuous Tone Controlled Squelch System)，连续语音控制静噪系统，俗称亚音频，是一种将低于音频频率的频率（67Hz-250.3Hz）附加在音频信号中一起传输的技术。因其频率范围在标准音频以下，故称为亚音频。当对讲机对接收信号进行中频解调后，亚音频信号经过滤波、整形，输入到CPU中，与本机设定的CTCSS频率进行比较，从而决定是否开启静音。</em>

&nbsp;

于是搭起框图，由于我们的音频文件的采样率是44.1kHz，而左右声道合在一起之后采样率便是88.2kHz，所以新加进来的正弦波发生模块的采样率需要设置为88.2e3。使用Add模块与原有的声音采样流加到一起。

&nbsp;

然后拿起手台设置好亚音模式(在这里设置为67Hz)后测试，果然在不加亚音的时候，手台不会响，只会提示有信号，加上67Hz亚音后，亚音静噪被打开，可以听到声音了。

![]({{ site.imageurl }}/2014/06/nbfm_tx_ctcss.png)

<a href="https://github.com/scateu/HackRF_Examples/blob/master/nbfm_tx/nbfm_tx_hackrf_ctcss.grc">grc框图下载</a>
