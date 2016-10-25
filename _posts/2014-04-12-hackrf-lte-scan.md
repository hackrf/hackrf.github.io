---
ID: 648
title: "HackRF 扫描LTE基站 支持中国的TDD-LTE"
author: scateu
date: 2014-04-12 10:00:18
post_excerpt: ""
layout: post
published: true
views:
  - "9505"
duoshuo_thread_id:
  - "1312073613704167495"
---
LTE-Cell-Scanner的TDD功能支持作者<a href="http://sdr-x.github.io">jxj同学</a>目前完成了该软件对于HackRF的支持。

据作者说HackRF的效果明显比rtlsdr要好不少，噪声系数彽了很多。

技术细节见<a title="为LTE小区搜索程序加入HackRF支持（使用纯C/C++语言操作HackRF）" href="http://www.hackrf.net/2014/04/lte-hackrf/">为LTE小区搜索程序加入HackRF支持（使用纯C/C++语言操作HackRF）</a>

&nbsp;

以下是jxj同学发布的新闻:

<strong>LTE小区搜索软件加入对HACKRF的支持，有望将来解调LTE SIB信息</strong>。

感谢 <a href="http://hackrf.net/" target="_blank">http://hackrf.net/</a> 借给我HackRF板供调试！

OpenCL加速的 TDD/FDD LTE小区搜索与跟踪源代码: <a href="https://github.com/JiaoXianjun/LTE-Cell-Scanner" target="_blank">https://github.com/JiaoXianjun/LTE-Cell-Scanner</a>

因为HackRF带宽可达20MHz，远高于rtl-sdr电视棒，因此有了HackRF，将来就可能加入解码LTE SIB信息的功能（程序原来主要针对rtl-sdr电视棒设计，受限于rtl-sdr电视棒带宽，只能解码LTE MIB）

<strong>视频演示</strong>
（国内） <a href="http://v.youku.com/v_show/id_XNjk3Mjc1MTUy.html" target="_blank">http://v.youku.com/v_show/id_XNjk3Mjc1MTUy.html</a>
（国外） <a href="http://www.youtube.com/watch?v=3hnlrYtjI-4" target="_blank">http://www.youtube.com/watch?v=3hnlrYtjI-4</a>

<iframe width="510" height="498" src="http://player.youku.com/embed/XNjk3Mjc1MTUy" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

<strong>历史介绍链接 </strong>
<a href="http://sdr-x.github.io/OpenCL%E5%8A%A0%E9%80%9FLTE%E5%B0%8F%E5%8C%BA%E6%90%9C%E7%B4%A2%28rtl-sdr%E7%94%B5%E8%A7%86%E6%A3%92%29,%20%E5%8D%8A%E7%A7%92%E6%89%AB%E4%B8%80%E4%B8%AA%E9%A2%91%E7%82%B9%28OpenCL%20accelerated%20LTE%20Cell%20Scanner%29/"> OpenCL加速LTE小区搜索</a>
<a href="http://sdr-x.github.io/China-Beijing-LTE-Cell-List/">一些LTE小区扫描结果</a>
<a href="http://sdr-x.github.io/4G%20LTE%28%E5%8C%97%E4%BA%AC%29%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%28rtl-sdr%20%E7%94%B5%E8%A7%86%E6%A3%92%E8%BD%AF%E4%BB%B6%E6%97%A0%E7%BA%BF%E7%94%B5%29%E6%96%B0%E6%89%AB%E5%88%B0%E7%94%B5%E4%BF%A1%28LTE%20SDR%20Beijing%29/"> 最初捕捉LTE频谱1</a>, <a href="http://sdr-x.github.io/%E5%9C%9F%E6%B3%95again%20LTE%20D%E9%A2%91%E6%AE%B5%282500~2690MHz%29%E4%B8%89%E5%A4%A7%E8%BF%90%E8%90%A5%E5%95%86TD-LTE%E9%A2%91%E8%B0%B1%E5%85%A8%E6%8A%93%28TD-LTE%20Beijing%20capture%20scan%29/">最初捕捉LTE频谱2</a>
