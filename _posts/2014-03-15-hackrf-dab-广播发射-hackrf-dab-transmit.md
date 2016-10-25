---
ID: 283
title: "HackRF DAB 广播发射/ HackRF DAB Transmit"
author: scateu
date: 2014-03-15 01:36:27
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167466"
views:
  - "12492"
---
<h3>Requirements</h3>

<ul>
<li>ffmpeg</li>
<li>tooLAME version 0.2l (http://toolame.sourceforge.net)</li>
<li>CRC-DabMux</li>
<li>crc-dabmod</li>
</ul>

<h3>Commands</h3>

<pre><code>mkfifo ./my.fifo

ffmpeg -re -i alizee.mp3 -ar 48000 -f wav - |toolame -s 48 -D 4 -b 128 /dev/stdin ./my.fifo


CRC-DabMux -A ./my.fifo -b 128 -i 1 -p 3 \
    -S -L "Startest HackRF.net" -C -i1 \
    -O fifo:///dev/stdout | \
    crc-dabmod -f -g0 -r 3200000 |\
    python hackrf_dab_sink.py
</code></pre>

<h3>Details</h3>

<ul>
<li><p><code>ffmpeg</code></p>

<ul>
<li><code>-re</code> means encode mp3 file in real time.</li>
</ul></li>
<li><p><code>toolame</code> converts raw wav input from <code>standard input</code> and encode into MP2 then output to <code>my.fifo</code></p>

<pre><code>python hackrf_dab_sink.py
</code></pre></li>
</ul>

<h3>About <code>hackrf_dab_sink.py</code></h3>

<p>it's just a simple gnuradio-companion flow graph.</p>

<pre><code>+-------------------+
|Sample Rate : 3.2M |
+-------------------+
+--------------+    +--------------+   +--------------+
|File Source   |    |Multiply      |   | osmocom sink |
| /dev/stdin   |____| Const        |___| 218.64MHz    |
| Complex      |    |    0.8e-3    |   | 14,50,20dB   |
|              |    |              |   |              |
+--------------+    +--------------+   +--------------+
</code></pre>

<p><em>Hint</em>: You need to adjust <code>Multiply Const</code> in order to avoid overload of output signal.</p>

<pre><code>#!/usr/bin/env python
##################################################
# Gnuradio Python Flow Graph
# Title: Hackrf Dab Sink
# Generated: Mon Mar  3 18:19:58 2014
##################################################

from gnuradio import blocks
from gnuradio import eng_notation
from gnuradio import gr
from gnuradio.eng_option import eng_option
from gnuradio.filter import firdes
from optparse import OptionParser
import osmosdr

class hackrf_dab_sink(gr.top_block):

    def __init__(self):
        gr.top_block.__init__(self, "Hackrf Dab Sink")

        ##################################################
        # Variables
        ##################################################
        self.samp_rate = samp_rate = 3200000

        ##################################################
        # Blocks
        ##################################################
        self.osmosdr_sink_0 = osmosdr.sink( args="numchan=" + str(1) + " " + "hackrf=0" )
        self.osmosdr_sink_0.set_sample_rate(samp_rate)
        self.osmosdr_sink_0.set_center_freq(168.160e6, 0)
        self.osmosdr_sink_0.set_freq_corr(-34, 0)
        self.osmosdr_sink_0.set_gain(14, 0)
        self.osmosdr_sink_0.set_if_gain(50, 0)
        self.osmosdr_sink_0.set_bb_gain(20, 0)
        self.osmosdr_sink_0.set_antenna("", 0)
        self.osmosdr_sink_0.set_bandwidth(1.75e6, 0)

        self.blocks_multiply_const_vxx_0 = blocks.multiply_const_vcc((0.8e-3, ))
        self.blocks_file_source_0 = blocks.file_source(gr.sizeof_gr_complex*1, "/dev/stdin", True)

        ##################################################
        # Connections
        ##################################################
        self.connect((self.blocks_multiply_const_vxx_0, 0), (self.osmosdr_sink_0, 0))
        self.connect((self.blocks_file_source_0, 0), (self.blocks_multiply_const_vxx_0, 0))


# QT sink close method reimplementation

    def get_samp_rate(self):
        return self.samp_rate

    def set_samp_rate(self, samp_rate):
        self.samp_rate = samp_rate
        self.osmosdr_sink_0.set_sample_rate(self.samp_rate)

if __name__ == '__main__':
    parser = OptionParser(option_class=eng_option, usage="%prog: [options]")
    (options, args) = parser.parse_args()
    tb = hackrf_dab_sink()
    tb.start()
    tb.wait()
</code></pre>

<h3>some typical DAB Channel</h3>

<ul>
<li>168.160MHz : China Channel 6B </li>
<li>218.640MHz : Europe Channel 11B</li>
</ul>

<h3>How to use tmux to open those two window at the same time?</h3>

<pre><code>tmux new-session -d -s dab
tmux send-key -t dab cd\ /home/scateu/hackrf/01Book/files/DAB Enter 
tmux send-key -t dab ./02alizee.sh Enter
tmux split-window -h -t dab
tmux send-key -t dab cd\ /home/scateu/hackrf/01Book/files/DAB Enter 
tmux send-key -t dab ./03hackrf_sink.sh Enter
tmux att -t dab
</code></pre>

<p>目前已经被rtl-sdr.com <a href="http://www.rtl-sdr.com/transmitting-dab-hackrf/">引用</a></p>

<blockquote>
  <p>A RTL-SDR.com reader has written in to let us know about his project
  involving transmitting Digital Audio Broadcasting (DAB) using GNU
  Radio and the HackRF. DAB is a digital radio technology that is used
  to broadcast radio stations. He uses the CRC-DABMUX and CRC-DABMOD
  software to modulate an audio file into DAB and then uses a GNU Radio
  python script to write the modulated signal to the HackRF for
  transmitting.</p>
</blockquote>
