---
ID: 718
title: "为gnuradio-companion安装图标及文件关联"
author: scateu
date: 2014-04-17 01:03:28
post_excerpt: ""
layout: post
published: true
views:
  - "2217"
duoshuo_thread_id:
  - "1312073613704167501"
---
<h2>问题</h2>
默认安装的gnuradio-companion并没有图标，丑哭了..........

而且grc文件的打开方式并没有关联到gnuradio-companion上
<h2>解决</h2>
其实是准备了图标的，只需要
<pre class="lang:default decode:true">/usr/local/libexec/gnuradio/grc_setup_freedesktop install</pre>
就好啦

并且双击扩展名为grc的文件会自动被gnuradio-companion打开。

&nbsp;
<h2>参考</h2>
<a href="http://gnuradio.org/redmine/projects/gnuradio/wiki/GNURadioCompanion">http://gnuradio.org/redmine/projects/gnuradio/wiki/GNURadioCompanion</a>

![]({{ site.imageurl }}/2014/04/grc-icon-256.png)
