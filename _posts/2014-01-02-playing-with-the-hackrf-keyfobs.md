---
ID: 97
title: "'Playing with the HackRF &#8211; Keyfobs'"
author: scateu
date: 2014-01-02 15:28:33
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167436"
views:
  - "12458"
---
<p><a href="http://blog.kismetwireless.net/2013/08/playing-with-hackrf-keyfobs.html">原文</a>
The HackRF is on Kickstarter, so here's a little write-up of some of the awesome stuff yohttp://blog.kismetwireless.net/2013/08/playing-with-hackrf-keyfobs.htmlu can do with it, and why you should go get one.</p>

<p>I've been wanting to play around with my car keyfobs for a while; mostly out of idle curiosity. They're something everyone has, and tend to put out a nice clean signal. For my testing I used a 2006 Subaru keyfob, and my friend's Kia keyfob - they use different encodings, so this is a fairly interesting test.</p>

<p>Beyond academic interest, keyfobs may not be that useful to play with - they (should) all use rolling codes so that a captured unlock sequence can't be used more than once. I wouldn't assume they've all implemented this properly, of course, as some conferences and speakers are pointing out. It's definitely an area where more research is needed.</p>

<p>To start with, I did some searching to find out what frequency they operate at. It turns out Kia runs at 315MHz, while Toyota and Subaru run at 433.847MHz (for many models, at least).</p>

<p>So, now we know where to look - what do we do with that? First off, I fired up GQRX to get an idea of what was going on; pressing the keyfob returned data (sorry, don't have a screenshot handy of that specifically, but here's an example of gqrx capturing OOK data):</p>

<p>So we know something is going on; now we want to record it. The quickest way to do this is hackrf_transfer:</p>

<p>$ hackrf_transfer -r Kia-312MHz-8M-8bit.iq -f 312000000 -s 8000000</p>

<p>Notice we're capturing slightly off-center from 315MHz. Since we're capturing 8MHz wide, we'll get our signal, but by offsetting from center we avoid the DC offset which causes a spike at the center of the HackRF tuning band. Recent firmware has made this significantly smaller, but it's still easy to avoid entirely, so lets do that.</p>

<p>Leave hackrf_transfer running for a while, while pressing the lock (or unlock) button on your key fob a few times. After a few seconds, hit control-c to stop capturing.</p>

<p>I suggest naming your files carefully - the capture data itself is raw IQ samples, and doesn't contain any indication of how wide the samples are (8M), what the center frequency is (312MHz), or how it's encoded (for HackRF, all the captures are 8bit). Better to name it properly to begin with.</p>

<p>Now we've got a big file of capture data; what can we do with it? One good tool is baudline - http://www.baudline.com/download.html ... Unfortunately the licensing terms for baudline prevent its inclusion in distributions, so you'll need to download it independently.</p>

<p>There are a few quirks with baudline - one of them is that it doesn't handle large files (over about 50MB) very well, another is that it runs "file" (the file autotype detection) on any file you provide for it. This is slow, so a quick fix for that is to alter your $PATH variable before launching baudline. My baudline launcher script looks like:</p>

<h1>!/bin/sh</h1>

<p>export PATH=~/bin/disable:$PATH baudline</p>

<p>and ~/bin/disable/ contains a "file" script which contains simply:</p>

<h1>!/bin/sh</h1>

<p>exit 0</p>

<p>So we need to make a smaller file for baudline - fortunately, "dd" handles this very well. To get the first 50MB of the sample file,</p>

<p>$ dd if=foo.iq of=trimmed.iq bs=1M count=50</p>

<p>To get subsequent blocks of the file,</p>

<p>$ dd if=foo.iq of=trimmed.iq bs=1M count=50 skip=50</p>

<p>The tool "split" can also be used to break the file up into parts.</p>

<p>So we've got a smaller chunk of our file, let's open it up in baudline. Use right-click, "input->open file..." then select the file and set the format to "raw" instead of "auto detect". That will get you:</p>

<p>The magic settings here are:</p>

<p>"custom" sample rate of 8M (since we captured at 8MHz / 8M samples wide). If you captured at another rate, like 20MHz, put 20000000 here.</p>

<p>"channels" are 2, since hackrf logs I and Q data. Since we're logging IQ, turn on the "quadrature" checkbox, and since HackRF logs differently than baudline expects, turn on "flip complex".</p>

<p>Finally, since HackRF logs unsigned 8bit samples, click "8 bit linear (unsigned)".</p>

<p>Now we get a fancy display, like:</p>

<p>And we get a nice display of our signal; in this screenshot it's a Subaru keyfob using OOK (on-off-keying) encoding, basically morse code (transmit or not transmit)</p>

<p>Using the Kia keyfob, we get a FSK (frequency-shift-keying) signal, zoomed in we see:</p>

