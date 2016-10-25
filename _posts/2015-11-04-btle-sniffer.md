---
ID: 1106
title: "基于HackRF的低功耗蓝牙(BTLE) Packet Sniffer/Scanner"
author: jxj
date: 2015-11-04 14:57:42
layout: post
published: true
views:
  - "5534"
duoshuo_thread_id:
  - "6213186720439993089"
---
（原始博文链接：<a href="http://sdr-x.github.io/BTLE-SNIFFER/">http://sdr-x.github.io/BTLE-SNIFFER/</a>）

## 最终更新

HACKRF BTLE packet sniffer已可以像TI（德州仪器）的一样，根据初始广播建链信息自动开始跟踪跳频数据信道！HACKRF切换时间很快，即使通过USB控制，也能达 到几ms内完成（&lt;8ms）。至此有了一个开源的BTLE sniffer！  <a href="https://github.com/JiaoXianjun/BTLE">https://github.com/JiaoXianjun/BTLE</a>

和TI的packet sniffer建链跳频捕捉对比：

![](http://sdr-x.github.io/media/cap-freq-hopping.png)

HACKRF BTLE sniffer 捕捉跟踪BTLE跳频链路的原理，其实不复杂。如图。

![](http://sdr-x.github.io/media/HACKRF-BTLE-sniffer.png)

## 再次更新

终于搞定了 BTLE packet sniffer/scanner btle_rx 对于跳频业务信道的跟踪。
观察了几个iPhone的跳频链路，跳频间隔都是30ms的，对于hackrf来说挺慢的，都能顺利跟踪解调并输出正确的跳频包信息。

我用自己的btle_tx模拟了更快的8ms跳频间隔的链路，hackrf btle_rx也能跟上。也就是说就算用电脑通过usb控制hackrf，频率切换也能做到至少8ms这么快（实际上肯定要更快一些）

下面是捕捉到的建链信息以及跳频起始，和TI的比对：

![](http://sdr-x.github.io/media/cap-freq-hopping.png)

新加了一些选项，比如强制监听某频率，比如基于独特字的包检测更灵活了。有了这两个选项，可以方便的用于监听其他类似协议。比如ANT+


	-f --freq_hz

	This frequency (Hz) will override channel setting (In case someone want to work on freq other than BTLE. More general purpose).

	-m --access_mask

	If a bit is 1 in this mask, corresponding bit in access address will be taken into packet existing decision (In case someone want a shorter/sparser unique word to do packet detection. More general purpose).

	-o --hop

	This will turn on data channel tracking (frequency hopping) after link setup information is captured in ADV_CONNECT_REQ packet.


## 新增

HACKRF BTLE sniffer. 之前的版本只支持37/38/39广播信道sniff，现在加入数据信道支持。可以手工指定数据信道，接入地址，CRC初始化值等建链参数去监听。

加入Verbose选项和raw选项。raw选项下，只要检测到指定前导字，则后续解调的42字节原始内容打印出来。利用btle_tx和btle_rx的RAW选项，HACKRF之间就容易去互动通信了。

## 原文

玩低功耗蓝牙的朋友一定对TI的CC2540 dongle不陌生，它可以抓取空中的BTLE的包。现在HACKRF也可以了！

演示视频：<a href="https://vimeo.com/144574631">https://vimeo.com/144574631</a>

这个视频演示了iPhone关开蓝牙时发出的一些包同时被HACKRF packet sniffer (画面上半部分)和TI的packet sniffer (画面下半部分，虚拟机内）捕捉。内容和数量都是一样的。

主要的新特性 （参见release note：<a href="http://sdr-x.github.io/BTLE-SNIFFER/">http://sdr-x.github.io/BTLE-SNIFFER/</a> ）：

1. `btle_rx`支持BTLE packet sniffer/scanner. 采用全定点快速算法实现低延迟实时处理。实测下来，接收的处理时延大约4ms，加上最坏情况下的buffer时延4ms，发射时延也可以在10ms以下，总的说来跟上一些慢速跳频的BTLE链路还是有希望的。实际观察很多设备建立BTLE链路时协商的跳频速度为30ms。因此为了玩BTLE保证收发跳频切换也许不用全部移植到板上的MCU中，通过USB也是可以的。

2. 新增`Discovery`包，可用于btle_tx在你的手机App（比如LightBlue）上显示任意设备名字和service. (其实我的这个ADS-B BTLE 中继就用了这个技巧  <a href="http://sdr-x.github.io/使用单HACKRF板接收ADS-B信息并通过BTLE发至手机/">http://sdr-x.github.io/使用单HACKRF板接收ADS-B信息并通过BTLE发至手机/</a>

3. Some bugs are fixed.

4. 注意！！！为了支持实时低延迟处理，你需要改hackrf driver：hackrf.c:

 - `lib_device->transfer_count` 变为 4
 - `lib_device->buffer_size` 变为 4096

然后重新编译安装driver，再编译这个BTLE sniffer.

除了演示视频也测试了极限性能，从空中无间隙的连续灌BTLE的包，TI的packet sniffer和HACKRF可以看到效果相同：


![](http://sdr-x.github.io/media/mine-btle-sniffer2.png)

![](http://sdr-x.github.io/media/TI3.png)

