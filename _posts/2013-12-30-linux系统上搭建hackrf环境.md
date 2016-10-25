---
ID: 74
title: "Linux系统上搭建HackRF环境"
author: scateu
date: 2013-12-30 18:23:04
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167435"
views:
  - "25518"
---
<strong><span style="color: #ff0000;">注意</span>:</strong> <strong>不要</strong>使用软件源里的GNURadio，因为软件源里的GNURadio太久没有重新打包。
<h2>方法一:Ubuntu 14.04版本上搭建环境方法：</h2>
<!--more-->

此种环境搭建的方法，用的是已经经过编译的程序，因而不用翻墙，并且速度较快。

首先需要安装Ubuntu 14.04，之后你只需要在Ubuntu 14.04下，输入以下命令即可：
<pre class="lang:default decode:true">sudo add-apt-repository ppa:gqrx/releases
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gqrx gnuradio gr-osmosdr hackrf</pre>
如果安装时出现 'Held packages' 类型的报错，那么应该是由于之前已经进行过安装，这时，输入以下命令即可：
<pre class="lang:default decode:true">sudo apt-get dist-upgrade</pre>
<h2>方法二: 手动编译</h2>
由于build-gnuradio脚本经常受到不可抗力的影响，导致安装失败。于是需要我们进行手动编译。

手动编译的顺序是
<ol>
	<li>安装各种依赖包</li>
	<li>gnuradio</li>
	<li>hackrf / rtlsdr</li>
	<li>gr-osmosdr</li>
</ol>
<h4>安装依赖包</h4>
<div>
<pre class="wrap:true lang:default decode:true ">sudo apt-get -y install build-essential cmake git-core autoconf automake  libtool g++ python-dev swig pkg-config libfftw3-dev libboost1.53-all-dev libcppunit-dev libgsl0-dev libusb-dev sdcc libsdl1.2-dev python-wxgtk2.8 python-numpy python-cheetah python-lxml doxygen python-qt4 python-qwt5-qt4 libxi-dev libqt4-opengl-dev libqwt5-qt4-dev libfontconfig1-dev libxrender-dev libusb-1.0</pre>
<h4>编译GNURadio</h4>
<pre class="lang:default decode:true">git clone --progress http://gnuradio.org/git/gnuradio.git
cd gnuradio
mkdir build
cd build
cmake ../
make -j4 #4代表用4核编译
sudo make install
sudo ldconfig</pre>
<h4>编译hackrf</h4>
<pre class="lang:default decode:true">git clone --progress http://github.com/mossmann/hackrf.git
cd hackrf/host
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo ldconfig</pre>
<h4>编译rtlsdr(可选)</h4>
<pre class="lang:default decode:true ">git clone --progress git://git.osmocom.org/rtl-sdr 
cd rtl-sdr
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON
sudo make install
sudo ldconfig</pre>
<h4>编译gr-osmosdr</h4>
<pre class="lang:default decode:true">git clone --progress git://git.osmocom.org/gr-osmosdr
cd gr-osmocom
mkdir build
cd build
cmake ../
make 
sudo make install
sudo ldconfig</pre>
<h4>编译gqrx(可选)</h4>
<pre class="lang:default decode:true ">git clone https://github.com/csete/gqrx.git
cd gqrx
mkdir build
cd build
qmake ../gqrx.pro
make
sudo make install
sudo ldconfig</pre>
&nbsp;

</div>
<h2>编译完成后</h2>
你可以尝试以下命令
<ul>
	<li>osmocom_fft : 一个简单的HackRF频谱仪</li>
	<li>osmocom_siggen : 一个简单的HackRF信号源</li>
	<li><a href="http://gqrx.dk/">gqrx</a> : 类似于SDR#的广播接收器</li>
</ul>
&nbsp;
