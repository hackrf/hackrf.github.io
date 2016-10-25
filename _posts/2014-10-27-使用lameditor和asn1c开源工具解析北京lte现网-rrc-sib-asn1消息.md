---
ID: 951
title: "使用lameditor和asn1c开源工具解析北京LTE现网 RRC SIB ASN1消息"
author: jxj
date: 2014-10-27 15:51:21
post_excerpt: ""
layout: post
published: true
views:
  - "5395"
duoshuo_thread_id:
  - "1312073613704167529"
---
<span class="f000">主要参考了这篇blog：
<a href="http://blog.csdn.net/peng_yw/article/details/22437251" target="_blank">http://blog.csdn.net/peng_yw/article/details/22437251</a></span>

本工作是这个LTE小区搜索项目的一部分： <a href="https://github.com/JiaoXianjun/LTE-Cell-Scanner" target="_blank">https://github.com/JiaoXianjun/LTE-Cell-Scanner</a>  一些介绍参见：<a title="LTE扫描相关文章" href="http://sdr-x.github.io" target="_blank">http://sdr-x.github.io</a><!--more-->

(关于一些基本的软件无线电概念，Linux操作，代码库git操作，以及软件包安装（apt-get）等，参见：<a href="http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B(tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes)/">这篇文章</a>)

<span class="f000">
解析RRC SIB ASN1消息的程序已经封装为一个matlab函数parse_SIB()，并用在这个Matlab/LTE_DL_receiver.m 脚本中，用来将PDSCH信道解调出来的SIB原始比特正确解析为SIB消息。成功解析的北京4G现网的三个SIB消息，见三个附件。</span>

具体方法：

1. 根据LTE RRC 的spec生成ASN1描述文档

从这里：
<a href="http://www.3gpp.org/ftp/Specs/archive/36_series/36.331/" target="_blank">http://www.3gpp.org/ftp/Specs/archive/36_series/36.331/</a>
下载36.331协议文档。
把36331-ac0.zip解压缩，得到36331-ac0.doc
把36331-ac0.doc另存为36331-ac0.txt.

从这里
<a href="http://sourceforge.net/projects/lameditor/" target="_blank">http://sourceforge.net/projects/lameditor/</a>
下载lameditor工具，并编译安装好。

然后执行：
<pre class="lang:default decode:true ">cd lameditor-1.0/src/getasn/</pre>
把36331-ac0.txt拷贝到上述目录，然后运行：
<pre class="lang:default decode:true ">./getasn 36331-ac0.txt</pre>
至此，得到LTE协议的ASN1描述文件36331-ac0.asn

2. 为LTE协议解析生成ASN1解码器

从这里：
<a href="https://github.com/vlm/asn1c" target="_blank">https://github.com/vlm/asn1c</a>
获取asn1c工具，并编译安装。

然后执行：
<pre class="lang:default decode:true ">cd asn1c/examples/
mkdir sample.source.LTERRC
cd sample.source.LTERRC</pre>
把第一步里得到的36331-ac0.asn拷贝到目录sample.source.LTERRC，然后执行：
<pre class="lang:default decode:true ">asn1c  -S /usr/local/share/asn1c -fcompound-names -fskeletons-copy -gen-PER -pdu=auto 36331-ac0.asn</pre>
得到编译工具所需的源文件。在正式编译我们所需的工具之前，需要对源文件做如下修改：

文件converter-sample.c:
在#include &lt;asn_internal.h&gt;后面，加入：
#define PDU BCCH_DL_SCH_Message
#define ASN_PDU_COLLECTION

文件per_opentype.c:
在ASN_DEBUG("Too large padding %d in open type", (int)padding);后面，加入：
padding = padding % 8;
并且注释掉：_ASN_DECODE_FAILED;

现在编译解码工具，执行：
<pre class="lang:default decode:true ">make -f Makefile.am.sample</pre>
得到解码工具progname

3. 解码工具的使用。

执行：
<pre class="lang:default decode:true ">./progname recv_bits.per -p BCCH-DL-SCH-Message</pre>
recv_bit.per即收到的PDSCH上的SIB的原始bit存成的二进制文件。-p用来指定消息类型，PDSCH上的SIB的消息类型为：BCCH-DL-SCH-Message

运行命令之后，会打印出解析出来的SIB消息各个字段的名称、内容。

一些从北京现网抓取的SIB原始bit文件（per）和解析后的SIB消息（txt）文件，供参考：<a title="北京现网抓取的SIB原始bit per文件以及解析的txt文件" href="http://sdr-x.github.io/%E4%BD%BF%E7%94%A8lameditor%E5%92%8Casn1c%E5%BC%80%E6%BA%90%E5%B7%A5%E5%85%B7%E8%A7%A3%E6%9E%90%E5%8C%97%E4%BA%ACLTE%E7%8E%B0%E7%BD%91%20RRC%20SIB%20ASN1%E6%B6%88%E6%81%AF/" target="_blank">文件链接在此</a>。

&nbsp;
