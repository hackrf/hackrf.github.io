---
ID: 297
title: "HackRF演进计划"
author: scateu
date: 2014-03-16 12:31:10
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167469"
views:
  - "13027"
---
开源硬件的好处之一便是，每个人都是作者，每个人都可以对它进行改进 除了使用HackRF，我们还有许多对于HackRF可以贡献的改进。

真正的发挥HackRF社区的力量，推进SDR普及的进程。

我们已经加入了2014年CSDN<a href="http://www.hackrf.net/2014/06/csdn_os_camp/">开源夏令营</a>活动，CSDN将会对为HackRF.net软件无线电社区做出贡献的同学提供5000元的奖励，欢迎大家申请。

以下是我们现在可以对HackRF做出的改进:
<h3>应用</h3>
<ul>
	<li>给 Debian / Ubuntu 等各Linux发行版进行GNURadio和HackRF的打包</li>
	<li>文档中文化 / 发展中文社区</li>
	<li>对于build-gnuradio脚本的国内镜像: 已由cuckoo<a href="http://www.hackrf.net/2014/04/gnuradio%E6%95%99%E8%82%B2%E7%BD%91%E9%95%9C%E5%83%8F%E5%9B%BD%E5%86%85%E9%95%9C%E5%83%8F/">完成</a></li>
	<li>制作Nightly-Build的LiveCD / LiveUSB:
<ul>
	<li>由于GNURadio和HackRF的代码经常会更新，所以需要LiveCD的自动生成脚本</li>
	<li>由HackRF.net讨论组里的大侠(QQ:124657440)牵头完成</li>
</ul>
</li>
	<li>VMWare等虚拟机试用镜像，让更多的同学能第一时间试用
<ul>
	<li>目前由citypw提供的<a href="http://www.hackrf.net/2014/03/%E6%B5%8B%E8%AF%95hackrf-one%E7%9A%84ubuntu%E9%95%9C%E5%83%8F/">镜像</a></li>
</ul>
</li>
	<li>积极参与邮件列表的讨论
<ul>
	<li><a href="mailto://hackrf-cn@googlegroups.com">hackrf-cn@googlegroups.com</a></li>
</ul>
</li>
	<li>参加GSoC(Google Summer of Code)，为HackRF提供改进</li>
	<li>Javascript版的GRC文件渲染器，方便大家分享GRC框图</li>
	<li>基于HackRF的LTE TDD信号发生实验</li>
	<li>Windows中SDRSharp对于HackRF One的支持</li>
	<li>加入低噪声放大器</li>
</ul>
<h3>硬件/固件/驱动</h3>
<ul>
	<li>Firmware改进，实现上电自动开机，不需要再按Reset</li>
	<li>替换HackRF的MAX5864芯片增大采样率</li>
	<li>实现HackRF的全双工</li>
	<li>射频算法的改进
<ul>
	<li>可变中频算法 <a href="https://github.com/mossmann/hackrf/issues/109">参阅Issues</a></li>
	<li>DC Offset 的修复</li>
</ul>
</li>
</ul>
