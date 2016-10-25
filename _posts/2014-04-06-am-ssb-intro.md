---
ID: 580
title: "关于调幅/单边带调制的一点讨论"
author: scateu
date: 2014-04-06 14:11:59
post_excerpt: ""
layout: post
published: true
views:
  - "2146"
duoshuo_thread_id:
  - "1312073613704167491"
---
考虑到一个信号$f(t)$

为了进行<strong>调幅</strong>调制，调制到中心频率为$f_c(t)$的载波上，我们需要将$f(t)$与$f_c(t)$相乘。

调制后的信号为:

$g(t)= f(t)\times f_c(t) $

<img class="alignnone" alt="" src="http://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Amfm3-en-de.gif/250px-Amfm3-en-de.gif" width="250" height="195" />
<h2>例子</h2>
下面，我们假设被调制信号(有效信息)$f(t) = \cos(f_1 t)$，是一个单一频率的声音。

载波中心频率为$f_c$，即$f_c(t)=\cos(f_c t)$为例:

则调制后的信号为:

$g(t) = f(t)\times f_c(t) = \cos(f_1 t) \times \cos(f_c t) $

根据积化和差公式:

$\cos(\alpha)\cos(\beta) = [\cos(\alpha + \beta) + \cos(\alpha - \beta)]/2 $

则有

$g(t) = [\cos(f_1 t + f_c t) + \cos(f_1 t - f_c t)]/2 = [\cos(f_1 + f_c)t + \cos(f_1 - f_c)t]/2$

那么我们会发现，我们所要传输的有效信息，被分成上下两个边带。而每个边带里的内容都是一样的。这样在我们传输信号的时候，浪费了发射功率。

<img class="alignnone" alt="" src="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/Am-sidebands.png/240px-Am-sidebands.png" width="240" height="181" />

于是Wikipedia上的说法我们就可以理解了:
<blockquote><a title="幅度调制" href="http://zh.wikipedia.org/wiki/%E5%B9%85%E5%BA%A6%E8%B0%83%E5%88%B6">调幅</a>技术输出的调制信号带宽为源信号的两倍。单边带调制技术可以避免带宽翻倍，同时避免将能量浪费在载波上，不过因为设备变得复杂，成本也会增加。</blockquote>
为了减少这种浪费，单边带调制技术出现了，它把其中一个边带滤掉，称为上边带(USB)或下边带(LSB)调制。
<h2>参考</h2>
[1] <a href="http://en.wikipedia.org/wiki/Amplitude_modulation">http://en.wikipedia.org/wiki/Amplitude_modulation</a>

[2] <a href="http://en.wikipedia.org/wiki/Single-sideband_modulation">http://en.wikipedia.org/wiki/Single-sideband_modulation</a>
