---
ID: 280
title: "A detail help info on hackrf_transfer"
author: scateu
date: 2014-03-15 00:35:15
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167465"
views:
  - "4782"
---
<p>Osmocom Sink has three gain,</p>

<h3>RF gain</h3>

<p>which is the switch for 14dB MGA-81563 amplifier.</p>

<h3>IF gain</h3>

<p>runs from 0 - 47 dB in osmocom_siggen runs from 0 - 40 dB in osmocom_fft</p>

<h3>BB gain</h3>

<p>RX: 0 - 62 dB (osmocom_fft)</p>

<p>So I guess the parameters of <code>hackrf_transfer</code> mean:</p>

<pre><code>-l  , set lna gain, means RX(osmocom source) IF gain, 8dB steps , 0-40dB
-i  , set vga(if) gain, means RX(osmocom source) BB gain, 2dB steps, 0-62dB
-x , set TX vga gain, means TX(osmocom sink) IF gain, 1dB steps, 0-47dB
-a , RF gain , controls 14dB MGA-81563 amplifier , and both RX and TX chain have its own MGA-81563, so it works for both RX and TX. 14dB or 0dB
</code></pre>

<p>And I think we should update the help info of hackrf_transfer.</p>

<h3>And Then</h3>

<blockquote>
  <p>Good catch.  I fixed the help in git, and I swiped -i for the IF
  frequency control since it didn't make any sense for RX VGA (baseband)
  gain (which is now -g).</p>
  
  <p>Mike</p>
</blockquote>
