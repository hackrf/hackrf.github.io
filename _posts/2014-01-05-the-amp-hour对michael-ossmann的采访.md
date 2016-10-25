---
ID: 153
title: "The Amp Hour对Michael Ossmann的采访"
author: scateu
date: 2014-01-05 03:08:38
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167439"
views:
  - "1582"
---
<a href="http://www.theamphour.com/the-amp-hour-161-gifted-grimgribber-grokker/">原文 </a>

Welcome, <a href="http://twitter.com/michaelossmann" target="_blank">Michael Ossmann</a>!
<ul>
	<li>Michael’s open source company is called <a href="http://greatscottgadgets.com" target="_blank">Great Scott Gadgets</a>.</li>
	<li>He got started in software and IT security work.</li>
	<li>It was only 4 years ago that he got back into and started building electronics. His first big project was the Ubertooth Zero.</li>
	<li>This was designed with <a href="https://dominicspill.com/" target="_blank">Dominic Spill</a>, one of the authors of the paper that inspired the device. It can discover “non-discoverable” bluetooth devices.</li>
	<li>This turned into <a href="http://www.kickstarter.com/projects/mossmann/ubertooth-one-an-open-source-bluetooth-test-tool" target="_blank">the Ubertooth One, a successfully funded Kickstarter project</a>.</li>
	<li>Michael was inspired in hardware by people like <a href="http://www.grandideastudio.com/" target="_blank">J</a><a href="http://www.grandideastudio.com/" target="_blank">oe Grand</a>, <a href="http://travisgoodspeed.blogspot.com/" target="_blank">Travis Goodspeed</a>, <a href="http://makezine.com/2010/10/28/hardware-will-cut-you-presentation/" target="_blank">Amanda Wozniak</a>. All doing security work and designing cool conference badges!</li>
	<li>Started out looking through sparkfun beginning electronics tutorial</li>
	<li>Michael also works with <a href="http://www.sharebrained.com/" target="_blank">Jared Boone</a>, OSHW developer.</li>
	<li>Designed the UberTooth Zero in EAGLE.</li>
	<li>Manufactured in Shanghai based on a recommendation.</li>
	<li>Most recently, <a href="http://greatscottgadgets.com/hackrf/" target="_blank">the Hack RF Project</a> has caught everyones attention as an SDR that goes from 30 MHz to 6 GHz for less than $300, both TX and RX! <a href="http://www.kickstarter.com/projects/mossmann/hackrf-an-open-source-sdr-platform" target="_blank">Funded nearly $550,000 on Kickstarter</a>.</li>
	<li>Works with existing software like <a href="http://gnuradio.org/" target="_blank">GNU radio</a> (which helps you program SDRs in C++ or Python) and <a href="http://sdrsharp.com/" target="_blank">SDRsharp</a>.</li>
	<li>Everything that Great Scott Gadgets does is open source hardware and software. So is the layout program (KiCAD).</li>
	<li><a href="http://gnuradio.org/redmine/projects/gnuradio/wiki/GNURadioCompanion" target="_blank">GNU radio companion</a> is a graphical tool to get beginners started.</li>
	<li>The HackRF Jawbreaker (prototype/beta unit) has the <a href="http://www.nxp.com/products/microcontrollers/cortex_m4/series/LPC4300.html" target="_blank">LPC43xx</a> as its main micro. Chosen for the highly configurable SDGPIO. Also has a small CPLD on board.</li>
	<li>Regardless of not having an FPGA on board, it can still do 1000 FFTs per second and stream lots of data back to PC for processing.</li>
	<li>The power on board is limited by design; this reduces cost and stays under the radar (sic) for the FCC. Regardless, it still has 10 dB of front end gain.</li>
	<li><a href="http://dangerousprototypes.com/2013/07/02/daisho-software-multitool-for-phy-layer-monitoring/" target="_blank">The newest project is called </a><a href="http://dangerousprototypes.com/2013/07/02/daisho-software-multitool-for-phy-layer-monitoring/" target="_blank">Daisho</a>. It’s an open, high speed man-in-the-middle protocol analyzer</li>
	<li>Includes super speedy standards like USB 3.0 and GB ethernet. The implementation is similar to the <a href="http://www.bunniestudios.com/blog/?p=2133" target="_blank">NETV by Bunnie</a>, but that can only do 1080i.</li>
	<li><a href="http://ossmann.blogspot.com/2013/05/introducing-daisho.html" target="_blank">Marshall Hecht is doing a lot of the design for that</a>. The USB 2.0 stuff already is up and running.</li>
	<li>Had one kickstarter that wasn’t successful, <a href="http://www.kickstarter.com/projects/mossmann/firefly-cap" target="_blank">the </a><a href="http://www.kickstarter.com/projects/mossmann/firefly-cap" target="_blank">Firefly cap</a>. It used an energy harvesting circuit, which priced it out of the perceived hobbyist market. Michael liked that KS showed the project was not viable in the market.</li>
	<li>Everything is open source and tracked on wikis, GitHub and standard open tools.</li>
	<li>One “designer” for the hardware, but they are using Git for design reviews (awesome!).</li>
