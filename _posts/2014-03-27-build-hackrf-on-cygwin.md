---
ID: 466
title: "在Cygwin下编译HackRF"
author: scateu
date: 2014-03-27 00:45:18
post_excerpt: ""
layout: post
published: true
views:
  - "3295"
duoshuo_thread_id:
  - "1312073613704167485"
---
由于HackRF One之前一直使用着旧的Jawbreaker的USB VendorID(1d50:604b)

2014年3月13日左右，作者对HackRF One启用了新的VendorID(1d50:6089)

见host/libhackrf/src/hackrf.c
的<a href="https://github.com/mossmann/hackrf/blob/master/host/libhackrf/src/hackrf.c#L119">119行</a>和第<a href="https://github.com/mossmann/hackrf/blob/master/host/libhackrf/src/hackrf.c#L276">276行</a>，程序先寻找1d50:6089再找1d50:604b

Windows下的编译参考：<a href="http://www.hackrf.net/2014/03/compile-hackrf-host-on-windows-and-linux/">http://www.hackrf.net/2014/03/compile-hackrf-host-on-windows-and-linux/</a>
<h2>安装Cygwin</h2>
<ul>
	<li>去<a href="http://cygwin.com">cygwin.com</a>上下载对应32位或64位系统的setup.exe</li>
	<li>执行setup程序，选择国内163的源</li>
	<li>下一步勾选几个软件包
<ul>
	<li><span style="line-height: 1.5em;">libusb1.0-dev</span></li>
	<li><span style="line-height: 1.5em;">cmake</span></li>
	<li><span style="line-height: 1.5em;">g++ </span></li>
	<li><span style="line-height: 1.5em;">gcc </span></li>
	<li><span style="line-height: 1.5em;">make</span></li>
</ul>
</li>
</ul>
<h2>编译</h2>
<pre class="lang:sh decode:true">cd hackrf/host/build
cmake ../ -G "Unix Makefiles" -DCMAKE_LEGACY_CYGWIN_WIN32=1 -DLIBUSB_INCLUDE_DIR=/usr/include/libusb-1.0
make
make install</pre>
就可以了。

需要注意的一点是，
<pre>-DLIBUSB_INCLUDE_DIR=/usr/include/libusb-1.0</pre>
这里指定了libusb的路径
<h2>下载</h2>
<a href="http://pan.baidu.com/s/1bnrKeMb">这里</a>是我编译好的hackrf_info等工具，32位Windows7上编译通过。

解压之后执行hackrf_info，会有如下结果
<pre class="lang:default decode:true">Found HackRF board.
Board ID Number: 2 (HackRF One)
Firmware Version: git-3f59f4b
Part ID Number: 0xbc5f4f4a 0xbc5f4f4a
Serial Number: 0x00000000 0x00000000 0x261c63c8 0x26776d53</pre>
&nbsp;
<h2>进一步</h2>
下一步需要把SDRSharp里用到的HackRF库改成最新版本的，应该需要重新编译SDRSharp的HackRF插件，甚至做相应的修改。

参见
<ul>
	<li><a href="https://github.com/zefie/sdrsharp_hackrf/blob/master/src/HackRF/HackRFDevice.cs">https://github.com/zefie/sdrsharp_hackrf/blob/master/src/HackRF/HackRFDevice.cs</a></li>
	<li><a href="https://github.com/zefie/sdrsharp_hackrf/">https://github.com/zefie/sdrsharp_hackrf/</a><a href="https://github.com/zefie/sdrsharp_hackrf/blob/master/src/HackRF/HackRFDevice.cs">
</a></li>
</ul>
&nbsp;
