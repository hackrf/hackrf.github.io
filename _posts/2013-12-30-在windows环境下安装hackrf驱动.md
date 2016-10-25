---
ID: 61
title: "在Windows环境下安装HackRF驱动"
author: scateu
date: 2013-12-30 10:25:00
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167434"
views:
  - "11910"
---
<a href="http://superfro.org/setting-up-hackrf-in-windows-with-sdr/">原始文章</a>

由于现在SDR#最新的nightly-build已经集成了对HackRF的支持，所以不需要再单独配置SDR#

一共只需要两步：zadig+SDR#
<h2>USB驱动</h2>
下载<a href="http://sourceforge.net/projects/libwdi/files/zadig/">Zadig</a>

用7zip解压之后，执行Zadig即可

<img src="http://superfro.org/wp-content/uploads/2013/07/zadig-300x136.jpg" alt="zadig" />

注意选择你的HackRF设备
<h2>SDRSharp</h2>
下载<a href="http://sdrsharp.com/downloads/sdr-nightly.zip">SDR#</a>

<del><strong>如果提示找不到HackRF设备</strong></del>
<ul>
	<li><del>由于SDR#没有跟进最新版本的HackRF固件所使用的新USB Vendor ID</del></li>
	<li><del>暂时可以使用我们的<a href="http://pan.baidu.com/s/1gdutNdp">修复</a></del></li>
	<li><del>解压之后三个文件放到SDRSharp目录里即可。</del></li>
</ul>
解压即可使用，可以收听广播了。注意在Configure里把AMP打开，增益选到合适的位置。

解调方式选WFM，设备选HackRF，在Configure里可以选择采样率，如果你的电脑不足够快，建议先选8Mbps的试一把。

<img src="http://sdrsharp.com/downloads/sdrsharp.png" alt="enter image description here" />

有可能需要安装 <a href="http://www.microsoft.com/en-us/download/details.aspx?id=30679">VS2012 Redistributable</a>
<h2>SDR-Radio</h2>
<div>SDR-Radio是一个在无线电爱好者范围内广受欢迎的SDR程序，在其作者订购的两块HackRF终于到货之后，他在几天之内迅速地加入了对HackRF的支持。</div>
<div>从这一点来说，HackRF的API还是比较易用的。</div>
<div>欢迎大家尝试。</div>
<a href="http://v2.sdr-radio.com/Download.aspx">http://v2.sdr-radio.com/Download.aspx</a>

&nbsp;
<h2>hackrf-tools for windows</h2>
在Cygwin下编译过程参阅：<a href="http://www.hackrf.net/2014/03/build-hackrf-on-cygwin/">http://www.hackrf.net/2014/03/build-hackrf-on-cygwin/</a>

<a href="http://the.midnightchannel.net/sdr/SDRSharp_Plugins/zefie/SDRSharp.HackRF.ZefieMod/hackrf-tools.zip">hackrf-tools for windows</a>

这个不一定需要安装,这个是hackrf-tools在windows下预编译好的版本，包含
<pre>hackrf_info      
hackrf_si5351c 
hackrf_transfer
hackrf_cpldjtag
hackrf_max2837 
hackrf_spiflash
hackrf_fm      
hackrf_rffc5071
hackrf_tcp</pre>