</ul>
For more info, check out Great Scott Gadgets or <a href="http://ossmann.blogspot.com" target="_blank">Michael’s Blog</a>. He’s also on twitter under the handle <a href="http://twitter.com/MichaelOssmann" target="_blank">@MichaelOssmann</a>.

<strong> And in an Amp Hour first, Michael wrote in with even more…stuff he forgot to mention, links and answers to questions on the subreddit that weren’t fully answered! Wow!</strong>
<blockquote>Here are a few things from the “How did I talk for over an hour and not
mention that?” department that might be good to include in the show
notes:

The person who has published some information about using HackRF with
Remote Keyless Entry systems is dragorn (who introduced me to Chris):
<a href="http://blog.kismetwireless.net/2013/08/playing-with-hackrf-keyfobs.html" target="_blank">http://blog.kismetwireless.net/2013/08/playing-with-hackrf-keyfobs.html</a>

One of the more exciting aspects of the HackRF Project was that I gave
away 500 Jawbreakers for beta testing. As far as I know, this was the
largest ever give-away of open source hardware:
<a href="http://ossmann.blogspot.com/2013/05/giving-away-hackrf.html" target="_blank">http://ossmann.blogspot.com/2013/05/giving-away-hackrf.html</a>

I’m adapting my two day SDR class into a free online video series. It
will consist primarily of lectures with a whiteboard and demonstrations
and exercises using GNU Radio Companion:
<a href="http://www.kickstarter.com/projects/mossmann/hackrf-an-open-source-sdr-platform/posts/563588" target="_blank">http://www.kickstarter.com/projects/mossmann/hackrf-an-open-source-sdr-platform/posts/563588</a>

At this point it is very safe to say that HackRF will be available for
sale post-Kickstarter. If people want to be notified about this and
other Great Scott Gadgets happenings, they can sign up for the
GSG-announce mailing list:
<a href="http://four.pairlist.net/mailman/listinfo/gsg-announce" target="_blank">http://four.pairlist.net/mailman/listinfo/gsg-announce</a>
<em>And here are some answers to questions I see on reddit that we didn’t</em>
<em> get to:</em>

oojingoo asked about tracking Bluetooth devices “idle in one’s pocket”
with Ubertooth: The primary use of Ubertooth One is passive monitoring
of Bluetooth communications. If a Bluetooth device is idle and is not
currently connected to any paired device, it may be totally silent such
that an Ubertooth One would not detect any packets. Such a device may
(as long as its Bluetooth function isn’t switched off) periodically
attempt to contact paired devices to see if they are available, and
those connection attempts are observable with Ubertooth. More about
tracking people with Ubertooth can be found on the Ubertooth blog:
<a href="http://ubertooth.blogspot.com/2012/11/so-you-want-to-track-people-with.html" target="_blank">http://ubertooth.blogspot.com/2012/11/so-you-want-to-track-people-with.html</a>

itdnhr asked about HackRF support in software other than GNU Radio: My
primary software goal for HackRF is to maintain strong support in GNU
Radio. Beyond that, we maintain libhackrf, a cross-platform software
library that anyone can use to add HackRF support to their software.

A few people asked about the future of Great Scott Gadgets and HackRF: I
see HackRF being an active project in something like its current form
for a long time to come. One of the potential uses for Daisho beyond
wired communication applications is SDR. We don’t have any Daisho
front-end modules for SDR yet, but we probably will eventually. In
particular I’m interested in being able to do SDR with much higher RF
bandwidth (100 MHz or more) than we can achieve with USB 2.0. So I’ll
likely have some USB 3.0 SDR stuff based on Daisho, and HackRF will
remain the lower cost, more portable, USB 2.0 solution.

codebudo asked if HackRF can be used for ham radio: Certainly. Several
of the HackRF beta testers already use HackRF for operation in various
amateur bands. It is very easy to use HackRF for receiving such
transmissions with software like Gqrx or SDR#. HackRF can transmit too,
but you’ll likely need external amplification and filtering as we
discussed on the show.

drabanus asked about HackRF applications including the possibility of
measuring distance with a moon bounce: Wow! It would be so cool if
someone did a moon bounce with HackRF! You would probably need some
large antennas and external amplification and filtering to do it. One
of the things I find so exciting about HackRF is that people can use it
for things I never even imagined. It has already been used to receive
weather satellite images, track automotive tire pressure monitors
(TPMS), experiment with remote keyless entry system security, monitor
GSM communications, listen to wireless microphone transmissions, control
radio controlled toys, and more.</blockquote>
