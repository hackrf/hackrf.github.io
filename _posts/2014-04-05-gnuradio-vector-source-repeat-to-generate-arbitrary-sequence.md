---
ID: 546
title: "GNURadio和HackRF使用Vector Source和Repeat来生成任意脉冲序列"
author: scateu
date: 2014-04-05 21:02:29
post_excerpt: ""
layout: post
published: true
views:
  - "2798"
duoshuo_thread_id:
  - "1312073613704167489"
---
<pre class="lang:default decode:true crayon-selected">+----------+     +----------+     +----------+     +----------+     +-----+     +-----+                        
|          |     |          |     |          |     |          |     |     |     |     |                
|          |     |          |     |          |     |          |     |     |     |     |                
|          |     |          |     |          |     |          |     |     |     |     |                
|          |     |          |     |          |     |          |     |     |     |     |                
|          |     |          |     |          |     |          |     |     |     |     |                
|          |     |          |     |          |     |          |     |     |     |     |                
+          +-----+          +-----+          +-----+          +-----+     +-----+     +-...                    

|&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |  t  |  t  |  t  |</pre>
假设我们想生成上图所示的波形。(实际上是27MHz遥控小车的前进遥控信号，详见<a title="用HackRF和GNURadio来实现对遥控小车的控制" href="http://www.hackrf.net/2014/03/%e7%94%a8hackrf%e5%92%8cgnuradio%e6%9d%a5%e5%ae%9e%e7%8e%b0%e5%af%b9%e9%81%a5%e6%8e%a7%e5%b0%8f%e8%bd%a6%e7%9a%84%e6%8e%a7%e5%88%b6/">这篇文章</a>)

其中t=0.5ms，信号由4个宽脉冲和10个窄脉冲组成。

那么，如何在不编写代码的情况下生成这个波形呢？

我们可以使用GNURadio提供的Vector Source模块。

![]({{ site.imageurl }}/2014/04/simple_generate.grc_.png)

我们在Vector Source里，填入
<pre class="lang:default decode:true">(1,1,1,0,1,1,1,0,1,1,1,0,1,1,1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0)</pre>
细心的同学一眼就能看出门道，1,1,1,0用来表示前面的宽脉冲，而1,0用来表示后面的窄脉冲。宽脉冲共4个，窄脉冲共10个。
<h3>Repeat</h3>
然后需要使用Repeat模块来使得每个数字都延续一定的时间。由于我们的框图中使用采样率为1MHz，那么想让每个数字所占用的时间t=0.5ms的话，则需要使Vector Source里的每个数字重复 1e6 * 0.5e-3 = 500 次。于是将Repeat模块的Interpolation设置为500即可。
<h3>测试</h3>
接着，我们可以连接WX Scope Sink来测试一下。

![]({{ site.imageurl }}/2014/04/simple_generated_scope_plot.png)
<h3>HackRF 发射</h3>
接上osmocom sink，将信号通过HackRF发出，设置中心频率为27MHz即可。

实际测试中，点击运行，旁边的遥控小车真的就动起来了。

&nbsp;

&nbsp;

另外，测试中发现，对于t=0.5ms这个值，遥控车的误差容忍是比较大的，在0.4-0.6之间貌似都能控制。
<h3>附: 小车控制指令集</h3>
n为窄脉冲的个数
<div>
<ul>
	<li>左: n=58</li>
	<li>右: n=64</li>
	<li>1档前进: n=10</li>
	<li>2档前进: n=22</li>
	<li>后退: n=40</li>
	<li>1档左前: n=28</li>
	<li>1档右前: n=34</li>
	<li>左后: n=46</li>
	<li>右后: n=52</li>
</ul>
</div>
<iframe width="320" height="240" style="width: 480px; height: 400px;" src="http://www.tudou.com/programs/view/html5embed.action?type=0&amp;code=wB4po5M5L5w&amp;lcode=&amp;resourceId=359197675_06_05_99" allowtransparency="true" border="0" frameborder="0" scrolling="no"></iframe>
