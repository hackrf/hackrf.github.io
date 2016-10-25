---
ID: 935
title: "用MATLAB+TCP+HACKRF来“看”TD-LTE信号"
author: jxj
date: 2014-06-21 18:26:25
post_excerpt: ""
layout: post
published: true
views:
  - "10194"
duoshuo_thread_id:
  - "1312073613704167527"
---
(关于一些基本的软件无线电概念，Linux操作，代码库git操作，以及软件包安装（apt-get）等，参见：<a href="http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B(tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes)/">这篇文章</a>)

本文介绍如何用<a href="http://sdr-x.github.io/Matlab%E7%9C%8B%E5%AE%9E%E6%97%B6TD-LTE%E4%BF%A1%E5%8F%B7(Matlab%E5%B0%8F%E4%BC%99%E4%BC%B4%E5%8F%AF%E5%A4%A7%E5%B1%95%E8%BA%AB%E6%89%8B%E4%BA%86)(Matlab%20TCP%20interface%20to%20HACKRF%20rtl-sdr)/">MATLAB通过TCP接口实时读取HACKRF接收到的信号</a>。其实并不复杂，可以说相当简单。<!--more-->

首先用gnuradio-companion建立如下grc：

![]({{ site.imageurl }}/2014/06/0.png)

这里osmocom Source源模块（用来接收HACKRF usb传来的数据）设置为：

采样率2M，频率1890MHz（中国移动TD-LTE频率，你也可以设置为你所在位置的其他信号频率），RF增益14dB，中频增益20dB，基带增益40dB（增益需要根据你所在地点的信号情况进行合理设置），带宽1M（只看中心频率带宽1M内的东西，比如PBCH，CRS之类的，也可以根据你的需求修改设置）。

TCP Sink模块（用来接收源模块数据，并通过TCP转发）设置为：

地址：127.0.0.1 （即本机）

端口：5000 （可改）

模式：Server （不要选Client）

接口数据类型应该选complex，因为源模块现在只能输出complex数据，I和Q均为float32数据，交替发送。

然后运行这个grc，此时如果一切正常，应该看到不断的输出OOOOOO，并且代表正在运行的那个小窗口也不会出现，因为现在没人连到这个TCP Server取数据，数据都还憋着，因此Overrun。

接下来打开Matlab，输入如下代码：
<pre class="lang:matlab decode:true">tcp_obj = tcpip('127.0.0.1', 5000, 'ByteOrder', 'LittleEndian', 'InputBufferSize', 2e6*2*4, 'Timeout', 2);
% 建立tcp对象，ip地址和端口要和grc里的一致，在我的计算机上必须设置为小端模式才能正确解析数据，最后两个参数是缓冲区大小和超时门限

fopen(tcp_obj);
% 和TCP Server建立tcp连接

% 进入无限循环实时显示HACKRF收到的信号
while(true)
  [x, count] = fread(tcp_obj, 0.02e6*2, 'float32'); % 读取10ms长度的信号。采样率2e6，因此0.02e6代表10ms，正好LTE一帧
  if count &lt; 0.02e6*2 %如果未能成功读取我们希望的长度，报错
    disp('Not enough samples returned!');
  else %如果成功读取，则计算每个样点的能量，并显示在时间轴上
    plot(x(1:2:end).^2 + x(2:2:end).^2);
    drawnow;
  end
end</pre>
运行以上代码，如果一切正常，代表Gnuradio正在运行的小窗口应该立刻出现，OOOO应该停止打印，而且MATLAB会弹出实时的LTE信号在时域上的能量包络。根据显示，可以判断出TD-LTE上下行配置，PBCH位置，以及每个slot中两个包含CRS的ofdm符号的位置，因为HACKRF时钟和基站一般存在微小误差，因此看到的时域信号应该是在向左或者向右滑动的，取决于你的HACKRF时钟偏快还是偏慢。抓取其中一次的显示如下图：

![]({{ site.imageurl }}/2014/06/1.png)

上图总共是10ms TD-LTE无线帧的时域能量分布。大约横轴0.1～0.8，1.1～1.8的地方为基站下行信号，其他时间是留给上行的时间，从上下行配比来看应该是典型的TD-LTE上下行配置2。0.6～0.8之间应该是PBCH和SSS、PSS，靠近1.8的地方应该是间隔5ms的另外一对SSS、PSS。从大约0.1开始每相邻的两个突起应该是1个时隙内包含CRS的两个OFDM符号的位置，即相邻四个突起为1ms子帧。

至此，用MATLAB通过TCP方法来实时读取HACKRF接收信号的方法初步验证完毕。

以上的方法其实通用性很强，不少其他开源SDR工具其实也都提供了TCP或者UDP转发功能，比如rtl_tcp，dump1090等，均可以通过TCP或UDP与MATLAB接口。

关于UDP，其实不太好用，因为它包长最长只有1472字节，连续实时接收大量信号时，因为UDP的分包操作，很难保证可靠性。还是首选TCP。

注意：你可能会遇到Gnuradio或者Matlab报告地址已被占用，或者拒绝连接的错误，这是你可能需要关掉Gnuradio或者MATLAB重来。以上Matlab代码中只是打开了TCP对象并无限使用，没有关闭它，所以你再次运行时极有可能遇到拒绝连接或者已被占用的错误。可以在Matlab程序被你终止后手工运行如下Matlab代码来关闭TCP连接清除对象（也可以在Matlab代码一开始做这些清理工作），避免重起：
<pre class="lang:matlab decode:true">if exist('tcp_obj','var')
  fclose(tcp_obj);
  delete(tcp_obj);
  clear tcp_obj;
end</pre>
&nbsp;

&nbsp;
