---
ID: 595
title: "airprobe的GNURadio 3.7版本及HackRF支持"
author: scateu
date: 2014-04-08 09:44:06
post_excerpt: ""
layout: post
published: true
views:
  - "7984"
duoshuo_thread_id:
  - "1312073613704167492"
---
由于GNURadio由3.6版本升级到3.7版本之后，原有的代码需要进行改动之后才可以进行编译。

改动介绍请参阅<a href="http://gnuradio.org/redmine/projects/gnuradio/wiki/Move_3-6_to_3-7">http://gnuradio.org/redmine/projects/gnuradio/wiki/Move_3-6_to_3-7</a>

否则会出现以下提示:
<pre class="lang:default decode:true">checking for GNURADIO_CORE... configure: error: Package requirements (gnuradio-core &gt;= 3) were not met:

No package 'gnuradio-core' found

Consider adjusting the PKG_CONFIG_PATH environment variable if you
installed software in a non-standard prefix.

Alternatively, you may set the environment variables GNURADIO_CORE_CFLAGS
and GNURADIO_CORE_LIBS to avoid the need to call pkg-config.
See the pkg-config man page for more details.</pre>
&nbsp;

Patch代码见<a href="https://github.com/scateu/airprobe-3.7-hackrf-patch">https://github.com/scateu/airprobe-3.7-hackrf-patch</a>
<pre class="lang:default decode:true crayon-selected">$ cd airprobe
$ patch -p1 &lt; ./zmiana.patch

  patching file gsm-receiver/Makefile.common
  patching file gsm-receiver/config/gr_libgnuradio_core_extra_ldflags.m4
  patching file gsm-receiver/config/gr_standalone.m4
  patching file gsm-receiver/src/lib/Makefile.swig.gen
  patching file gsm-receiver/src/lib/gsm.i
  patching file gsm-receiver/src/lib/gsm_constants.h
  patching file gsm-receiver/src/lib/gsm_receiver_cf.cc
  patching file gsm-receiver/src/lib/gsm_receiver_cf.h
  patching file gsm-receiver/src/python/gsm_receive.py
$ cd gsm-receiver
$ . ./bootstrap</pre>
&nbsp;

另外，gsm_receive_hackrf_3.7.py程序对原有的gsm_receive_rtl.py进行了修改，主要是改变了增益，请将它放在airprobe/gsm-receiver/src/python目录下使用。

&nbsp;
<h3>airprobe编译</h3>
<pre class="lang:default decode:true">git clone git://git.gnumonks.org/airprobe.git

cd airprobe/gsmdecode
./bootstrap
./configure
make

cd airprobe/gsm-receiver
./bootstrap
./configure
make</pre>
&nbsp;
