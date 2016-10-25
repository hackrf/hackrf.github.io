---
ID: 743
title: "在Mac上安装HackRF环境"
author: dovecho
date: 2014-04-18 21:36:07
post_excerpt: ""
layout: post
published: true
views:
  - "7510"
duoshuo_thread_id:
  - "1312073613704167508"
---
<h2><strong>我的Mac可以支持HackRF么？</strong></h2>
Mac OS X，之所以成为Mac，全靠底层的Unix系统。因此，她有与Linux非常类似的操作过程，却又有其独特的地方。

系统的相似性使得Linux下面的代码可以直接在Mac下使用（只是要重新编译），因此在Mac里面搭建全套的HackRF环境是完全可能的，而且是非常方便的。<!--more-->
<h2>编程环境搭建</h2>
Mac下面的编程环境包括如下程序，需要按顺序安装
<ul>
	<li>Xcode (从AppStore安装：<a href="https://itunes.apple.com/cn/app/xcode/id497799835?mt=12">https://itunes.apple.com/cn/app/xcode/id497799835?mt=12</a>)</li>
	<li>XQuartz/X11 (<a href="http://xquartz.macosforge.org/landing/">http://xquartz.macosforge.org/landing/</a>)</li>
	<li>MacPorts (<a href="https://trac.macports.org/wiki/InstallingMacPorts">https://trac.macports.org/wiki/InstallingMacPorts</a>)</li>
</ul>
<h4><span style="color: #ff0000;">1. 安装XCode：</span></h4>
直接从AppStore安装就好了~
<h4 style="color: #ff2600;">2. 安装XQuartz/X11：</h4>
XQuartz是一个在Mac OS X下支持X窗口系统的开源软件，许多开源程序都是依靠XQuartz实现图形界面的。安装方法也很简单，下载链接中的dmg文件，然后双击打开即可安装。
<h4 style="color: #ff2600;">3. 安装MacPorts</h4>
MacPorts的安装可以参考
<ol>
	<li>Macports 网站指南：<a href="http://www.macports.org/install.php">http://www.macports.org/install.php</a></li>
	<li>开源中国社区的指南（中文）：<a href="http://www.oschina.net/question/129318_17613">http://www.oschina.net/question/129318_17613</a></li>
</ol>
推荐的安装方法是：下载dmg或pkg包文件，然后按照提示安装。如果需要自行编译或采用其他安装方法，可以参考MacPorts的网站（英文）。

安装好后，打开”实用工具“里的”终端“，然后键入如下命令来确保MacPorts是最新的（此命令也可不定期运行）：

有同学说“这个已经不是在mac的终端里面运行了，需要在XQuartz里面运行”
<pre>sudo port -v selfupdate</pre>
<h2>软件无线电环境搭建</h2>
安装HackRF最重要的是软件无线电环境的搭建，需要用到的程序包括：
<p style="padding-left: 30px;">gnuradio、hackrf、rtl-sdr（可选）、gr-osmosdr、gqrx（可选）</p>
 与Linux下不太相同，采用MacPorts安装，可自动下载相关的依赖程序，而不需输入长长的依赖包。可相应的运行下列脚本，安装所需程序：
<pre>sudo port install gnuradio
sudo port install hackrf
sudo port install rtl-sdr
sudo port install gr-osmosdr
sudo port install gqrx</pre>
安装之后，可定期运行下列脚本，查看哪些安装程序已经过时：
<pre>sudo port outdated</pre>
然后可以用下列脚本来升级程序
<pre>sudo port upgrade outdated</pre>
如果需要卸载某个程序，可以用如下脚本
<pre>sudo port uninstall NAME</pre>
<h2>关于GNURadio的安装问题</h2>
有爱好者表示，在Mac OS X 10.9上安装gnuradio不能成功，此问题在<a href="http://gnuradio.4.n7.nabble.com/Status-of-GNU-Radio-with-OSX-10-9-td44553.html">这里</a>有很多讨论，有建议称可以安装gnuradio的开发版以解决安装失败的问题：
<pre> sudo port install gnuradio-devel</pre>
不过要有心理准备，全新安装耗时非常久，如果网络不够快的话，建议将这一安装放在半夜进行，一觉醒来，可能万事大吉了~
