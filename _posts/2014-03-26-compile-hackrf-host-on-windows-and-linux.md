---
ID: 440
title: "Windows和Linux环境下编译libhackrf和相关utils"
author: scateu
date: 2014-03-26 12:48:04
post_excerpt: ""
layout: post
published: true
views:
  - "4133"
duoshuo_thread_id:
  - "1312073613704167482"
---
文章翻译自hackrf/host/README.md
<div id="wmd-preview-section-31">
<h2 id="如何在windows环境下编译">如何在Windows环境下编译</h2>
</div>
<div id="wmd-preview-section-32">
<h3 id="cygwin-or-mingw的依赖库">cygwin or mingw的依赖库</h3>
<ul>
	<li>cmake-2.8.12.1 见 <a href="http://www.cmake.org/cmake/resources/software.html">http://www.cmake.org/cmake/resources/software.html</a></li>
	<li>libusbx-1.0.18 见 <a href="http://sourceforge.net/projects/libusbx/files/latest/download?source=files">http://sourceforge.net/projects/libusbx/files/latest/download?source=files</a></li>
	<li>安装HackRF的Windows驱动，使用Zadig 参见 <a href="http://sourceforge.net/projects/libwdi/files/zadig">http://sourceforge.net/projects/libwdi/files/zadig</a>
<ul>
	<li>使用Zadig 时，选中HackRF USB device，并使用WinUSB驱动来安装/替换即可。</li>
</ul>
</li>
</ul>
Windows编译注意:
<em>hackrf-tools 只能在Windows的cmd里执行，</em><em>不要从Cygwin 或Mingw shell里执行，因为Cygwin/Mingw里的Ctrl+C的效果不正确，特别是对hackrf_transfer使用Ctrl+C时，不会正确关掉，从而损坏录制的IQ数据。</em>

</div>
<div id="wmd-preview-section-33">
<h3 id="对于cygwin">对于Cygwin:</h3>
<pre><code>cd host
mkdir build
cd build
cmake ../ -G "Unix Makefiles" -DCMAKE_LEGACY_CYGWIN_WIN32=1 -DLIBUSB_INCLUDE_DIR=/usr/local/include/libusb-1.0/
make
make install
</code></pre>
</div>
<div id="wmd-preview-section-34">
<h3 id="对于mingw">对于MinGW:</h3>
<pre><code>cd host
mkdir build
cd build
</code></pre>
正常的版本:
<pre><code>cmake ../ -G "MSYS Makefiles" -DLIBUSB_INCLUDE_DIR=/usr/local/include/libusb-1.0/
</code></pre>
Debug 版本:
<pre><code>cmake ../ -G "MSYS Makefiles" -DCMAKE_BUILD_TYPE=Debug -DLIBUSB_INCLUDE_DIR=/usr/local/include/libusb-1.0/
</code></pre>
最后:
<pre><code>make
make install
</code></pre>
</div>
<div id="wmd-preview-section-35">
<h2 id="在linux下编译host程序">在Linux下编译host程序</h2>
</div>
<div id="wmd-preview-section-36">
<h3 id="linux-debianubuntu的依赖">Linux (Debian/Ubuntu)的依赖</h3>
<pre><code>sudo apt-get install build-essential cmake libusb-1.0-0-dev
</code></pre>
</div>
<div id="wmd-preview-section-37">
<h3 id="在-linux下编译">在 Linux下编译</h3>
<pre><code>cd host
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo ldconfig
</code></pre>
</div>
<div id="wmd-preview-section-38">
<h2 id="清理cmake-临时文件文件夹">清理CMake 临时文件/文件夹</h2>
<pre><code>cd host/build
rm -rf *
</code></pre>
</div>
