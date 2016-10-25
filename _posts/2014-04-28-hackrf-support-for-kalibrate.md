---
ID: 769
title: "为kalibrate添加HackRF支持"
author: scateu
date: 2014-04-28 04:17:48
post_excerpt: ""
layout: post
published: true
views:
  - "5408"
duoshuo_thread_id:
  - "1312073613704167510"
---
HackRF.net的scateu同学日前熬了一个周末为kalibrate增加了HackRF的支持。

kalibrate程序能够使用GSM的FCCH信息来进行频率校准，并且可以扫描GSM频点。<!--more-->

<a href="https://github.com/scateu/kalibrate-hackrf" target="_blank">https://github.com/scateu/<wbr />kalibrate-hackrf</a>
<pre>$ ./kal -s GSM900                                                                                                           
hackrf_init()
hackrf_open()
hackrf_set_sample_rate()
hackrf: set gain
kal: Scanning for GSM-900 base stations.
GSM-900:
        chan: 18 (938.6MHz + 336Hz)     power: 1341750.80
        chan: 23 (939.6MHz + 317Hz)     power: 1571924.69
        chan: 28 (940.6MHz + 317Hz)     power: 1396134.44
        chan: 29 (940.8MHz + 348Hz)     power: 1362095.72
        chan: 33 (941.6MHz + 405Hz)     power: 555542.24
        chan: 108 (956.6MHz + 367Hz)    power: 1534510.98
        chan: 113 (957.6MHz + 371Hz)    power: 1771700.00
        chan: 114 (957.8MHz + 374Hz)    power: 1666143.15
        chan: 118 (958.6MHz + 371Hz)    power: 1602046.14
</pre>
<h2> 编译安装</h2>
<pre>./bootstrap
./configure
make
sudo make install</pre>
