---
ID: 333
title: " gnuradio-companion搭载hackrf one制作FM收音机带频谱仪(一)"
author: paud
date: 2014-03-17 20:31:49
post_excerpt: ""
layout: post
published: true
views:
  - "7845"
duoshuo_thread_id:
  - "1312073613704167470"
---
well, just keep it simple and stupid，直接上图了

![]({{ site.imageurl }}/2014/03/2014-03-17-181724的屏幕截图.png)

本菜对照图片给各位菜菜们讲解其原理，要搭成以上流图，请执行以下步骤

1.添加osmocom source，按ctrl+f，在右侧列表搜索，位于source 目录

2.同样方式，将其他模块添加好，fft sink 位于instrumentation / wx，wbfm receive位于modulators，audio sink如其名位于audio

3.然后链接，只要点一下,in out就可以链接了。

4.按照上图对模块进行设置，双击就可以看到属性对话框了，注意几点，source做为源的接收器，pmm要设置成你的hackrf的ppm(见包装盒内测试报告单)，frequency则是你要接收的频率，本菜收听的是104.8MHz的调频广播。

5.从信号源输出之后兵分两路，一部分显示波形，即GUI FFT，令一部分则通过WBFM解码，然后由audio sink进行播放。

well done, so far so good! 下面来测试我们的劳动成果，

首先generate the flow graph(F5)，然后execute(F6),即可看到和听到我们的广播了，okay, show me!

![]({{ site.imageurl }}/2014/03/2014-03-17-182035的屏幕截图.png)

&nbsp;

未完待续……

to be continue...

下一节本菜教大家制作带调节器的FM收音机！

<span style="line-height: 1.5em">本文版权归本人</span><span style="line-height: 1.5em">QQ:35769382</span><span style="line-height: 1.5em">和hackrf.net所有，转载请注明出处，本讲到此为止，谢谢大家，再会！</span>
