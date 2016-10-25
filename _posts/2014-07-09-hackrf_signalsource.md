---
ID: 964
title: "为HackRF添加模拟信号源功能"
author: dovecho
date: 2014-07-09 21:43:54
post_excerpt: ""
layout: post
published: true
views:
  - "5307"
duoshuo_thread_id:
  - "1312073613704167530"
---
HackRF提供了诸多方便的小工具， 其中最重要的莫过于Hackrf_transfer了，它可以在Linux、Mac、Windows等诸多系统下使用，实现HackRF最基本的信号产生、接收功能。<!--more-->

最近对hackrf_transfer做了一些修改，增加了一个全新的SignalSource模式，可以在命令行下直接控制HackRF产生单频模拟信号，方便各种调试与测试，代码可以参见Github：

<a title="github.com/dovecho: hackrf_transfer.c" href="https://github.com/dovecho/hackrf/blob/master/host/hackrf-tools/src/hackrf_transfer.c" target="_blank">源代码</a>、<a title="github.com/dovecho: hackrf_transfer.c" href="https://github.com/dovecho/hackrf/commits/master/host/hackrf-tools/src/hackrf_transfer.c" target="_blank">修改历史</a>

增加了[-c]开关，通过 [-c] 指定输出信号幅度，POWER_LEVEL，激活SignalSource模式，取值0~255，如下：
<pre>[-c power_level] # Signal source mode, power level, 0-255, dc value to DAC (default is 1)</pre>
使用时，除了[-c] 外，再指定频率、各滤波器增益，还要指定采样率（选择系统支持的即可），不需指定发射文件，使用方法：
<pre>hackrf_transfer -c POWER_LEVEL -f FREQUENCY ...</pre>
目前已经向<a href="https://github.com/mossmann/hackrf" target="_blank">mossmann</a>提交了<a href="https://github.com/mossmann/hackrf/pull/118" target="_blank">PullRequest</a>，心急的筒子们可以先现在下载源代码编译。

&nbsp;
