---
ID: 446
title: "HackRF 在不同操作系统里的编译安装要点"
author: scateu
date: 2014-03-26 13:28:57
post_excerpt: ""
layout: post
published: true
views:
  - "4316"
duoshuo_thread_id:
  - "1312073613704167483"
---
&nbsp;
<div id="preview">

原文见<a href="https://github.com/mossmann/hackrf/wiki/Operating-System-Tips">https://github.com/mossmann/hackrf/wiki/Operating-System-Tips</a>

以下提供一些特定操作系统的编译/安装的注意的要点。
<h2>Windows下编译</h2>
见<a href="http://www.hackrf.net/2014/03/compile-hackrf-host-on-windows-and-linux/">http://www.hackrf.net/2014/03/compile-hackrf-host-on-windows-and-linux/</a>
<h2 id="ubuntu-13-04">Ubuntu 13.04</h2>
<ol>
	<li>依赖
<pre><code>sudo apt-get -y install build-essential cmake git-core autoconf automake  libtool g++ python-dev swig \
pkg-config libfftw3-dev libboost1.53-all-dev libcppunit-dev libgsl0-dev \
libusb-dev sdcc libsdl1.2-dev python-wxgtk2.8 python-numpy \
python-cheetah python-lxml doxygen python-qt4 python-qwt5-qt4 libxi-dev \
libqt4-opengl-dev libqwt5-qt4-dev libfontconfig1-dev libxrender-dev ia32-libs</code></pre>
<pre><code>sudo add-apt-repository ppa:terry.guo/gcc-arm-embedded
sudo apt-get update &amp;&amp; sudo apt-get install gcc-arm-none-eabi</code></pre>
</li>
	<li><code>libhackrf</code>
<pre><code>git clone git://github.com/mossmann/hackrf.git
cd hackrf/host
mkdir build &amp;&amp; cd build
cmake ..
make
sudo make install</code></pre>
</li>
	<li>GNU Radio (注意这里会安装GNU Radio 3.7.x 版本, 意味着旧的流程图会由于程序内部命名的问题，无法使用)
<pre><code>git clone git://git.gnuradio.org/gnuradio.git
cd gnuradio
mkdir build &amp;&amp; cd build
cmake ../
make -j4 # example build command to speed up compilation process when 4 cores are available
sudo make install
sudo ldconfig</code></pre>
</li>
	<li><code>gr-osmosdr</code>
<pre><code>git clone git://git.osmocom.org/gr-osmosdr
cd gr-osmosdr
mkdir build &amp;&amp; cd build
cmake ../
make
sudo make install</code></pre>
</li>
</ol>
<h2 id="os-x-10-5-with-macports">OS X (10.5+) with MacPorts</h2>
<pre><code>sudo port install gr-osmosdr +full</code></pre>
更进一步的使用homebrew来在OS X上编译HackRF的信息，请<a href="https://github.com/robotastic/homebrew-hackrf">参考链接</a>
<h2 id="gentoo-linux">Gentoo Linux</h2>
<pre><code>emerge hackrf-tools
USE="hackrf" emerge gr-osmosdr</code></pre>
<h2 id="-linux-">其它Linux发行版</h2>
使用 <a href="http://gnuradio.org/redmine/projects/gnuradio/wiki/InstallingGR#Using-the-build-gnuradio-script">build-gnuradio</a> 脚本来自动从源代码安装/编译 libhackrf, gr-osmosdr, and GNU Radio.

</div>
