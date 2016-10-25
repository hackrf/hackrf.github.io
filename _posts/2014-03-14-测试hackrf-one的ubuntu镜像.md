---
ID: 274
title: "测试HackRF One的Ubuntu虚拟机"
author: citypw
date: 2014-03-14 20:42:56
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167464"
views:
  - "7023"
---
为了方便大家玩HackRF One，这里我共享了一个Ubuntu的VM虚拟机(vmware workstation)的文件，只需要进入后就可以使用最新版本的gnuradio和gqrx(没测试过)

注意，目前该VM文件只在64位Linux系统中测试通过。Windows上由于驱动的问题，目前还不能跑通。

另：该虚拟机在Mac下通过Parallels是可以跑起来的。

<a href="http://pan.baidu.com/s/1mgNp5cG">http://pan.baidu.com/s/1mgNp5cG</a>

由于百度盘每个文件要不能大于1GB，所以把文件拆分成了4个文件，运行以下命令让文件还原：<code> </code>

$cat sdr12.04_part_a* &gt; sdr12.04.tar.bz2

之后就可以正常解压缩了。
<pre><code>用户名:root 密码:loveeff
</code></pre>
Happy Hacking!!!
