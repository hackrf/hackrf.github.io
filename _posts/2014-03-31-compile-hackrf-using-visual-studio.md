---
ID: 523
title: "Windows下使用Visual Studio 2012 编译 HackRF"
author: scateu
date: 2014-03-31 17:43:31
post_excerpt: ""
layout: post
published: true
views:
  - "3921"
duoshuo_thread_id:
  - "1312073613704167487"
---
<strong>注: </strong>部分所需下载资源已经<a href=" http://download.csdn.net/user/u014466216">上传到CSDN</a>
<h2>编译所需的库下载</h2>
<ol>
	<li>下载安装Visual Studio 2010以上的版本，这里用2012</li>
	<li>下载安装<a href="http://www.cmake.org/cmake/resources/software.html">CMake </a></li>
	<li>下载 pthread库</li>
	<li>下载 libusb1.0库</li>
	<li>下载 HackRF: <a href="https://github.com/mossmann/hackrf" target="_blank">https://github.com/mossmann/hackrf</a></li>
</ol>
<h2> 编译</h2>
<ol>
	<li>解压hackrf</li>
	<li>在host目录下新建build文件夹</li>
	<li>进入build目录新建include和libs文件夹</li>
	<li>将pthread libusb1.0库的头文件libusb.h sched.h semaphore.h  pthread.h 复制到 include 目录下</li>
	<li>将libusb-1.0.lib pthreadVSE2.lib 复制到libs目录下</li>
</ol>
<h2> 使用CMake生成Visual Studio的工程文件</h2>
CMake除了生成GNU Makefile之外，还可以生成Visual Studio的工程。

