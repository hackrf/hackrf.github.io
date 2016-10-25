---
permalink: /analysis-of-hardware/
ID: 23
title: "硬件分析"
author: scateu
date: 2013-12-29 01:21:53
post_excerpt: ""
layout: page
published: true
duoshuo_thread_id:
  - "1312073613704167430"
views:
  - "25442"
---
<strong>功耗测定见<a title="HackRF One功耗测定" href="http://www.hackrf.net/2014/03/hackrf-one-power-consumption/">链接</a></strong>
<h3>HackRF 的硬件原理</h3>
![]({{ site.imageurl }}/2013/12/HackRF_架构.png)

硬件主要由以下几部分组成
<ul>
	<li>RFFC5072: 混频器提供80MHz到4200MHz的本振</li>
	<li>MAX2837: 2.3GHz to 2.7GHz 无线宽带射频收发器</li>
	<li>MAX5864: ADC/DAC, 22MHz采样率 8bit</li>
	<li>LPC4320/4330: ARM Cortex M4处理器, 主频204MHz</li>
	<li>Si5351B: I2C可编程任意CMOS时钟生成器，由800MHz分频提供40MHz 50MHz 及采样时钟</li>
	<li>MGA-81563: 0.1–6GHz 3V, 14 dBm 放大器</li>
	<li>SKY13317: 20 MHz-6.0 GHz 射频单刀三掷(SP3T)开关</li>
	<li>SKY13350: 0.01-6.0 GHz 射频单刀双掷(SPDT)开关</li>
</ul>
以接收过程为例，信号由天线进入后流程如下
<ul>
	<li>由射频开关决定是否经由14dB的放大器进行放大</li>
	<li>经过镜像抑制滤波器对信号进行高通或低通滤波</li>
	<li>信号进行RFFC5072芯片混频到2.6GHz固定中频
<ul>
	<li>最新的固件支持可变中频的选项</li>
	<li>中频范围2.150GHz - 2.750GHz</li>
</ul>
</li>
	<li>信号送入MAX2837芯片混频到基带，输出差分的IQ信号
<ul>
	<li>其间MAX2837芯片可以对信号进行带宽限制</li>
</ul>
</li>
	<li>MAX5864芯片对基带信号进行数字化后送入CPLD和单片机 TODO FIXME</li>
	<li>CPLD</li>
	<li>LPC4320/4330处理器将采样数据通过USB送至计算机</li>
</ul>
<h3>HackRF One针对Jawbreaker做了哪些改进</h3>
<ul>
	<li>删除了板载废柴微带天线</li>
	<li>将RFFC5072和MAX2837放入屏蔽罩内保护起来，防止外界及板上其它芯片的干扰，并试图防止静电击穿部分芯片</li>
	<li>重新布局，使得射频连线更紧凑</li>
</ul>
实际测试过程中，我们发现Jawbreaker中由于布线问题造成的全频谱范围内1MHz为周期出现的小信号干扰，在HackRF One中完全消除。

<em>注意</em> 测试版本Jawbreaker已经不被最新版的固件所支持。
<h2>Datasheet</h2>
<ul>
	<li><a href="http://www.maxim-ic.com/datasheet/index.mvp/id/5452/t/al">MAX2837 2.3 to 2.7 GHz transceiver</a>
<ul>
	<li><a href="http://datasheets.maxim-ic.com/en/ds/MAX2837.pdf">Datasheet</a></li>
	<li>There's also a register map document that Mike received directly from Maxim. Send an email to Mike or submit a support request to Maxim if you want a copy.</li>
</ul>
</li>
	<li><a href="http://www.maxim-ic.com/datasheet/index.mvp/id/3946/t/do">MAX5864 ADC/DAC</a>
<ul>
	<li><a href="http://datasheets.maxim-ic.com/en/ds/MAX5864.pdf">Datasheet</a></li>
</ul>
</li>
	<li><a href="http://www.silabs.com/products/clocksoscillators/clock-generator/Pages/lvcmos-clocks-5-outputs.aspx">Si5351 clock generator</a>
<ul>
	<li><a href="http://www.silabs.com/Support%20Documents/TechnicalDocs/AN619.pdf">AN619: Manually Generating an Si5351 Register Map</a></li>
	<li><a href="http://www.silabs.com/Support%20Documents/TechnicalDocs/Si5351.pdf">Datasheet</a> - this document is a mess of typos, and best used in conjunction with AN619, which has its own typos. Usually, you can reconcile what's true by comparison and a bit of thought.</li>
	<li><a href="http://www.silabs.com/products/clocksoscillators/clock-generators-and-buffers/Pages/clock+vcxo.aspx">Other Documentation</a> - includes application notes, user guides, and white papers.</li>
</ul>
</li>
	<li>CoolRunner-II CPLD (considering switch to MAX V)</li>
	<li><a href="http://www.nxp.com/products/microcontrollers/cortex_m4/lpc4300/">LPC43xx ARM Cortex-M4 microcontroller</a>
<ul>
	<li><a href="http://www.nxp.com/documents/user_manual/UM10503.pdf">User Manual</a></li>
	<li><a href="http://www.nxp.com/documents/data_sheet/LPC4350_30_20_10.pdf">Datasheet</a></li>
	<li><a href="http://www.nxp.com/products/microcontrollers/cortex_m4/lpc4300/LPC4330FBD144.html#documentation">Other Documentation (LPC4330FBD144)</a> - includes errata and application notes.</li>
	<li><a href="http://www.keil.com/support/man/docs/ulink2/ulink2_hw_connectors.htm">ARM-standard JTAG/SWD connector pinout</a></li>
</ul>
</li>
	<li><a href="https://estore.rfmd.com/RFMD_Onlinestore/Products/RFMD+Parts/PID-P_RFFC5072.aspx">RFFC5072 mixer/synthesizer</a>
<ul>
	<li><a href="http://www.rfmd.com/CS/Documents/RFFC5071_2DS.pdf">Datasheet</a></li>
	<li><a href="https://estore.rfmd.com/RFMD_Onlinestore/Products/RFMD+Parts/PID-P_RFFC5071.aspx">Other Documentation</a> - includes programming guides and application notes.</li>
</ul>
</li>
</ul>
