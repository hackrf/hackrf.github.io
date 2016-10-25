---
ID: 367
title: "GNURadio关于OFDM的模块"
author: scateu
date: 2014-03-19 15:29:31
post_excerpt: ""
layout: post
published: true
views:
  - "7012"
duoshuo_thread_id:
  - "1312073613704167474"
---
GNURadio默认提供的OFDM调制解调模块见下面的列表。
<ol>
	<li>OFDM Carrier Allocator / OFDM载波分配器
<ul>
	<li>由复数值创建频域的OFDM符号，添加导频。</li>
	<li>This block turns a stream of complex, scalar modulation symbols into vectors which are the input for an IFFT in an OFDM transmitter. It also supports the possibility of placing pilot symbols onto the carriers.</li>
	<li>The carriers can be allocated freely, if a carrier is not allocated, it is set to zero. This allows doing OFDMA-style carrier allocations.</li>
</ul>
</li>
	<li>OFDM Channel Estimation /  OFDM信道估计
<ul>
	<li>Estimate channel and coarse frequency offset for OFDM from preambles</li>
	<li>Input: OFDM symbols (in frequency domain). The first one (or two) symbols are expected to be synchronisation symbols, which are used to estimate the coarse freq offset and the initial equalizer taps (these symbols are removed from the stream). The following <code>n_data_symbols</code> are passed through unmodified (the actual equalisation must be done elsewhere). Output: The data symbols, without the synchronisation symbols. The first data symbol passed through has two tags: 'ofdm_sync_carr_offset' (integer), the coarse frequency offset as number of carriers, and 'ofdm_sync_eq_taps' (complex vector). Any tags attached to the synchronisation symbols are attached to the first data symbol. All other tags are propagated as expected.</li>
</ul>
</li>
	<li>OFDM Cyclic Prefixer / OFDM循环前缀
<ul>
	<li>Adds a cyclic prefix and performs pulse shaping on OFDM symbols.</li>
	<li>Input: OFDM symbols (in the time domain, i.e. after the IFFT). Optionally, entire frames can be processed. In this case, <code>len_tag_key</code> must be specified which holds the key of the tag that denotes how many OFDM symbols are in a frame. Output: A stream of (scalar) complex symbols, which include the cyclic prefix and the pulse shaping. Note: If complete frames are processed, and <code>rolloff_len</code> is greater than zero, the final OFDM symbol is followed by the delay line of the pulse shaping.</li>
	<li>The pulse shape is a raised cosine in the time domain.</li>
</ul>
</li>
	<li>OFDM Demod / OFDM解调</li>
	<li>OFDM Frame Acquisition / OFDM帧采集
<ul>
	<li>take a vector of complex constellation points in from an FFT and performs a correlation and equalization.</li>
</ul>
</li>
	<li>OFDM Frame Equalizer / OFDM帧均衡器
<ul>
	<li>OFDM frame equalizer.</li>
	<li>Performs equalization in one or two dimensions on a tagged OFDM frame.</li>
	<li>This does two things: First, it removes the coarse carrier offset. If a tag is found on the first item with the key 'ofdm_sync_carr_offset', this is interpreted as the coarse frequency offset in number of carriers. Next, it performs equalization in one or two dimensions on a tagged OFDM frame. The actual equalization is done by a ofdm_frame_equalizer object, outside of the block.</li>
</ul>
</li>
	<li>OFDM Insert Preamble / OFDM插入前同步码
<ul>
	<li>insert "pre-modulated" preamble symbols before each payload.</li>
</ul>
</li>
	<li>OFDM Mod / OFDM调制</li>
	<li>OFDM Receiver / OFDM接收机</li>
	<li>OFDM Sampler / OFDM 采样器</li>
	<li>OFDM Serializer / OFDM 序列化
<ul>
	<li>Serializes complex modulations symbols from OFDM sub-carriers.</li>
</ul>
</li>
	<li>OFDM Sync PN /</li>
	<li>Schmidl &amp; Cox OFDM synch.</li>
	<li>OFDM Transmitter / OFDM 发射机</li>
</ol>
&nbsp;

下图是ofdm_loopback.grc，实现了一个简单的OFDM回环。

![]({{ site.imageurl }}/2014/03/ofdm_loopback.grc_.png)

更详细的示例可以参见/usr/local/share/gnuradio/examples/digital/ofdm/里的其它框图。

&nbsp;
<h2>参考文献</h2>
[1] <a href="http://gnuradio.org/redmine/attachments/download/134/gr_ofdm.ppt">http://gnuradio.org/redmine/attachments/download/134/gr_ofdm.ppt</a>
