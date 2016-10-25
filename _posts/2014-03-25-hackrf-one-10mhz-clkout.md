---
ID: 434
title: "HackRF One 10MHz时钟输出"
author: scateu
date: 2014-03-25 15:43:17
post_excerpt: ""
layout: post
published: true
views:
  - "8876"
duoshuo_thread_id:
  - "1312073613704167481"
---
最新版本的HackRF固件中(2014年3月15日更新的版本)

HackRF One 一共有3个SMA接头。
单独的那个SMA头是天线(ANTENNA)。
在板子的另一侧的两个SMA接头，一个是时钟输入(CLKIN)，一个是时钟输出(CLKOUT)。

我们使用示波器接到CLKOUT上，可以看到输出的10MHz时钟同步信号如下图。

![]({{ site.imageurl }}/2014/03/10MHz.jpeg)

&nbsp;

这个信号是由Si5351B时钟芯片给出的，有了这个信号，我们可以对多个HackRF One进行同步，从而完成高级的数字通信应用，如MIMO等。
<h2>CLKOUT接到频谱仪的REF IN</h2>
在没有接10MHz同步的时候，有频偏
![]({{ site.imageurl }}/2014/03/INT_REF.jpg)

接了10MHz同步之后：
![]({{ site.imageurl }}/2014/03/EXT_REF.jpg)

参考时钟芯片Si5351C官方编程手册<a href="http://www.silabs.com/Support%20Documents/TechnicalDocs/AN619.pdf">AN619</a>可知：Si5351C的时钟源可选择外部石英晶体或CMOS电平的外部时钟信号。

当使用外部CMOS时钟驱动Si5351C时，须将PLLx_SRC位须置1。当外部时钟频率大于40Mhz时，必须使用CLKIN输入分频器将外部时钟频率降至10-40Mhz内。

通过配置Register 15的CLKIN_DIV[1:0]位可将外部时钟分频系数设定为1，2，4或8。

相关的代码修改见以下两次提交:

<a href="https://github.com/mossmann/hackrf/commit/ca04d7c04ba42750de16e990caf5dedb6fab770e">https://github.com/mossmann/hackrf/commit/ca04d7c04ba42750de16e990caf5dedb6fab770e</a>

<a href="https://github.com/mossmann/hackrf/commit/15bda174c74dd35938cda41545519b409bb538ab">https://github.com/mossmann/hackrf/commit/15bda174c74dd35938cda41545519b409bb538ab</a>
