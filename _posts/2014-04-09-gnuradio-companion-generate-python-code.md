---
ID: 606
title: "gnuradio-companion生成Python代码"
author: scateu
date: 2014-04-09 12:13:46
post_excerpt: ""
layout: post
published: true
views:
  - "4129"
duoshuo_thread_id:
  - "1312073613704167493"
---
首先我们用gnuradio-compaion搭建如下的框图，功能是显示麦克风采回来的声音的频谱。<!--more-->

注意，在这里我们把Options里的ID改为了audio_test。

![]({{ site.imageurl }}/2014/04/a.png)

然后点击运行按钮左边的Generate the flow graph按钮，就会在当前目录生成audio_test.py
<pre class="nums:true lang:default decode:true">#!/usr/bin/env python
##################################################
# Gnuradio Python Flow Graph
# Title: Audio Test
# Generated: Wed Apr  9 12:01:49 2014
##################################################

from gnuradio import audio
from gnuradio import eng_notation
from gnuradio import gr
from gnuradio import wxgui
from gnuradio.eng_option import eng_option
from gnuradio.fft import window
from gnuradio.filter import firdes
from gnuradio.wxgui import fftsink2
from grc_gnuradio import wxgui as grc_wxgui
from optparse import OptionParser
import wx

class audio_test(grc_wxgui.top_block_gui):

    def __init__(self):
        grc_wxgui.top_block_gui.__init__(self, title="Audio Test")

        ##################################################
        # Variables
        ##################################################
        self.samp_rate = samp_rate = 32000

        ##################################################
        # Blocks
        ##################################################
        self.wxgui_fftsink2_0 = fftsink2.fft_sink_f(
        	self.GetWin(),
        	baseband_freq=0,
        	y_per_div=10,
        	y_divs=10,
        	ref_level=0,
        	ref_scale=2.0,
        	sample_rate=samp_rate,
        	fft_size=1024,
        	fft_rate=15,
        	average=False,
        	avg_alpha=None,
        	title="FFT Plot",
        	peak_hold=False,
        )
        self.Add(self.wxgui_fftsink2_0.win)
        self.audio_source_0 = audio.source(samp_rate, "", True)

        ##################################################
        # Connections
        ##################################################
        self.connect((self.audio_source_0, 0), (self.wxgui_fftsink2_0, 0))

# QT sink close method reimplementation

    def get_samp_rate(self):
        return self.samp_rate

    def set_samp_rate(self, samp_rate):
        self.samp_rate = samp_rate
        self.wxgui_fftsink2_0.set_sample_rate(self.samp_rate)

if __name__ == '__main__':
    import ctypes
    import os
    if os.name == 'posix':
        try:
            x11 = ctypes.cdll.LoadLibrary('libX11.so')
            x11.XInitThreads()
        except:
            print "Warning: failed to XInitThreads()"
    parser = OptionParser(option_class=eng_option, usage="%prog: [options]")
    (options, args) = parser.parse_args()
    tb = audio_test()
    tb.Start(True)
    tb.Wait()</pre>
代码很短，大致上做了以下几件事情：
<ul>
	<li>import许多依赖库</li>
	<li>以grc_wxgui.top_block_gui为基类继承出audio_test类
<ul>
	<li>该类描述了整个流图</li>
	<li>创建了Audio Source和FFT Sink，并将二者Connect起来。</li>
</ul>
</li>
	<li>第65行开始，是程序的主流程。可以理解为C语言里的main函数
<ul>
	<li>处理命令行参数</li>
	<li>创建我们上述定义的audio_test类</li>
	<li>调用它的Start方法，把流程图跑起来</li>
</ul>
</li>
</ul>
