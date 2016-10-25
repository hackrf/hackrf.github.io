---
ID: 269
title: "Csete给GNURadio提供的PPA"
author: scateu
date: 2014-03-14 13:35:19
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167463"
views:
  - "2195"
---
<p><a href="https://groups.google.com/forum/#!topic/gqrx/ALjNReW3RRE">原文出处</a></p>

<p>Greetings,</p>

<p>I am happy to announce that we now have a PPA with gqrx and gnuradio packages for Ubuntu 12.04, 12.10, 13.04 and 13.10, so it should cover a wide range of Debian &amp; derivative distributions.</p>

<p>Release channel: <a href="http://https://launchpad.net/~gqrx/+archive/releases">https://launchpad.net/~gqrx/+archive/releases</a>
Development snapshots: <a href="https://launchpad.net/~gqrx/+archive/snapshots">https://launchpad.net/~gqrx/+archive/snapshots</a></p>

<p>Currently, the release channel is empty but it will be populated when Gqrx 2.2 is released in a few days.</p>

<p>All dependencies that are not available from the official repositories should be available through the PPA. The PPA is still a work in progress and we need people to test the packages. I have already tested with Funcube Dongle Pro, Pro+ and rtlsdr. Will also test with USRP and HackRF later.</p>

<p>Before using this PPA you must make sure that any prior installations of gnuradio and gqrx (manually or via script) are removed or disabled.</p>

<p>Once you have a GNU Radio-free computer, installing from this PPA is as simple as:</p>

<pre><code>$ sudo apt-add-repository ppa:gqrx/snapshots
$ sudo apt-get install gqrx
</code></pre>

<p>For USB-based devices, you must copy the udev rules files to /etc/udev/rules.d/ manually. I will fix this later.</p>

<p>It has taken many days to get this up and running and I hope it will be useful. Feedback is welcome!</p>

<p>Alex</p>
