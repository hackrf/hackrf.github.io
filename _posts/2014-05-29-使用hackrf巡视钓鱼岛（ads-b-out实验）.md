---
ID: 888
title: "使用HackRF巡视钓鱼岛（ADS-B out实验）"
author: jxj
date: 2014-05-29 12:13:04
post_excerpt: ""
layout: post
published: true
views:
  - "7513"
duoshuo_thread_id:
  - "1312073613704167522"
---
好吧，承认我是标题党。其实就是<a href="http://sdr-x.github.io/%E4%BD%BF%E7%94%A8HACKRF%E5%B7%A1%E8%A7%86%E9%92%93%E9%B1%BC%E5%B2%9B(HACKRF%20ADS-B%20out)/">用HACKRF发射ADS-B信号</a>，因为技术含量偏低，只好标题博眼球了。。。

(关于一些基本的软件无线电概念，Linux操作，代码库git操作，以及软件包安装（apt-get）等，参见：<a href="http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B(tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes)/">这篇文章</a>)

做了这么个简单的实验（见附件图和视频： <a title="http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html" href="http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html">http://v.youku.com/v_show/id_XNzE4NzIzNDIw.html</a>）。

![]({{ site.imageurl }}/2014/05/hackrf-adsb-rtl-sdr-dump10901.png)

实现的功能是：

matlab按照ads-b协议生成信号，因为自己生成可以填入任意经纬度等信息，经HACKRF和天线发射，然后由rtl-sdr电视棒配合dump1090或者gr-air-modes接收。

生成信号的坐标序列是按照钓鱼岛周围一圈设置的，所以你在ADS-B接收界面中看到的就是一架飞机在以3000ft的高度巡视钓鱼岛。。。


警告：

不要真的用1090MHz实验！如果实验，你该找一个合法的或者足够安全的频率（比如某些敌国卫星的生癖频率），以最最最低的功率发射，最好在地下室或者暗室玩这个，而且要用闭路的射频电缆，不要开路玩。

出了事后果自负。

不要问我源代码在哪里，不会公开。

其实发射相对接收简单许多，按照协议做就是了，对于熟手，依靠互联网公开的资料，半天到一天就能搞定。

