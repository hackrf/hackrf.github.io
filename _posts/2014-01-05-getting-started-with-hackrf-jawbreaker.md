---
ID: 144
title: "Getting started with HackRF Jawbreaker"
author: scateu
date: 2014-01-05 01:26:30
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167438"
views:
  - "3183"
---
<a href="http://bgamari.github.io/posts/2013-06-15-hackrf.html">原文</a>

<section id="main">
<h1 id="getting-started-with-hackrf-jawbreaker">Getting started with HackRF Jawbreaker</h1>
This was done on Ubuntu 13.04. We’ll assume that <code>$ROOT</code> is set to the directory within which you will be working. For this you’ll need (at very least) the <code>build-essential</code> and <code>cmake</code> packages.
<h2 id="compiling-libhackrf-and-hackrf-tools">Compiling <code>libhackrf</code> and <code>hackrf-tools</code></h2>
<pre><code>$ cd $ROOT
$ git clone git://github.com/mossmann/hackrf.git
$ cd hackrf/host
$ mkdir build
$ cd build
$ cmake ..
$ make
$ sudo make install</code></pre>
By default <code>cmake</code> will install things in <code>/usr/local</code>. We’ll need this in <code>PATH</code> and <code>LD_LIBRARY_PATH</code>,
<pre><code>$ PATH=$PATH:/usr/local/bin
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib</code></pre>
<h2 id="compiling-new-firmware">Compiling new firmware</h2>
To compile the firmware we’ll need mossmann’s <code>libopencm3</code> branch as well as the <code>gcc-arm-embedded</code> toolchain available.

We’ll be compiling two separate firmware images. The image compiled by <code>Makefile</code> is compiled assuming the device will be booting the firmware directly from RAM while that built by <code>Makefile_rom_to_ram</code> will be built under the assumption that it was loaded from ROM.
<pre><code>$ cd $ROOT/hackrf
$ git clone git://github.com/mossmann/libopencm3.git
$ cd libopencm3
$ make
$ cd $ROOT/hackrf/firmware/hackrf_usb
$ make LIBOPENCM3=$ROOT/hackrf/libopencm3
$ make LIBOPENCM3=$ROOT/hackrf/libopencm3 -f Makefile_rom_to_ram</code></pre>
<h2 id="updating-firmware">Updating firmware</h2>
<pre><code>$ lsusb
Bus 002 Device 008: ID 1d50:604b OpenMoko, Inc. 
...
$ sudo chmod ugo+rw /dev/bus/usb/002/008
$ hackrf-spiflash -w $ROOT/hackrf/firmware/hackrf-usb/hackrf_usb_rom_to_ram.bin</code></pre>
<h2 id="fetching-and-installing-gnuradio">Fetching and installing gnuradio</h2>
As it turns out, Python 3 tends to confuse gnuradio’s <code>cmake</code> configuration. For this reason, we force <code>cmake</code> to build against Python 2.7,
<pre><code>$ cd $ROOT
$ git clone git://github.com/gnuradio/gnuradio.git
$ cd gnuradio
$ mkdir build
$ cd build
$ cmake -DPYTHON_EXECUTABLE=/usr/bin/python2.7 -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython2.7.so.1 ../ -DENABLE_GR_WAVELET=ON -DEANBLE_GR_</code></pre>
You should check over the output of cmake at this point and find any missing dependencies (look for <code>gr-*</code> modules which are listed as <code>disabled</code>). I happened to lack the <code>cheetah</code>, <code>sdl</code>, and <code>fftw</code> packages,
<pre><code>$ sudo apt-get install python-cheetah libfftw3-dev libsdl1.2-dev</code></pre>
After installing dependencies you should re-run <code>cmake</code>.

The build itself will take awhile,
<pre><code>$ make
$ sudo make install</code></pre>
<h2 id="fetching-and-installing-gr-osmosdr">Fetching and installing <code>gr-osmosdr</code></h2>
The <code>gr-osmosdr</code> block provides an interface between the Jawbreaker hardware and GnuRadio,
<pre><code>$ cd $ROOT
$ git clone git://git.osmocom.org/gr-osmosdr
$ cd gr-osmosdr
$ mkdir build
$ cmake -DPYTHON_EXECUTABLE=/usr/bin/python2.7 -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython2.7.so.1 ../
$ make
$ sudo make install</code></pre>
<h2 id="starting-gnuradio-companion">Starting GnuRadio Companion</h2>
After starting GnuRadio Companion,
<pre><code>$ export PYTHONPATH=$PYTHONPATH:/usr/lib/python2.7/dist-packages
$ gnuradio-companion</code></pre>
You should see an <code>osmocom</code> source and sink. This can be used to pull or push samples from the Jawbreaker.

To test, connect an <code>osmocom</code> source (<code>Sources</code> group) to a <code>WX GUI FFT Sink</code> (<code>Instrumentation/WX</code> group), press the “Generate flow graph” button and then “Execute flow graph”. You should see a window like,
<div><img alt="Spectrum from HackRF" src="http://bgamari.github.io/media/hackrf-fft.png" />Spectrum from HackRF

</div>
Press your thumb against the antenna to confirm a change in the spectrum.
<h2 id="tuning-to-fm-bands">Tuning to FM bands</h2>
See <a href="http://bgamari.github.io/media/fm-decoder.grc">this graph</a> for an example signal chain decoding a standard FM radio station (88.5 MHz, by default).

</section>