<p>What do we see here? After the transmitter turns on, it sends a pattern of 'off' and 'on' signals, basically '101010101010'. Look back at the Subaru OOK capture - it also sends a consistent '1010101010' pattern at the beginning. This preamble tells the receiver that real packet data is coming, and helps us see how long the duty cycle is for 'on' and 'off'.</p>

<p>Scrolling further down the Kia FSK signal, we see it begin to send real data:</p>

<p>Which is the actual FSK encoded bits being sent to the car to unlock.</p>

<p>Finally, in baudline, it's possible to select a range and save it to a file:</p>

<p>by selecting the range, and right clicking to "output->export selection as..."</p>

<p>Beware, baudline changes the format of the file when you export it. This will affect loading the file in the future, and any gnuradio code you develop to process the file in the future. A less destructive option is to use 'dd' to trim the file (just make some guesses at the size) and load it into baudline.</p>

<p>To re-open a file you've saved from baudline, use the following settings:</p>

<p>Specifically, the file changes from 8bit unsigned to 16bit, and no longer needs the "flip complex" checkbox set.</p>

<p>Next post, I'll load a HackRF sample file into Gnuradio-Companion and try to extract the ASK bitstream directly.</p>

<h1>Part 2</h1>

<p>Playing with keyfobs and baudline is a lot of fun, now lets try something more complex where the output data has more meaning (even if we decode the keyfob data, it's probably not going to show anything logical other than a big random number - feel free to try though!)</p>

<p>The transmitter in this case is a junky 433mhz transmitter meant for use with Arduino, which runs about $2:</p>

<p>We know this one transmits at 433mhz, so lets do a capture with the HackRF at 435MHz:</p>

<p>$ hackrf_transfer -r 435MHz-ASK-HACKRF-CW-8M.iq -f 435000000 -s 8000000</p>

<p>When we load this into Baudline (see previous post about this), we get:</p>

<p>Who remembers their CW?</p>

<p>So we know we have something reasonable in the file; now lets load it into GRC, GNU Radio Companion.</p>

<p>GRC allows you to build flow graphs with a GUI. They're then compiled to python (and can be run independently of GRC). Writing code using the GNU Radio blocks can achieve the same thing, but GRC has a lot of helpful features to make it simpler for GNU Radio newbies (of which I'm definitely one). One downside is the GRC UI can sometimes make it challenging to find block - if you're having trouble finding something in the list, you can start typing the name and it should search for it.</p>

<p>Step one is to load our log file into a format GNU Radio can use easily. The HackRF logs are IQ data pairs, stored as unsigned 8 bit integers (you can read more about IQ, or Quadrature Sampling, here). GNU Radio components generally want complex IQ data, so we need to transform the file to that (as a big ugly image that breaks the layout entirely on the blog page):</p>

<p>You can get the grc xml file here.</p>

<p>A lot of things are going on here, and we haven't even started trying to do anything interesting!</p>

<p>This uses the standard "File Source" component to load the file, then converts UChars (unsigned 8 bit values) to floats in the "UChar to Float" block. The "Deinterleave" stage splits the single stream of IQIQIQ data into I and Q channels, then recombines them as a "complex" stream via "Float to Complex", which most GNU Radio stages expect.</p>

<p>The "Add Const" stage then re-centers the data around 0; since it uses unsigned ints, with a range of 0-256, the HackRF samples center around 127. (I had to bug Mike Ossmann about this - all the more reason to go sign up for the HackRF kickstarter - if it makes the stretch goal, we get videos of GNU Radio courses!)</p>

<p>Finally, the "Throttle" stage does, perhaps, the obvious - throttles the throughput to 8M. Without this stage, it could easily swamp the system it is running on. In general I've found it makes sense to throttle to the rate of the capture, because there can be odd interactions between rate and bandwidth otherwise - with IQ samples, the rate and bandwidth are linked.</p>

<p>The throttle stage uses the variable throttle_rate, found at the top of the image. This lets you easily set the rate for different capture, with one significant gotcha: GRC doesn't represent numbers in a format it can actually use! GRC can accept raw numbers, or scientific notation (or hex, or octal, or any other format Python supports), but represents them as a human readable value which is NOT accepted. In this case, 8000000 or 8e6 are acceptable values, however 8M is not. Go figure.</p>

<p>So, now we have the file loaded in a format we can use. What next?</p>

<p>The first step is to look at our signal - we can direct the output of throttle into a FFT sink like this:</p>

<p>And get a view of our signal:</p>

<p>Where we see our DC spike at our capture frequency (435MHz), and some of our signal, around 433.8MHz.</p>

<p>Now, look at the setup of the FFT sink - by default GNU Radio doesn't know (and doesn't care) what your center frequency is. Unless you tell it otherwise, your center frequency is 0. The signal would then be at -1.2MHz. Nothing really cares about this, but I like to know where things really are, so I like to configure the FFT to be accurate:</p>

<p>In this case, the "Sample Rate" and "Baseband Freq" are set to variables, "samp_rate" and "capture_freq". "Sample Rate" should match the input sample rate (in this case, 8M, the value we throttled to) or else the FFT won't update properly.</p>

