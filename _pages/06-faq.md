---
ID: 219
permalink: /faq/
title: "FAQ"
author: scateu
date: 2014-03-13 13:01:23
post_excerpt: ""
layout: page
published: true
duoshuo_thread_id:
  - "1312073613704167459"
views:
  - "13734"
---
<h2>能用虚拟机吗？</h2>
Windows不能。Linux和Mac下据说是可以的。
<h2 id="hackrf-one的两个按钮管啥用">HackRF One的两个按钮管啥用？</h2>
外面的那个是Reset，上电之后，需要按一下Reset开机。
里面的那个是DFU按钮，按住DFU按钮开机，会进入HackRF的刷机模式，也可以理解成为”修砖”模式，只有在你的HackRF被你刷成砖的时候才会用到，平时不需要用。
<div id="wmd-preview-section-1354">
<h2 id="hackrf-one的三个sma头分别管啥用">HackRF One的三个SMA头分别管啥用？</h2>
单独的那个SMA头旁边标注了Antenna，是天线接口。
两个靠在一起的SMA头，一个是CLKOUT，提供10MHz时钟输出，用于多个HackRF之间的时间同步。
另一个是CLKIN，可以接受10MHz时钟的输入，也是用于时钟同步。
关于时钟同步的细节可以参阅<a href="http://www.hackrf.net/2014/03/hackrf-one-10mhz-clkout/">这篇文章</a>。
<h2>如何在Linux下面搭建全套开发环境?</h2>
有两种方案，详细情况参见<a title="Linux系统上搭建HackRF环境" href="http://www.hackrf.net/2013/12/linux%e7%b3%bb%e7%bb%9f%e4%b8%8a%e6%90%ad%e5%bb%bahackrf%e7%8e%af%e5%a2%83/">Linux下搭建开发环境</a>一文
<ul>
	<li>build-gnuradio一键安装脚本</li>
	<li>手动编译的顺序是
<ul>
	<li>安装各种依赖包</li>
	<li>gnuradio</li>
	<li>hackrf / rtlsdr</li>
	<li>gr-osmosdr</li>
</ul>
</li>
</ul>
</div>
<h2>hackrf_open() failed: HACKRF_ERROR_NOT_FOUND (-5)</h2>
这是由于没有安装udev rules，有两种解决办法。
<ul>
	<li>用"管理员权限"执行: sudo hackrf_info</li>
	<li>编译libhackrf的时候用
<pre class="lang:default decode:true">cmake ../ -DINSTALL_UDEV_RULES=ON</pre>
</li>
</ul>
<h2>HackRF</h2>
天线插到单独的那个天线口上

剩下两个"天线口"分别是CLK_IN和CLK_OUT，用于时钟同步

直接录制40MHz的信号
<pre><code>hackrf_transfer -r car.iq -f 40000000 -s 8000000 -i 60
</code></pre>
回放上述信号
<pre><code>hackrf_transfer -t car.iq -f 40000000 -s 8000000 -a 1 -l 30 -x 40
</code></pre>
听广播 - <a href="http://gqrx.dk">gqrx</a>

Windows听广播 - 先用<a href="http://sourceforge.net/projects/libwdi/files/zadig/">Zadig</a>装驱动，再装<a href="http://sdrsharp.com/downloads/sdr-nightly.zip">SDR# </a>

频谱仪
<pre><code>osmocom_fft -f 103.9M -c 0
</code></pre>
信号源
<pre><code>osmocom_siggen 
</code></pre>
<h2>GNURadio</h2>
不建议用软件源里的GNURadio，建议手动编译.

gnuradio.org推荐使用的无痛一键安装脚本
<pre><code>$ wget http://www.sbrac.org/files/build-gnuradio
$ chmod +x build-gnuradio
$ ./build-gnuradio -v -ja
</code></pre>
一般的Build流程
<pre><code>mkdir build
cd build
cmake ../
make
sudo make install
sudo ldconfig
</code></pre>
Build的先后顺序:
<ul>
	<li>gnuradio</li>
	<li>hackrf / rtlsdr</li>
	<li>gr-osmosdr</li>
</ul>
其它GNURadio有趣的示例位于<code>/usr/local/share/gnuradio/examples/</code>
<h2>Others</h2>
用HackRF监控华盛顿地区的公共无线电通信: <a href="http://openmhz.com">openMHz.com</a>

听DTMF
<pre><code>aoss multimon -a dtmf
</code></pre>
练Morse码
<pre><code>xcwcp
</code></pre>
两台电脑间用FSK传输数据
<pre><code>minimodem -t 100 
minimodem 100
</code></pre>
gMFSK 多频键控
<pre><code>padsp gmfsk 
aoss gmfsk
</code></pre>
github.com/scateu/gr-remotecar

看飞机ADS-B: dump1090

看卫星位置: gPredict

NOAA图像解码: wxtoimg
