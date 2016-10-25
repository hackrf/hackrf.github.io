---
ID: 1091
title: "“hackrf+电池”独立中继1090MHz ADS-B信息经BTLE链路至手机"
author: jxj
date: 2015-08-31 20:22:03
post_excerpt: ""
layout: post
published: true
views:
  - "3244"
duoshuo_thread_id:
  - "1312073613704167539"
---
原始博文链接：<a href="http://sdr-x.github.io/%E4%BD%BF%E7%94%A8%E5%8D%95HACKRF%E6%9D%BF%E6%8E%A5%E6%94%B6ADS-B%E4%BF%A1%E6%81%AF%E5%B9%B6%E9%80%9A%E8%BF%87BTLE%E5%8F%91%E8%87%B3%E6%89%8B%E6%9C%BA/"><strong class="final-path">使用单HACKRF板接收ADS-B信息并通过BTLE发至手机</strong></a>

(本文提到的固件也可以这里下载: <a href="https://github.com/JiaoXianjun/ADS-B-BTLE-air-relay-HACKRF-firmware">https://github.com/JiaoXianjun/ADS-B-BTLE-air-relay-HACKRF-firmware</a> )

(为了降低风险，转发至手机的的经纬度信息已经人为降级)

写了一个hackrf固件，可以把接收的ADS-B (1090MHz) 包解析后，信息通过BTLE (2.4GHz) 链路广播. 这时你用手机就能查看飞机信息了. (比如用这个APP LightBlue。APP store下载). ( 优酷视频

(<a href="http://v.youku.com/v_show/id_XMTI4MjY2NDc0OA==.html">http://v.youku.com/v_show/id_XMTI4MjY2NDc0OA==.html</a>),

youtube视频 (<a href="https://youtu.be/MqX74sk-sa4">https://youtu.be/MqX74sk-sa4</a>))

![]({{ site.imageurl }}/2015/08/adsb-btle-air-relay.png)

两种尝试我的固件的方法:
<ul>
	<li><em><strong>####1. 把固件临时加载进hackrf的RAM并运行</strong></em></li>
</ul>
(hackrf掉电或者重启后会丢失此固件，因此不会影响原有固件)

下载固件： <a href="https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/adsb-btle-air-relay.dfu">https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/adsb-btle-air-relay.dfu</a>

下载和安装dfu-util:

git clone git://gitorious.org/dfu-util/dfu-util.git

也放了一个dfu-util在这里 <a href="https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/dfu-util.tar.gz">https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/dfu-util.tar.gz</a>

启动HackRF One 至 DFU 模式：在上电或者重启hackrf时按下 DFU键不放. 在 3V3 LED 亮起后释放DFU键. ( 详细参见 <a href="https://github.com/mossmann/hackrf">https://github.com/mossmann/hackrf</a> )

加载我的固件进RAM并且运行:

sudo dfu-util --device 1fc9:000c --alt 0 --download adsb-btle-air-relay.dfu

打开LightBlue APP, 你将会看到类似下面的飞机信息:

ICAO-addr/Flight   altitude   speed   latitude   lontitude
<ul>
	<li><em><strong>####2. 把我的固件刷进hackrf的flash固化</strong></em></li>
</ul>
(掉电也不会丢失，但会冲到你原有的固件，所以你可能将来需要刷回原有固件)

下载 adsb-btle-air-relay.bin (<a href="https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/adsb-btle-air-relay.bin">https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/adsb-btle-air-relay.bin</a>)

进行下一步之前，请确保你的hackrf正在跑原始固件, 然后：

hackrf_spiflash -w adsb-btle-air-relay.bin

hackrf_spiflash 工具应该已经随你的hackrf驱动安装了 <a href="https://github.com/mossmann/hackrf">https://github.com/mossmann/hackrf</a>. 我也放了一个hackrf_spiflash在这里  <a href="https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_spiflash">https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_spiflash</a>

运行时, TX led (红) 应该闪动的很快. 如果没有闪, 你可能需要把板子启动到DFU模式，再按一下reset键，可能就跑起来了.
<ul>
	<li><em><strong>####* 如何刷回hackrf原始固件</strong></em></li>
</ul>
因为我的固件不支持和hackrf_spiflash工具通信, 所以你需要先把hackrf原始固件以DFU方式刷进RAM跑起来（这时就能和hackrf_spiflash正常通信了）, 然后再用hackrf_spiflash工具把hackrf原始固件刷进flash固化.

进DFU模式，然后：

dfu-util --device 1fc9:000c --alt 0 --download hackrf_usb.dfu

我也放了一个[hackrf_usb.dfu]在这里(<a href="https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_usb.dfu">https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_usb.dfu</a>))

注意：此时千万不要重启你的板子。

再刷原始固件进板子的flash固化:

hackrf_spiflash -w hackrf_usb.bin

我也放了一个[hackrf_usb.bin]在这里(<a href="https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_usb.bin">https://github.com/sdr-x/sdr-x.github.io/blob/master/_resource/hackrf_usb.bin</a>))

如果没有错误，你的板子就恢复了原始固件.

想知道如何编译固件和刷新固件的详细信息，可以参见<a href="https://github.com/mossmann/hackrf">https://github.com/mossmann/hackrf </a>（firmware目录）