<p>We use variable blocks in GRC, so:</p>

<p>So now we know where our signal is - to avoid the spike of the DC offset, we captured off-center at 435MHz. We also captured far, far more data than we ever need in terms of bandwidth. We can solve both of these problems at once using a "Frequency XLating FIR Filter".</p>

<p>The Frequency XLating Filter allows us to do three things at once:</p>

<pre><code>Shift the center frequency of the signal.  We'll use this to "scroll down" to our desired signal.
Apply a pass filter.  This filters out parts of the signal we don't care about, so when we're measuring things later on, we're only measuring our signal and not some other radio noise.
Decimate the signal.  Contrary to the actual definition of the word, in SDR terms, Decimate simply means Divide.  This reduces the bandwidth and sample of the signal, which reduces the amount of processing power need to handle it.  Since this reduces the sample rate, we'll have to remember this in future blocks!
</code></pre>

<p>Here we set the decimation using variables again - they're so much more convenient than trying to change all the dependent modules later. To get the decimation value, we divide the incoming sample rate by the target rate - in this case, 50KHz, a relatively arbitrary value - it's wide enough to encompass our signal, and we don't have to fiddle too much because our signal is VERY strong in this sample. In reality we could probably use a much smaller value here. We have to cast the result to an integer, otherwise GRC gets angry; Since this is really all Python under the covers, we use the Python integer cast, int(...).</p>

<p>Under "Taps", we use the variable firdes_tap - more on this later. This performs the filtering to isolate our signal.</p>

<p>Finally under "Center Frequency", we tell it to shift - again using variables. We want to shift down to our signal, so we need to shift by about 1.2MHz, or 435MHz - 433.828MHz. Since we're shifting down, we make it negative.</p>

<p>I use GNU Radio 3.6 - the latest cutting edge release is 3.7, which has changed some things around. Specifically in this case, the "Center Frequency" is now treated as the "actual" center frequency. If you're on GNU Radio 3.7, remove the negative sign here. This, and other changes, can be found here.</p>

<p>[ EDIT 09/02/2013 - Having updated to GNU Radio 3.7, I'm not sure what the changelog really indicates should be done - a negative offset as shown in the example script works fine, and a positive does not. ]</p>

<p>The firdes_tap variable defines our low-pass filter. The key parameters here are:</p>

<pre><code>Gain (1) - We're not adding any gain into the signal.
Sample rate (samp_rate) - This has to match the sample rate coming in, in this case 8M
Cutoff frequency (2000) - We're defining the lower cutoff at 2KHz
Transition band frequency (20000) - Set the transition band of the filter.  This should be less than half of the sample rate (in our case our decimated sample rate is 50k, so 20k is reasonable)
</code></pre>

<p>The WIN_HAMMING and 6.76 parameters tell GNU Radio to use a windowed filter and can generally be left alone.</p>

<p>After passing through the filter, decimation, and re-centering, we can put the signal back into a FFT display and see something like this:</p>

<p>Notice that the signal covers much less bandwidth, and is silenced except around our target area. Playing with the values in the low_pass filter will yield different results, and you can learn a lot more about lowpass filter setup here.</p>

<p>Remember as well to set up your FFT plot properly:</p>

<p>Specifically, since we have decimated our signal to our target rate, it is now only 50KHz. We have also changed the center frequency of the signal, so we set both "Sample Rate" and "Baseband Freq" to our target variables.</p>

<p>Now we have a lower-bandwidth, centered, filtered, ASK / OOK signal. What can we do with it?</p>

<p>ASK and OOK are basically "high volume for one" and "low volume for zero". They vary the amplitude (hence "Amplitude Shift Keying"). We can detect this in GNU Radio by measuring the magnitude of the signal:</p>

<p>The "Complex to Mag" converts a complex IQ signal to a simple magnitude. Because in our sample our signal is so strong, and we've filtered it so that only our signal is being looked at, we can do a simple magnitude calculation on it. If we had other stray signals still in the sample, we wouldn't be able to take the sample route.</p>

<p>Notice how the "Out" tab of the "Complex to Mag" block is orange instead of blue - this indicates the output is a Float, and that blocks which connect to this must expect a float.</p>

<p>We can set the scope sink to expect a float via the "Type" dropdown menu.</p>

<p>Running our GRC now gives us the scoped output of the magnitude of the signal. Using the mousewheel to scroll out the scope time range a little, we see:</p>

<p>Which looks gratifyingly like the signal we saw in baudline back in the beginning, only we've constructed it by isolating our desired signal, focusing in on it, and converting it to magnitude counts.</p>

<p>You can get the sample data file here, and the GRC file here.</p>

<p>Next post, we'll focus on taking this signal from GRC and processing it back into data.</p>

<p>Thanks to Mike Ossmann, everyone in #hackrf on Freenode, and everyone who helped proof-read this and offered advice on the post.</p>
