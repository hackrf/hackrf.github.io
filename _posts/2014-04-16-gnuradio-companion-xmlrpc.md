---
ID: 712
title: "使用XMLRPC来对GNURadio进行远程调用"
author: scateu
date: 2014-04-16 14:36:50
post_excerpt: ""
layout: post
published: true
views:
  - "2021"
duoshuo_thread_id:
  - "1312073613704167500"
---
在实际使用gnuradio-companion的过程中，我们经常需要对GNURadio的框图的各个参数进行动态地修改。

在本地使用的时候，我们可以在grc框图中使用Slider来进行本地的参数改变。

但是，在需要使用程序远程地通过网络对grc框图控制，本地控制就会很麻烦，例如需要使用远程桌面的方式登过去操作。

于是，XMLRPC Server应运而生。<!--more-->

XMLRPC的意思是XML格式的远程过程调用。

首先，我们搭建如下框图，文件可以在<span class="lang:default decode:true  crayon-inline ">/usr/local/share/gnuradio/examples/grc/xmlrpc/xmlrpc_server.grc</span> 处得到

![]({{ site.imageurl }}/2014/04/xmrpc_server.png)

然后添加一个XMLRPC Server模块，监听在本地的1234端口。

然后，我们就可以写一个Python程序来对它进行控制，代码在<span class="lang:default decode:true  crayon-inline">/usr/local/share/gnuradio/examples/grc/xmlrpc/xmlrpc_client_script.py</span>
<pre class="lang:default decode:true ">#!/usr/bin/env python

import time
import random
import xmlrpclib

#create server object
s = xmlrpclib.Server("http://localhost:1234")

#randomly change parameters of the sinusoid
for i in range(10):
    #generate random values
    new_freq = random.uniform(0, 5000)
    new_ampl = random.uniform(0, 2)
    new_offset = random.uniform(-1, 1)
    #set new values
    time.sleep(1)
    s.set_freq(new_freq)
    time.sleep(1)
    s.set_ampl(new_ampl)
    time.sleep(1)
    s.set_offset(new_offset)</pre>
我们可以看到，只需要简单地调用xmlrpclib库，然后就可以对应的函数来控制grc框图里的参数了。

&nbsp;

如果不想写代码，在远程再起一个框图

![]({{ site.imageurl }}/2014/04/xmlrpc_client.png)

就可以完成控制了。
