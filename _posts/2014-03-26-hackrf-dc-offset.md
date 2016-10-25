---
ID: 445
title: "HackRF关于DC Offset直流偏置"
author: scateu
date: 2014-03-26 17:33:49
post_excerpt: ""
layout: post
published: true
views:
  - "3630"
duoshuo_thread_id:
  - "1312073613704167484"
---
<h2 id="我接收到的频谱中间那一个大的尖峰是啥译自wiki">我接收到的频谱中间那一个大的尖峰是啥？</h2>
<h2>(译自<a href="https://github.com/mossmann/hackrf/wiki/FAQ#what-is-this-big-spike-in-the-center-of-my-received-spectrum">wiki</a>)</h2>
<div id="wmd-preview-section-1366">
<h3 id="q">Q:</h3>
我发现我的HackRF接收的时候，不管调谐到什么频率上，在FFT显示的正中央都会一个大的尖峰。是我的HackRF挂了么？

</div>
<div id="wmd-preview-section-4177">
<h3 id="a">A:</h3>
你看到的是直流偏置(DC Offset)，或者称直流分量。直流这个用语是来自于电学中的“直流电流”。指的是信号中的不变的部分，信号中另一部分会随时间改变，这部分称为交流分量(AC)。 例如，考察下面所示，表示一个信号的数字序列:

</div>
<div id="wmd-preview-section-6358">
<pre><code>-2, -1, 1, 6, 8, 9, 8, 6, 1, -1, -2, -1, 1, 6, 8, 9, 8, 6, 1, -1, -2, -1, 1, 6, 8, 9, 8, 6, 1, -1</code></pre>
上述周期信号包含了一个较强的正弦组分，跨度从-2到9。如果你把这个信号的频谱绘制出来,你会看到这个正弦波的频率上有一个峰(这是正常的)并且 在0 Hz(直流)上有第二个峰.如果信号的值从-2到2的话(以0为中心)，就没有直流偏置了。由于上述信号是以3.5为中心的(数字由-2到9), 这里面就存在了直流分量.

HackRF产生的采样点是对无线波形的测量。但是测量方法容易导致HackRF引入的直流偏置。这是测量系统中的一个人为因素(It’s an artifact of the measurement system),不是接收到的无线信号的表征. 不只是HackRF有直流偏置的问题; 所有的正交采样系统都有这个问题。

在2013.06.1版本的固件里有一个Bug，导致了DC offset变得更糟糕。在最差的情形下，某些板卡遇到了在几秒种的工作之后直流偏置到了一个很大的drift.这个bug后来被修复了。这个修复减小了 直流偏置，但是没有完全消除它。当用到如HackRF这样的正交采样系统的时候，你是需要与直流偏置打交道的。

</div>
<div id="wmd-preview-section-6419">
<h2 id="我应该怎么处理直流偏置dc-offset-译自wiki">我应该怎么处理直流偏置(DC Offset)</h2>
<h2>(译自<a href="https://github.com/mossmann/hackrf/wiki/FAQ#how-do-i-deal-with-the-dc-offset">wiki</a>)</h2>
</div>
<div id="wmd-preview-section-1630">
<h3 id="q-1">Q:</h3>
好吧，现在我理解我的接收频谱正中间的那个大的尖峰是啥了，我怎么处理它呢？

</div>
<div id="wmd-preview-section-3591">
<h3 id="a-1">A:</h3>
有几个选择:
<ol>
	<li>忽略它. 对于很多应用来说，它不是个大问题，你会学会忽略它的。</li>
	<li>避免它. 处理直流偏置最好的办法就是调谐的时候偏离中心频率一点; 调谐到与你感兴趣的信号邻近的一个频率上，这样你就可以把中心0Hz的频率躲过去了。如果你的算法在0频附近工作最好的话（很多算法都是这样），你可以在 数字域中移频，把你感兴趣的频率移动到0Hz，并把直流偏置移动到远离0Hz的位置。HackRF的最大采样率比较大，因此即使遇到了相对宽带的信号，用 这种偏离中心频率调谐的方法也是可以解决的。</li>
	<li>修正它. 有几种不同的方法来在软件上移除直流偏置。然而，这些技术可能会把靠近0Hz的信号给降低一些。这会看起来更好，但是例如从解调算法上来 说，这并不一定意味着更好。 当然，修正直流偏置也总是一个好的选择。</li>
</ol>
译者注: GNURadio Companion里面有DC Blocker滤波器。另外，在gqrx和SDR#等软件里也提供了DC Removal等选项。

</div>
