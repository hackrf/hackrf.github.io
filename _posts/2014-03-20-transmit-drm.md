---
ID: 398
title: "GNURadio / HackRF 发射DRM广播信号"
author: scateu
date: 2014-03-20 12:12:42
post_excerpt: ""
layout: post
published: true
views:
  - "4288"
duoshuo_thread_id:
  - "1312073613704167477"
---
DRM(Digital Radio Modiale)，用于短波和超短波波段(HF和VHF)，实现数字音频广播，带宽在10-20kHz。

<a href="https://github.com/kit-cel/gr-drm">gr-drm项目</a>使用GNURadio实现了一个DRM短波发射机，基于这套流程，我们可以使用HackRF发射DRM广播了。

实现了以下功能:
<ol>
	<li>DRM音频编码</li>
	<li>DRM Scrambler</li>
	<li>DRM SDC业余描述信道生成</li>
	<li>DRM FAC快速访问信道生成</li>
	<li>交织</li>
	<li>加入OFDM 循环前缀</li>
	<li>EEP 相等级别保护</li>
</ol>
另外，gr-drm项目参加了2012年度的Google Summer of Code活动，最近作者更新之后，已经支持GNURadio 3.7了。

![]({{ site.imageurl }}/2014/03/drm.png)
<h2>安装注意事项</h2>
<pre>
git clone http://github.com/kit-cel/gr-drm
</pre>
如果只进行发射，只需要安装faac即可。切勿使用软件源里提供的faac，因为它没有提供针对DRM的支持。

其它按照README.md来操作即可。

最后不要忘记执行一下gr-drm/hier-blocks里面的流程框图，来生成相应的XML模块。
<h2>参考文献</h2>
[1] https://github.com/kit-cel/gr-drm/raw/master/drm_transmitter_gnuradio.pdf

[2] https://github.com/kit-cel/gr-drm/raw/master/drm_transmitter_presentation.pdf

[3] https://github.com/kit-cel/gr-drm/raw/master/misc/drm_transmitter_design.pdf

[4] http://www.drm.org
