---
ID: 541
title: "HackRF AIS接收实例"
author: scateu
date: 2014-04-12 11:51:49
post_excerpt: ""
layout: post
published: true
views:
  - "2934"
duoshuo_thread_id:
  - "1312073613704167497"
---
<strong>背景</strong>
船舶自动识别系统（Automatic Identification System, 简称AIS系统）由岸基（基站）设施和船载设备共同组成，是一种新型的集网络技术、现代通讯技术、计算机技术、电子信息显示技术为一体的数字助航系统和设备。
船舶自动识别系统（AIS）由舰船飞机之敌我识别器发展而成，配合全球定位系统（GPS）将船位、船速、改变航向率及航向等船舶动态结合船名、呼号、吃水及危险货物等船舶静态资料由甚高频（VHF）频道向附近水域船舶及岸台广播，使邻近船舶及岸台能及时掌握附近海面所有船舶之动静态资讯，得以立刻互相通话协调，采取必要避让行动，对船舶安全有很大帮助。

<strong>安装</strong>
<pre class="lang:default decode:true">git clone https://github.com/bistromath/gr-ais.git
cd gr-ais
mkdir build
cd build
cmake ../
make
sudo make install
sudo ldconfig</pre>
<strong>使用</strong>
<pre class="lang:default decode:true">ais_rx -s osmocom -g &lt;增益&gt; -e &lt;ppm误差&gt; -r &lt;采样率&gt;</pre>
<strong>参考</strong>
<ul>
	<li><a href="http://www.cruisersforum.com/forums/f134/fast-cheap-ais-using-20-rtl-sdr-dvb-t-sdr-dongles-84234.html">http://www.cruisersforum.com/forums/f134/fast-cheap-ais-using-20-rtl-sdr-dvb-t-sdr-dongles-84234.html</a></li>
	<li><a href="http://www.rtl-sdr.com/wp-content/uploads/2013/05/gr-ais_in_SuSE12.3.en_.v.1.pdf">http://www.rtl-sdr.com/wp-content/uploads/2013/05/gr-ais_in_SuSE12.3.en_.v.1.pdf</a></li>
	<li>opencpn</li>
</ul>
&nbsp;