运行cmd，到build目录下并根据Visual Studio不同版本执行下面相应命令
<div>用Visual Studio 2010编译：</div>
<div>
<pre class="lang:default decode:true">cmake ../ -G "Visual Studio 10" -DLIBUSB_INCLUDE_DIR=include -DLIBUSB_LIBRARIES=../../libs/libusb-1.0  -DTHREADS_PTHREADS_INCLUDE_DIR=include -DTHREADS_PTHREADS_WIN32_LIBRARY=libs/pthreadVSE2.lib</pre>
</div>
<div>用Visual Studio 2012编译：</div>
<div>
<pre class="lang:default decode:true">cmake ../ -G "Visual Studio 11" -DLIBUSB_INCLUDE_DIR=include -DLIBUSB_LIBRARIES=../../libs/libusb-1.0  -DTHREADS_PTHREADS_INCLUDE_DIR=include -DTHREADS_PTHREADS_WIN32_LIBRARY=libs/pthreadVSE2.lib</pre>
</div>
<div>用Visual Studio 2013编译：</div>
<div>
<pre class="lang:default decode:true ">cmake ../ -G "Visual Studio 12" -DLIBUSB_INCLUDE_DIR=include -DLIBUSB_LIBRARIES=../../libs/libusb-1.0  -   DTHREADS_PTHREADS_INCLUDE_DIR=include -DTHREADS_PTHREADS_WIN32_LIBRARY=libs/pthreadVSE2.lib</pre>
执行结果如下:
<pre class="lang:default decode:true ">-- The C compiler identification is MSVC 17.0.50727.1
-- The CXX compiler identification is MSVC 17.0.50727.1
-- Check for working C compiler using: Visual Studio 11
-- Check for working C compiler using: Visual Studio 11 -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler using: Visual Studio 11
-- Check for working CXX compiler using: Visual Studio 11 -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Found Threads: C:/Users/user/Desktop/hackrf-master/host/build/libs/pthreadVSE2.lib  
-- Udev rules not being installed, install them with -DINSTALL_UDEV_RULES=ON
-- Configuring done
-- Generating done
-- Build files have been written to: C:/Users/user/Desktop/hackrf-master/host/build</pre>
<h2> 编译中出现的问题</h2>
用Visual Studio 2012编译出现以下错误
<pre class="lang:default decode:true">3&gt;  libgetopt_static.vcxproj -&gt; C:\Users\user\Desktop\hackrf-master\host\build\hackrf-tools\src\Release\libgetopt_static.lib
4&gt;..\..\..\libhackrf\src\hackrf.c(660): error C2143: 语法错误 : 缺少“;”(在“const”的前面)
4&gt;..\..\..\libhackrf\src\hackrf.c(661): error C2143: 语法错误 : 缺少“;”(在“类型”的前面)
4&gt;..\..\..\libhackrf\src\hackrf.c(662): error C2143: 语法错误 : 缺少“;”(在“类型”的前面)
4&gt;..\..\..\libhackrf\src\hackrf.c(663): error C2065: “i”: 未声明的标识符
4&gt;..\..\..\libhackrf\src\hackrf.c(663): warning C4018: “&lt;”: 有符号/无符号不匹配
4&gt;..\..\..\libhackrf\src\hackrf.c(663): error C2065: “chunk_size”: 未声明的标识符
4&gt;..\..\..\libhackrf\src\hackrf.c(668): error C2065: “i”: 未声明的标识符
4&gt;..\..\..\libhackrf\src\hackrf.c(669): error C2065: “chunk_size”: 未声明的标识符
4&gt;..\..\..\libhackrf\src\hackrf.c(670): error C2065: “transferred”: 未声明的标识符
4&gt;..\..\..\libhackrf\src\hackrf.c(828): warning C4028: 形参 3 与声明不同
2&gt;..\..\..\libhackrf\src\hackrf.c(660): error C2143: 语法错误 : 缺少“;”(在“const”的前面)
2&gt;..\..\..\libhackrf\src\hackrf.c(661): error C2143: 语法错误 : 缺少“;”(在“类型”的前面)
2&gt;..\..\..\libhackrf\src\hackrf.c(662): error C2143: 语法错误 : 缺少“;”(在“类型”的前面)
2&gt;..\..\..\libhackrf\src\hackrf.c(663): error C2065: “i”: 未声明的标识符
2&gt;..\..\..\libhackrf\src\hackrf.c(663): warning C4018: “&lt;”: 有符号/无符号不匹配
2&gt;..\..\..\libhackrf\src\hackrf.c(663): error C2065: “chunk_size”: 未声明的标识符
2&gt;..\..\..\libhackrf\src\hackrf.c(668): error C2065: “i”: 未声明的标识符
2&gt;..\..\..\libhackrf\src\hackrf.c(669): error C2065: “chunk_size”: 未声明的标识符
2&gt;..\..\..\libhackrf\src\hackrf.c(670): error C2065: “transferred”: 未声明的标识符
2&gt;..\..\..\libhackrf\src\hackrf.c(828): warning C4028: 形参 3 与声明不同</pre>
<h4>解决办法</h4>
将hackrf.c文件下的
<pre class="nums:true start-line:600 lang:default decode:true ">const unsigned int chunk_size = 512;
unsigned int i;
int transferred = 0;</pre>
由于C++和C代码关于定义位置的区别，将这3句代码(第600行开始)剪切到所在函数开始的变量定义位置。
<pre class="nums:true start-line:642 lang:default decode:true ">int ADDCALL hackrf_cpld_write(hackrf_device* device,
unsigned char* const data, const unsigned int total_length)
{
const unsigned int chunk_size = 512;
unsigned int i;
int transferred = 0;
int result = libusb_release_interface(device-&gt;usb_device, 0);
...
}</pre>
然后编译通过。
<h2>使用SDRSharp测试</h2>
<ol>
	<li>在\hackrf-master\host\build\libhackrf\src\Release 下找到hackrf.dll</li>
	<li>在pthread库中找到 pthreadVSE2.dll</li>
	<li>下载解压<a href="http://sdrsharp.com/downloads/sdr-nightly.zip">SDRSharp</a></li>
	<li>复制hackrf.dll和 pthreadVSE2.dll 到SDRSharp 目录下</li>
	<li>将hackrf.dll重命名为并替换原有的libhackrf.dll</li>
	<li>启动SDRSharp即可</li>
</ol>
&nbsp;
<h2>Visual Studio 2008的Solution例子</h2>
链接：http://pan.baidu.com/s/1mgqPpK8 密码：uhci

</div>
