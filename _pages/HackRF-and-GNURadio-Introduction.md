---
ID: 206
permalink: /HackRF-and-GNURadio-Introduction/
title: "《入门》"
author: scateu
date: 2014-03-11 09:40:44
post_excerpt: ""
layout: page
published: true
duoshuo_thread_id:
  - "1312073613704167457"
views:
  - "49467"
---
<em>本文正在写作中</em>

[toc]
<h1>引言</h1>
近年来，软件工程师的触角早已经慢慢地向传统硬件工程师的保留领地里伸去。例如FPGA的出现使得硬件设计从原来的"原理图-&gt;PCB图-&gt;制板-&gt;焊接-&gt;调试-&gt;改板-&gt;制板-&gt;焊接"的长周期、慢节奏开发中解放出来。缩短了硬件开发的周期，等同于延长了硬件工程师的寿命。
<h2>软件行业的先进内容</h2>
软件行业中一些先进的理念正在被引入硬件设计中,例如版本控制。在软件行业中，我们使用如SVN和GIT等工具对软件代码进行管理，基本地思想是，将程序员每次对代码的改动，都做为一个版本提交到版本库中，不同的版本之间可以进行对比和合并。

而硬件设计中，由于大部分EDA软件不提供该功能，使得硬件工程师只能人工地管理电路的原理图和PCB图，经常出现版本混乱的情况。

开源的电路设计软件如KiCAD将所有的原理图和PCB图以纯文本的方式保存，这样硬件设计过程中也可以使用版本控制系统对每一次改动进行跟踪。
<h2>开源, GPL</h2>
开源协议中最著名的是GPL(GNU Public License)。GPL最美妙的地方在于，你使用了或者说是“抄袭”了GPL代码并没有关系，但是只要你用了GPL的代码，你就必须要把你基于此做出的成果也按照GPL协议发布出来。

而GPL之外的软件世界，大家都在小心地维护着不被珍视的版权，敝帚自珍。开源的思想是鼓励你把你的作品发布出来，勇于接受其它的人学习以及批评。

在GPL等开源协议的引领下，开源软件世界已经形成了一整套有生命力、有活力的软件工具体系。Linux的成功证实了一点。
<h2>无线电</h2>
同样，在无线电这一发展了一个多世纪的行业中，软件设计模式的加入，使得近年来无线电的玩法出现了新的变化。人们不再需要为一种特定的调制解调方案而单独地设计对应的硬件，并耗巨大的精力和时间对硬件各部分进行调优。

于是，我们想到可以用数字的方法，以不变应万变的形式，使用一块射频前端板卡，将无线信号变频到基带部分，然后将全部带宽采样回电脑，所有原来由特定硬件来实现的功能如混频器、滤波器、放大器、衰减器都变成了计算机中的运算。
<h2>为什么我们需要开源软件无线电?</h2>
无线电行业已经逐渐被安捷伦(是德科技)和罗德施瓦茨等巨头公司垄断，他们造出一块又一块仪表，写出一摞又一摞让人看不懂的通信标准，把好好的功能拆分成一个又一个的选件(Option)来卖钱。如同早期玩电脑的人可以接触一些命令行、了解一些电脑的内部原理，现在玩电脑的人早就被360等公司包围的严严实实，让你无暇也无力去了解电脑及互联网的任何原理。

GNURadio就是开源世界中软件无线电的代表项目。它的出现，使得开源世界能够打破传统通信巨头的垄断，使得人们能够自由地了解整个通信系统的任何细节。

GNURadio实现了软件无线电所需要的大部分模块，并且完成了对于采样数据流的缓冲、调度，并由开源社区集体维护。值得一提的是，GNURadio不同于MATLAB等旨在仿真的工具，它生来就是准备玩真的，GNURadio对于软件无线电射频前端硬件的支持非常全面，例如USRP、HackRF、BladeRF等。

这篇<a href="http://www.taylorkillian.com/2013/08/sdr-showdown-hackrf-vs-bladerf-vs-usrp.html">文章</a>仔细地分析了三者的区别和优缺点。<a href="http://blog.sina.com.cn/s/blog_72628e9f0101c25g.html">这里</a>给了一个中译版。总的说来，USRP B2X0 系列价格最贵、性能最好；bladeRF 支持频段没有那么宽，但优点在于可以脱机运行；HackRF 价格低廉、开放程度高，最适合黑客玩家。

特别值得一提的是，HackRF是完全开源的软件无线电射频前端。它从原理图到PCB图，从驱动程序到单片机固件，甚至连加工制板所需要的工艺要求，全部以GPL协议无保留地发布，这对于我们学习研究软件无线电无疑是一件非常值得庆幸的事情。

HackRF已经在KickStarter上卖出了2000多块板卡。而BladeRF只卖出了500块板卡。
<h2>结语</h2>
在本书中，笔者试图以一个非通信科班出身的资深的Linux玩家的视角，浅显地解释无线电的一些基本知识，并以HackRF为基础，向大家介绍开源软件无线电框架GNURadio的基本用法及示例。期望有更多的软件工程师能够发现这一块尚未被开垦的大陆，从而为开源软件无线电贡献出更多更好的开源项目。
<h1>Hello, World. 快速上手</h1>
本章试图以几个简单易懂的示例，向大家不经证明地展示一些无线电及信号处理的一些有趣的事实。旨在使读者能够对我们所要研习的领域有一个全面、粗犷而不失准确的认识。

本章假定读者对于Linux有基本的操作能力，能读懂理解基本的Python语言，假定读者对于频率、载波、相位等概念有基本的认识，并接触过示波器等常见工科仪表。
<h2>HackRF的FM广播接收</h2>
<h3>在Windows系统</h3>
<ul>
	<li>首先安装<a href="http://sourceforge.net/projects/libwdi/files/zadig/">Zadig</a></li>
	<li>将HackRF与电脑连接，按一下HackRF的电源/Reset按钮使其开机</li>
	<li>使用Zadig安装其驱动。</li>
	<li>下载最新版本的<a href="http://sdrsharp.com/downloads/sdr-nightly.zip">SDR#</a></li>
	<li>(可选)下载Windows平台的<a href="http://the.midnightchannel.net/sdr/SDRSharp_Plugins/zefie/SDRSharp.HackRF.ZefieMod/hackrf-tools.zip">hackrf-tools</a>
<ul>
	<li>需要安装Microsoft Visual C++ 2012 Redistributable Package</li>
</ul>
</li>
</ul>
TODO: SDR#的截图 TODO: 加入Zadig截图

然后打开SDR#，即可尝试接收FM广播了。
<h3>Linux系统</h3>
参照gqrx.dk上的安装指南
<h4>安装GNURadio</h4>
在之前安装GNURadio是一件非常困难的事情，现在有了sbrac.org提供的GNURadio安装脚本，所有的事情变得非常简单。只需要执行以下几条命令就可以安装好。

特别需要注意的是，由于GNURadio软件包目前被各大发行版的支持还不是特别好，因此完全不建议使用<code>apt-get</code>等方式安装GNURadio。
<pre><code>$ wget http://www.sbrac.org/files/build-gnuradio
$ chmod +x build-gnuradio
$ ./build-gnuradio -v -ja
</code></pre>
然后在<code>Proceed?</code>后面输入<code>y</code>回车。在<code>Do you have SUDO privileges?</code>提示后面依然输入<code>y</code>回车。

注意，执行<code>build-gnuradio</code>脚本的时候，不要预先使用<code>sudo</code>。

加入<code>-ja</code>的选项后，使用多核处理器的并行处理能力来编译GNURadio，加快编译速度。整个编译安装过程可能会持续2小时甚至更长的时间，取决于网络速度。因此建议使用比较快的Linux镜像源服务器。

然后<code>build-gnuradio</code>脚本会自动安装所需要的软件包，然后开始安装。

值得一提的是，<code>build-gnuradio</code>脚本会自动安装最新版本的hackrf软件。
<h4>编译安装gqrx</h4>
<pre><code>$ git clone https://github.com/csete/gqrx.git gqrx.git
$ cd gqrx.git
$ mkdir build
$ cd build
$ qmake ..
$ make
</code></pre>
<h4>LiveCD试用</h4>
如果现在还没有时间和精力来编译安装gqrx甚至GNURadio环境，可以使用社区其它人已经搭建好的<a href="http://files.persona.cc/linux/ubuntu/ubuntu-12.04.2-custom-sdr-amd64.iso">LiveCD</a>引导光盘试用GNURadio。

放入光盘，重启电脑，选择以光盘引导。

打开HackRF的电源按钮。

只需要打开终端，输入<code>hackrf_info</code>命令
<pre><code>$ hackrf_info 
Found HackRF board.
Board ID Number: 2 (Unknown Board ID)
Firmware Version: git-f9ffe90
Part ID Number: 0xbc5f4f4a 0xbc5f4f4a
Serial Number: 0x00000000 0x00000000 0x261c63c8 0x26776d53
</code></pre>
证明HackRF已经被电脑所识别。然后输入
<pre><code>gqrx
</code></pre>
然后选择硬件为HackRF，然后按下左上角的开关按钮，并将HackRF调谐到103.9MHz，不出意外的话，你就可以听到FM广播了。
<pre><code>TODO: gqrx的照片
</code></pre>
<h2>osmocom_fft osmocom_siggen</h2>
使用osmocom_fft 即可开启一个简单的频谱仪

osmocom_siggen 则提供了一个简单的信号发生器
<h2>FM 广播的发射</h2>
随便找一个mp3文件什么的，转换成WAV文件。在此建议转换成44.1kHz，双声道。

例如使用ffmpeg转换:
<pre><code>ffmpeg -i source.mp3 music.wav
</code></pre>
打开gnuradio-companion搭建一个框图

<img src="../img/wbfm_tx.grc.png" alt="" />
<h3>WAV Source</h3>
<ol>
	<li>samp_rate采样率要设置为250kHz，这个与我们的wav文件采样率为44.1kHz有关。实际试验，如果samp_rate设置为500kHz，放出来的声音会加速一倍。</li>
	<li>N Channels表示Wav文件的声道数，填2</li>
	<li>File里填写你在上一步制备的Wav文件地址</li>
</ol>
<h3>Stream Mux</h3>
Stream Mux的作用是把两条数据流合并为一条流，例如N0是来自第一条流的采样点，N1来自第二条流采样点，则Stream Mux会将两条流以如下方式输出:
<pre><code>[N0, N1, N0, N1, N0, N1, ...]
</code></pre>
值得注意的是此处的Type需要填为Float，GNURadio里的数据类型是以颜色表示的。

另外，同一主色的情况下，颜色越深表示它是一个Vector。TODO: 待仔细调查

<img src="../img/GNURadioDataTypes.png" alt="" />
<h3>Osmocom Sink</h3>
在GNURadio里，Sink表示信号输出，Source表示信号输入。
<ul>
	<li>Device Arguments可以填上hackrf=0</li>
	<li>sample rate设置为samp_rate*4</li>
	<li>Ch0 Freq Corr (ppm)
<ul>
	<li>HackRF的频率较正值，在没有经过仪表校正时，可以直接先填0，有条件的同学可以使用频谱仪或信号源进行标定。</li>
	<li>Ch0 Frequency 要发射的频率，此处我填了93e6，表示93MHz</li>
	<li>Ch0 RF Gain(dB)
<ul>
	<li>表示HackRF放大器是否开启</li>
	<li>尽管此处Gain可任意输入，但事实上只有两档，0和14dB，并不是连续可调的，在此我们填14</li>
</ul>
</li>
	<li>Ch0 IF Gain(dB)
<ul>
	<li>表示HackRF的中频增益</li>
	<li>从电路上来看，应试指的是进入MAX2837收发器后给的增益</li>
	<li>在此我们填40</li>
</ul>
</li>
	<li>Ch0 BB Gain(dB)
<ul>
	<li>表示HackRF的基带增益</li>
	<li>可能指是的进入ADC/DAC芯片后给的增益</li>
	<li>在此我们填20</li>
</ul>
</li>
	<li>Ch0 BandWidth，填250e3</li>
</ul>
</li>
</ul>
另外，你自行尝试将上述框图中的<code>Wav File Source</code>改为<code>Audio Source</code>，即可实现语音的实时FM发射。注意要修改一下各个采样率。

<img src="../img/audio_wbfm_tx.grc.png" alt="" />

警告: 请短时间尝试性的操作，切勿长时间干扰正常业务频率。
<h2>遥控小车的重放</h2>
市面上的遥控小车多使用27MHz或者40MHz等频率进行遥控。在此我们在不分析遥控信号的情况下，对信号进行录制和重放，使大家对HackRF的操作有一个简单的认识。

在本书的后续章节，我们会进一步地分析市面上一种常见的遥控小车的遥控器，并使用GNURadio编写信号处理模块，重新生成遥控小车的指令，最后简单实现一个电脑上具有图形界面的遥控小车的控制程序。

<img src="../img/car.png" alt="" />

我们使用内嵌的<code>hackrf_transfer</code>指令来实现录制-回放。首先看一下<code>hackrf_transfer</code>的用法:
<pre><code>$ hackrf_transfer 
receive -r and receive_wav -w options are mutually exclusive
Usage:
    -r &lt;filename&gt; # Receive data into file.
    -t &lt;filename&gt; # Transmit data from file.
    -w # Receive data into file with WAV header and automatic name.
       # This is for SDR# compatibility and may not work with other software.
    [-f set_freq_hz] # Set Freq in Hz between [5MHz, 6800MHz]. 设置中心频率
    [-a set_amp] # Set Amp 1=Enable, 0=Disable. 设置10dB放大器是否打开 
    [-l gain_db] # Set lna gain, 0-40dB, 8dB steps 低噪声放大器增益
    [-i gain_db] # Set vga(if) gain, 0-62dB, 2dB steps VGA放大器(中频)
    [-x gain_db] # Set TX vga gain, 0-47dB, 1dB steps 发射的增益
    [-s sample_rate_hz] # Set sample rate in Hz (8/10/12.5/16/20MHz, default 10MHz). 设置采样率
    [-n num_samples] # Number of samples to transfer (default is unlimited). 传输的采样点数
    [-b baseband_filter_bw_hz] # Set baseband filter bandwidth in MHz. 设置基带滤波器带宽(MHz)
    Possible values: 1.75/2.5/3.5/5/5.5/6/7/8/9/10/12/14/15/20/24/28MHz, default &lt; sample_rate_hz.
</code></pre>
需要注意几点: - 此处的VGA是指Variable Gain Amplifier，指可变增益的放大器。 - 虽然HackRF的宣传文档里写的是支持30MHz到6GHz，但是实际上驱动支持从5MHz到6800MHz
<h3>录制指令</h3>
<pre><code>$ hackrf_transfer -r car.iq -f 27000000 -s 8000000 -i 60
call hackrf_sample_rate_set(8000000 Hz/8.000 MHz)
call hackrf_baseband_filter_bandwidth_set(7000000 Hz/7.000 MHz)
call hackrf_set_freq(27000000 Hz/27.000 MHz)
Stop with Ctrl-C
16.0 MiB / 1.000 sec = 16.0 MiB/second
16.0 MiB / 1.000 sec = 16.0 MiB/second
16.0 MiB / 1.000 sec = 16.0 MiB/second
^CCaught signal 2
 4.2 MiB / 0.273 sec = 15.3 MiB/second

User cancel, exiting...
Total time: 3.27395 s
hackrf_stop_rx() done
hackrf_close() done
hackrf_exit() done
fclose(fd) done
exit
</code></pre>
然后使用玩具小车的遥控器靠近HackRF的天线，操作小车，完毕后按Ctrl-C结束程序

其中，-f参数是指采回的中心频率，在这里的意思是把27.0MHz为中心，共8M带宽的信号，下变频到0Hz为中心频率，并传输回计算机，保存成为car.iq。 <code>-i 60</code>参数是接收中频增益
<h3>重放指令</h3>
<pre><code>$ hackrf_transfer -t car.iq -f 27000000 -s 8000000 -a 1 -l 30 -x 40 
call hackrf_sample_rate_set(8000000 Hz/8.000 MHz)
call hackrf_baseband_filter_bandwidth_set(7000000 Hz/7.000 MHz)
call hackrf_set_freq(27000000 Hz/27.000 MHz)
call hackrf_set_amp_enable(1)
Stop with Ctrl-C
16.0 MiB / 1.000 sec = 16.0 MiB/second
16.0 MiB / 1.000 sec = 16.0 MiB/second
16.0 MiB / 1.000 sec = 16.0 MiB/second
 4.7 MiB / 1.000 sec =  4.7 MiB/second

User cancel, exiting...
Total time: 4.00070 s
hackrf_stop_tx() done
hackrf_close() done
hackrf_exit() done
exit
</code></pre>
注意<code>-a 1</code>打开HackRF的功率放大器，使最终的信号提升10dB。<code>-l</code>参数设置了HackRF的LNA(Low Noise Amplifier,低噪声放大器的增益)，<code>-x</code>参数设置了HackRF的发射中频增益。在此，我们选择这些参数，以使得HackRF的发射功率适当。

然后，你就会发现遥控小车按照你刚刚录制的动作跑起来了。

另外，关于<code>hackrf_transfer</code>的详细情况，可以参考代码<code>hackrf/host/hackrf-tools/src/hackrf_transfer.c</code>。
<h3>宽带录制</h3>
由于HackRF最大支持20MHz的采样率，于是我们可以一次把市面上的27MHz和40MHz的遥控车同时录制。

我们选取35MHz为采样的中心。
<pre><code>hackrf_transfer -r both.iq -f 35000000 -s 20000000 -i 60

hackrf_transfer -t both.iq -f 35000000 -s 20000000 -a 1 -l 30 -x 40 
</code></pre>
<h1>预备知识</h1>
本章试图以浅显而直观地方式简要地介绍在软件无线电中会经常遇到的一些常用名词。

章节末提供了一个使用声卡在示波器上打字的示例，以加深读者的理解。
<h2>CW / 莫尔斯码</h2>
Morse Code，又称CW，是最古老但是也最广泛的无线电通信方式了。由于CW通信方式所占用的带宽小、抗干扰能力强。作为一种信息编码标准，摩尔斯电码拥有其他编码方案无法超越的长久生命。摩尔斯电码在海事通讯中被作为国际标准一直使用到1999年。1997年，当法国海军停止使用摩尔斯电码时，发送的最后一条消息是：所有人注意，这是我们在永远沉寂之前最后的一声呐喊！

如果你想学习莫尔斯码，最简单的方式就是在手机上装一个Morse Code练习软件，例如笔者某天晚上，于学校的操场上一边练习一边散步，走了十几圈后，就可以不加参考地发送莫尔斯码了。

在Linux系统里，xcwcp和gMFSK两个程序可以帮助你做一些简单的莫尔斯码通讯训练。

Google曾经在2012年的愚人节推出了手机的<a href="http://mail.google.com/mail/help/promos/tap/">莫尔斯码输入法</a>。

美国NBC电视台有一期<a href="http://v.youku.com/v_show/id_XMTU1MDc5MDQ=.html">节目</a>，同样一段文字，一组人使用手机发送，另一组人使用FT817便捷式短波发信机通过摩尔斯码自动键进行收发。结果摩尔斯码组明显获胜。

顺便提一句，此刻北京市仍然可以发电报，在出西单地铁站不远，出门右转的联通营业厅。有兴趣的同学可以去体验一下。
<h2>业余无线电</h2>
业余无线电是一种在全世界非常普遍的业余爱好。喜爱业余无线电的人也被称为业余无线电爱好者或HAM，在美国大约有一百多万人，日本大约有三百万人，中国大约有二十万人，在全世界总共大约四百多万人。他们必须学习相关知识并通过所在国家的测试才能领取到业余无线电执照，同时领取政府分配给的呼号。

例如在北京，获取业余无线电执照，需要向北京市无线电协会(www.brsa.org.cn)提交申请，然后参加培训、考试和验机，最后才可以获取操作能力证书和电台执照。

qrz.com提供了全球无线电爱好者的呼号查询服务。

业余无线电爱好者完成一次通联之后，会通过卡片收寄局或邮件方式双方互送QSL卡片。

例如下图即是清华集体业余电台(BY1QH)的台址和通联卡片，摘自<a href="http://qrz.com/db/BY1QH">http://qrz.com/db/BY1QH</a>

<img src="../img/BY1QH.JPG" alt="" /> <img src="../img/BY1QH-QSL.JPG" alt="" />

无线爱好者之间通常使用一种名为Q简语的三字符缩写来提升通信效率。例如: QTH代表台址，QRP代表减小发信机功率。

通联结束后，双方会互致73表达问候。73在摩尔斯码里的表示是--... ...--，是一段非常对仗的声音。

更多关于业余无线电的知识，可以参阅望京集体业余电台(BY1WJ)的网站<a href="http://www.by1wj.com">http://www.by1wj.com</a>
<h3>ISM Band 免执照频段</h3>
<h2>gMFSK</h2>
gMFSK是一个非常强大的Linux平台下的业余无线电调制解调软件。

需要注意的是，由于最近的Linux发行版都使用ALSA做为声卡驱动，而gMFSK目前还只能使用旧的OSS声卡驱动。

但是我们可以使用alsa-oss这个软件包提供的aoss脚本，来为gMFSK伪造出一个<code>/dev/dsp</code>接口出来。
<pre><code>sudo apt-get install alsa-oss

aoss gmfsk
</code></pre>
<h2>DTMF</h2>
以大提琴演奏家杜普蕾(Jacqueline Mary du Pré)为主题的电影《她比烟花寂寞》(英文片名: Hilary and Jackie)中有一个细节，杜普蕾用大提琴发出DTMF声音，拔通了电话。名侦探柯南剧场版第12部《战栗的乐谱》中也有类似的桥段。

DTMF(Dual-Tone Multi-Frequency, 双音多频)的原理是键盘上的所有按键由高音部分(1209Hz, 1336Hz, 1477Hz, 1633Hz)和低音部分(697Hz, 770Hz, 852Hz, 941Hz)
<pre><code>sudo apt-get install multimon

aoss multimon -a DTMF
</code></pre>
然后拿起手机，打开拨号键盘，按下几个声音，然后multimon便可以解析出DTMF。

进一步地，我们使用gnuradio-companion搭建一个简单的声音频谱仪，直接对话筒输入的声音进行FFT。

<img src="../img/DTMF_Audio_FFT_grc.png" alt="" />

当手机发出2的拨号音时的频谱697Hz,1336Hz

<img src="../img/DTMF-2.png" alt="" />

那么，最后我们再用gnuradio-companion搭建一个简单的生成DTMF中2键的框图

<img src="../img/DTMF-2-TX.png" alt="" />

值得一提有两点：
<ul>
	<li>注意各个模块的端口颜色，代表了不同的数据类型。黄色表示float，搭建的时候需要做修改。</li>
	<li>最后加入Multipy Const来减小声音的响度</li>
</ul>
然后先开启multimon，然后再执行我们的框图。multimon便解析出我们生成的2了。

另外，multimon也是一个非常有趣的项目，不止可以完成DTMF的解调，还可以完成AX.25信号的解调。此外，multimon-ng项目为multimon增加了更多的解调模式。
<h2>声卡示波器</h2>
<img src="../img/StartestOnOcilloScopeScreen.png" alt="" />

上图中，我们使用声卡的左右声道输出分别接到示波器的X-Y两路，使得示波器显示了汉字。

示波器的X-Y模式可以理解为，把X路输入的电压值反映为示波器电子枪对于电子横向的偏转，把Y路输入的电压值反映为示波器电子枪对于电子纵向的偏转。 于是，我们只需要生成一系列X,Y的数值组合，就可以操控示波器的显示了。

<img src="../img/Startest.bmp" alt="" />

我们使用画图软件生成一幅黑白图片，然后使用Python编写一段小脚本将上述图片转换成为声音。
<pre><code>import wave
import struct
import Image

BLACK = 0
WHITE = 255

def getImage():
    im = Image.open('startest.bmp') #black and white 1000 x 270
    pix = im.load() #black is 0; white is 255
    print im.size
    return pix

def main():
    im = Image.open('startest.bmp') #black and white 1000 x 270
    pix = im.load() #black is 0; white is 255
    print im.size # 1000,270

    f = wave.open('startest.wav','w')
    f.setsampwidth(2)
    f.setnchannels(2)
    f.setframerate(192000)

    max_x = im.size[0]
    max_y = im.size[1]
    data = []
    for i in range(im.size[0]):
    for j in range(im.size[1]):
        if pix[i,j] == BLACK:
            x = -(65535.0 * i*1.0/max_x - 32767.0) * 0.8
            y = (65535.0 * j*1.0/max_y - 32767.0) * 0.8
            d = struct.pack('hh',y,x)
            data.append(d)
    for v in range(10): #重复10遍，以延长WAV文件时间，使效果更明显
    for d in data:
        f.writeframes(d)
    f.close()        

if __name__ == "__main__":
    main()
</code></pre>
最后，找到一根废旧耳机，断之。把里面的导线剥出来，然后用示波器的X路表笔勾住耳机线的地和左声道，用示波器的Y路表笔勾住耳机线的地和右声道。将耳机线的另一头接到电脑的声音输出上。

TODO:接线图?

打开任意一个音频播放器，播放刚刚我们生成的WAV文件，在示波器上即可显示我们的文字。

有趣的是，当你调整音量时，示波器上显示的图形会随之进行缩放。另外，如果发现显示的文字有虚化的现象，有可能是我们生成的示波器上的点的位置变化太快，可以尝试每个采样点重复地多生成几遍。TODO: 待验证

让我们回想一下，刚刚我们做了哪些事情。我们写了一段程序，按照192kHz的采样速率，按照我们的要求生成了WAV文件里的每一个采样点。

那么，如果我们合理的选择一些点的位置，让这些点代表不同的含意，例如把示波器的屏幕等分成四个区域，点落在各区域里分别代表数字00,01,10,11。那么，我们就可以用声音来传播数字信息了。

TODO: 补充QPSK的示意图

事实上，这种思路就是通信系统里常用的QPSK(Quadrature Phase Shift Keying,正交相移键控)的方案。

更进一步，如果我们对于打出的点的精确度有足够的信心，我们可以把区域分的更多，例如分成16个区域。点落在每个区域里分别表示
<pre><code>0000
0001
0010
0011
0100
0101
0110
0111
1000
1001
1010
1011
1100
1101
1110
1111
</code></pre>
这样，我们只需要打出一个点便可以表示更多的数据，这便是16QAM的基本想法。

实际上BPSK、QPSK、16QAM、64QAM都使用了这种想法，不过在细节上有更多关于实际通信系统实现上的考虑，例如使用改变相位的方式来打出点。
<h2>其它常用的通信概念</h2>
<h3>dB / dBm</h3>
dB(decibel)是一个表征相对值的单位。计算方法为

$10 \times \log_{10}{(\frac{A}{B})}$

一辆卡车的重量是1000Kg，一只背包的重量是1Kg。那么卡车的重量比背包的重量大30dB

$10 \times \log_{10}{\frac{1000Kg}{1Kg}} = 30 dB $

同样，A信号的功率是100W，B信号的功率是1W。那么A信号比B信号功率大20dB

在无线通信领域里经常会遇到成千上万倍的比例，引入dB的概念之后，工程师们可以免于抄写长串0的苦恼。

同时，引入dB之后，我们可以把原来的乘法运算变成了加法运算。例如一个可以放大1000倍的功率放大器，我们可以记为30dB放大器；一个把信号衰减一半的衰减器，我们可以记为-3dB衰减器。

dBm则是将被衡量的功率值与1mW的功率进行比较，30dBm也即1W。

$30 dBm = 10 * \log_{10}{\frac{1W}{1mW}} $

在工程中经常用一些估算方法来求概略值，例如：增加3dB = 乘2倍； 减少3dB = 变成1/2 ；增加10dB =乘10倍

这样便可以直接进行快速运算来求得概略值：
<pre><code>+3dB= *2
+6dB= *4    (2*2)
+7dB= *5    (+10dB-3dB = 10/2)
+4dB= *2.5  (+10dB-6dB = 10/4)
+1dB= *1.25 (+4dB-3dB=2.5/2)
+2dB= *1.6  (+6dBm-4dBm=4/2.5=1.6)
</code></pre>
举个例子，计算47dBm时，40dBm = $10^4$mW，再多7dBm = 5 * $10^4$mW = 50W。

以下的计算读者可以验算一下并把它记忆。
<pre><code>0 dBm = 1 mW
10 dBm = 10 mW
14 dBm = 25 mW
15 dBm = 32 mW
16 dBm = 40 mW
17 dBm = 50 mW
20 dBm = 100 mW
30 dBm = 1000 mW = 1W
</code></pre>
<h3>IQ采样 / 复采样</h3>
I 指的是 in-phase(同相)数据， Q指的是quadrature(正交)data (because the carrier is offset by 90 degrees)

参考:
<ul>
	<li>NI: <a href="http://www.ni.com/white-paper/4805/en/">http://www.ni.com/white-paper/4805/en/</a></li>
	<li>IQ Data for dummies: <a href="http://whiteboard.ping.se/SDR/IQ">http://whiteboard.ping.se/SDR/IQ</a></li>
	<li><a href="http://www.fourier-series.com/IQMod/">http://www.fourier-series.com/IQMod/</a></li>
</ul>
为什么直接对波形的幅度进行采样不满足要求？而一定要使用IQ采样？
<h4>硬件设计上的考虑</h4>
大部分的通讯系统都使用了调相(PM, Phase Modulation)的调制方式，但是由于硬件设计上的考虑，直接操作一个波形的相位是困难的。

为了避免直接操作波形的相位，我们使用了以下的想法:

$A\sin(2 \pi f_c t + \phi) = A\cos(\phi)\cos(2\pi f_c t) - A\sin(\phi)(2\pi f_c t)$

其中，我们令

$I = A\cos(\phi) $

$Q = A\sin(\phi) $

这其中使用了和差化积公式:

$\cos(\alpha+\beta) = \cos(\alpha)\cos(\beta) - \sin(\alpha)\sin(\beta)$

于是，要想改变$A\sin(2 \pi f_c t + \phi) $波形的相位$\phi$，我们只需要同时改变I和Q之值即可， 然后将I作为幅度乘以$\cos(2\pi f_c t)$，将Q作为幅度乘以$\sin(2\pi f_c t)$，最后将两者相加，即可完成相位的调制。

这样的做法使得硬件电路上的设计变得简单多了，把对时间的操作变为了对电压的操作，在电路中对时间的精确控制不易，但对电压的精确操作却是容易的。

在此有一个假定，我们认为所有的真实信号(也就是I)都可以被表示为 当我们取得了一组I,Q之值时，我们不只得到了我们信号的瞬时值，还知道了生成这个信号的函数:

有一点需要明确，任何发射到空中的信号或者说真实信号都是不存在虚部的。
<h4>采样率的考虑</h4>
另外，采用IQ采样可以降低每个支路的采样率，如果用幅度检波后的采样率将是其两倍，降低了对ADC(Analog to Digital Converter)的要求。
<h3>本振(LO, Local Oscillator)</h3>
本振频率，英文Local Oscillator。 就是LC振荡器。用在超外差接收机中。超外差接收机中有一个振荡器叫本机振荡器。它产生的高频电磁波与所接收的高频信号混合而产生一个差频，这个差频就是中频。如要接收的信号是900KHZ.本振频率是1365KHZ.两频率混合后就可以产生一个465KHZ或者2265KHZ的差频。接收机中用LC电路选择465KHZ作为中频信号。因为本振频率比外来信号高465KHZ所以叫超外差。

TODO: FIXME!!
<h3>阻抗匹配</h3>
<h3>中频</h3>
<h3>变频</h3>
$\cos(\omega_1 t) * \cos(\omega_2 t) = $ TODO
<h3>驻波</h3>
<h3>噪声系数</h3>
<h3>FFT</h3>
FFT(Fast Fourier Transform, 快速傅里叶变换)
<h3>OFDM</h3>
<h1>GNURadio及HackRF介绍</h1>
<h2>背景</h2>
<h3>GNURadio</h3>
<h4>Boost</h4>
<h3>HackRF</h3>
FIXME: 参考<a href="http://2013.hackitoergosum.org/presentations/Day2-04.HackRF%20A%20Low%20Cost%20Software%20Defined%20Radio%20Platform%20by%20Benjamin%20Vernoux.pdf">PDF</a>

FIXME: 参考<a>kickstarter</a>的介绍

HackRF是一款由Michael Ossmann发起的开源软件无线电外设，旨在从30MHz到6GHz，于2012年从DARPA处拿了一笔经费，制作了500块测试版本Jawbreaker，并向社会分发测试。在经过用户对Jawbreaker的反馈后，作者对硬件板卡做了重新布线，改善了射频性能，这一点我们将会在后文详细讨论。 随后于2013年7月31日至9月4日共计35天的时间，在著名的社会化融资平台Kickstarter上，迅速地获得多达1991人的预订，共预订出价值为$602,960的HackRF One。

现在已经被gqrx和osmocom-sdr等支持
<ul>
	<li>全面支持GNURadio</li>
	<li>30MHz – 6GHz</li>
	<li>与RTL2832U(RTLSDR)不同，HackRF可以进行发射</li>
	<li>比USRP更廉价</li>
	<li>最大采样率: 20 Msps (10倍于电视棒RTLSDR)</li>
	<li>接口: High Speed USB</li>
	<li>USB供电</li>
	<li>硬件/软件全部开源</li>
	<li>获得了DARPA的Cyber Fast Track项目的支持</li>
	<li>已经在KickStarter上拿到投资</li>
</ul>
测试版本Jawbreaker已经不被最新版的固件所支持。
<h4>HackRF 的硬件原理</h4>
TODO: 插图 硬件主要由以下几部分组成
<ul>
	<li>RFFC5072: 混频器提供80MHz到4200MHz的本振</li>
	<li>MAX2837: 2.3GHz to 2.7GHz 无线宽带射频收发器</li>
	<li>MAX5864: ADC/DAC, 22MHz采样率 8bit</li>
	<li>LPC4320/4330: ARM Cortex M4处理器, 主频204MHz</li>
	<li>Si5351B: I2C可编程任意CMOS时钟生成器，由800MHz分频提供40MHz 50MHz 及采样时钟</li>
	<li>MGA-81563: 0.1–6GHz 3V, 14 dBm 放大器</li>
	<li>SKY13317: 20 MHz-6.0 GHz 射频单刀三掷(SP3T)开关</li>
	<li>SKY13350: 0.01-6.0 GHz 射频单刀双掷(SPDT)开关</li>
</ul>
以接收过程为例，信号由天线进入后流程如下
<ul>
	<li>由射频开关决定是否经由14dB的放大器进行放大</li>
	<li>经过镜像抑制滤波器对信号进行高通或低通滤波</li>
	<li>信号进行RFFC5072芯片混频到2.6GHz固定中频</li>
	<li>信号送入MAX2837芯片混频到基带，输出差分的IQ信号
<ul>
	<li>其间MAX2837芯片可以对信号进行带宽限制</li>
</ul>
</li>
	<li>MAX5864芯片对基带信号进行数字化后送入CPLD和单片机 TODO FIXME</li>
	<li>CPLD干了什么?</li>
	<li>LPC4320/4330处理器将采样数据通过USB送至计算机</li>
</ul>
<h4>HackRF One针对Jawbreaker做了哪些改进</h4>
<ul>
	<li>删除了板载废柴微带天线</li>
	<li>将RFFC5072和MAX2837放入屏蔽罩内保护起来，防止外界及板上其它芯片的干扰，并试图防止静电击穿部分芯片</li>
	<li>重新布局，使得射频连线更紧凑</li>
</ul>
实际测试过程中，我们发现Jawbreaker中由于布线问题造成的全频谱范围内1MHz为周期出现的小信号干扰，在HackRF One中完全消除。

TODO: 上对比图
<h2>环境搭建</h2>
本章节介绍如何在Linux环境中搭建GNURadio和HackRF的开发调试平台。
<h3>GNURadio</h3>
不要使用发行版自带的GNURadio软件包，因为发行版的维护者比较懒，或者说GNURadio的开发者们没有主动向发行版里发布版本。
<pre><code>$ wget http://www.sbrac.org/files/build-gnuradio
$ chmod +x build-gnuradio
$ ./build-gnuradio -v -ja
</code></pre>
然后在<code>Proceed?</code>后面输入<code>y</code>回车。在<code>Do you have SUDO privileges?</code>提示后面依然输入<code>y</code>回车。

注意，执行<code>build-gnuradio</code>脚本的时候，不要预先使用<code>sudo</code>。

加入<code>-ja</code>的选项后，使用多核处理器的并行处理能力来编译GNURadio，加快编译速度。整个编译安装过程可能会持续2小时甚至更长的时间，取决于网络速度。因此建议使用比较快的Linux镜像源服务器。

然后<code>build-gnuradio</code>脚本会自动安装所需要的软件包，然后开始安装。

值得一提的是，<code>build-gnuradio</code>脚本会自动安装最新版本的hackrf软件。
<h3>HackRF 环境搭建</h3>
<h2>应用示例</h2>
<h5>Throttle</h5>
<h4>FM接收</h4>
gqrx

另参考http://binaryrf.com/viewtopic.php?f=9&amp;t=16
<h4>FM发射</h4>
<h4>DAB发射</h4>
<h4>遥控小车信号解析</h4>
<h4>NOAA卫星接收</h4>
<h4>GSM信号解调</h4>
<h4>HackRF LTE-Cell-Scanner</h4>
http://v.youku.com/v_show/id_XNjc1MjIzMDEy.html
<h4>解析Pocsag Pagers</h4>
http://binaryrf.com/viewtopic.php?f=9&amp;t=8
<h4>Washington DC HackRF</h4>
http://lukeberndt.com/2013/using-a-hackrf-to-capture-an-entire-radio-system/

http://www.openmhz.com/

Tech

There is a lot going on behind this simple looking website. Here is a high level overview of how it works, but shot me an email if you want details.

The radio signals are received using the HackRF Software Defined Radio (SDR). The SDR receives a wide swath of radio spectrum and passes it to a computer to process and decode. Using this approach, it is possible to receive all of the transmission from the radio system and decode them simultaneously. Without the SDR a separate radio receiver would be need for each channel.

In a Trunking system, one of the radio channels is set aside for to manage the assignment of radio channels to talkgroups. When someone wants to talk, they send a message on the control channel. The system then assigns them a channel and sends a Channel Grant message on the control channel. This lets the talker know what channel to transmit on and anyone who is a member of the talkgroup know that they should listen to that channel.

In order to follow all of the transmissions, this system constantly listens to and decodes the control channel. When a channel is granted to a talkgroup, the system creates a monitoring process. This process will start to process and decode the part of the radio spectrum for that channel which the SDR is already pulling in.

No message is transmitted on the control channel when a talkgroup's conversation is over. So instead the monitoring process keeps track of transmissions and if there has been no activity for 5 seconds, it ends the recording and uploads to the webserver.

The monitoring and recording is being run off of a laptop in my apartment and uses a crappy antenna. The website is run off a VPS I have running up in the magical cloud.

The webserver is pretty simple. It is written in NodeJs. The audio is stored as WAV files and indexed using MongoDB. The server simply watches for new files being placed in a directory and then moves them and adds them to the DB. Socket.io is used to updated all of the browsers visiting the site that a new transmission has been added. See - Easy, Peasy!
<h4>解析大车的信号</h4>
http://binaryrf.com/viewtopic.php?f=3&amp;t=20 http://blog.kismetwireless.net/2013/08/playing-with-hackrf-keyfobs.html
<h4>解析FLEX信号</h4>
<h4>Kismet教程</h4>
http://blog.kismetwireless.net/2013/08/hackrf-pt-2-gnuradio-companion-and.html
<h4>AIS</h4>
船 161.975MHz or 162.025MHz
<h4>Bluetooth monitoring</h4>
<h4>wireless microphones</h4>
<h4>DVB</h4>
<h4>DECT手机?</h4>
RFID (Radio Freq Identification)

Cellular GSM base station

GPS receiver

AM/FM Radio TX/RX, APCO-25 (USA) / TETRA (EU) Digital Radio

Digital Television (ATSC/DVB-T)

Passive radar
<h4>1090</h4>
gr-air-modes osmocom modes_gui

-d, --dcblock Use a DC blocking filter (best for HackRF Jawbreaker) [default=False]
<h4>通过网络传输IQ数据</h4>
<h4>TV Sharp</h4>
<h4>gr-dvb</h4>
<h4>gr-drm</h4>
<h1>GNURadio模块编写示例 - 遥控小车的信号分析与生成</h1>
可以参阅http://gnuradio.org/redmine/projects/gnuradio/wiki/OutOfTreeModules
<h2>直接重放</h2>
首先使用频谱仪或者查资料获知其频率在27.145MHz，所以我们取中心频率为27MHz，以8M采样率采回16M个点，时长2秒
<pre><code>hackrf_transfer -r car.iq -f 27000000 -s 8000000 -n 16000000
</code></pre>
重放，注意优化它的发射增益
<pre><code>hackrf_transfer -t car.iq -f 27000000 -s 8000000 -a 1 -l 30 -i 30 -x 40 
</code></pre>
<h2>使用GNURadio对采集到的iq进行分析</h2>
<h2>控制信号分析结果</h2>
27.145MHz的遥控小车的信号大致可以认为是如下的PPM/AM波形:

PPM意为Pulse Position Modulation，脉冲位置调制

<img src="../img/RemoteCar-AMDemod.png" alt="" />
<pre><code>                      TIME3         TIME4
      --------+    +---------+    +-------+    +--------- ... -------+    +---.....
              |    |         |    |       |    |                     |    |
              |    |         |    |       |    |                     |    |
              |    |         |    |       |    |                     |    |
              |    |         |    |       |    |                     |    |
              +----+         +----+       +----+                     +----+
              TIME0          TIME0        TIME0
           --&gt;|                                TIME2                 |&lt;---
</code></pre>
其中的典型值为 TIME0 = 520us TIME2 = 20ms TIME3,TIME4 = [300us,1.3ms]

TIME3的时间长度控制了小车的左右 TIME4的时间长度控制了小车的油门量
<h2>用Python生成基带进行原理验证</h2>
<pre><code>import struct

SAMP_RATE=8e6
TIME_TOTAL = int(1 * SAMP_RATE) #s

TIME0 = int(520e-6 * SAMP_RATE)
TIME10 = int(300e-6 * SAMP_RATE)
TIME11 = int(1.3e-3 * SAMP_RATE)
TIME2 = int(20e-3 * SAMP_RATE)

Control0 = 0 #[0,1] 0.5 = stop orientation
Control1 = 1#speed

TIME3 = (TIME11-TIME10) * Control0 + TIME10
TIME4 = (TIME11-TIME10) * Control1 + TIME10

TIME_REST = TIME2-TIME0*3-TIME3-TIME4

MIN=struct.pack('B',128)
MAX=struct.pack('B',255)

def WriteFrame(value,quantity,f):
    j = 0
    while j &lt; quantity:
    f.write(value) #i
    f.write(value) #q
    j += 1

def main():
    f = open('w.iq','wb')

    i = 0
    WriteFrame(MAX,1e-3*SAMP_RATE,f)
    i += 1e-3*SAMP_RATE
    while i &lt; TIME_TOTAL:
    WriteFrame(MIN,TIME0,f)
    i += TIME0
    WriteFrame(MAX,TIME3,f)
    i += TIME3
    WriteFrame(MIN,TIME0,f)
    i += TIME0
    WriteFrame(MAX,TIME4,f)
    i += TIME4
    WriteFrame(MIN,TIME0,f)
    i += TIME0
    WriteFrame(MAX,TIME_REST,f)
    i += TIME_REST
    f.close()        

if __name__ == "__main__":
    main()
</code></pre>
然后生成了w.iq的原始基带数据.

如何查看它是否正确呢？让我们打开gnuradio-companion来做出一个简单的信号流程来调试一下。

TODO
<h2>写GNURadio模块: 第二种车</h2>
<h3>信号原理分析</h3>
<img src="../img/RemoteCar-Analysis.png" alt="" />

使用gnuradio-companion搭建AM解调，然后输出到WX GUI Scope Sink里，发现信号在27MHz是如下情形:
<pre><code>+----------+     +----------+     +----------+     +----------+     +-----+     +-----+                        
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
+          +-----+          +-----+          +-----+          +-----+     +-----+     +-...                     

|&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |  t  |  t  |  t  |         
</code></pre>
每个控制帧都由4个长脉冲和n个短脉冲组成

经过测试，找到n的值如下:
<pre><code>左: n=58
右: n=64
1档前进: n=10
2档前进: n=22
后退: n=40
1档左前: n=28
1档右前: n=34
左后: n=46
右后: n=52
</code></pre>
<h3>模块建立</h3>
<pre><code>$ gr_modtool new remotecar

$ gr_modtool add RemoteCarIIBaseBand -t sync
GNU Radio module name identified: remotecar
Language: C++
Block/code identifier: RemoteCarIIBaseBand
Enter valid argument list, including default arguments: double samp_rate,bool run, int command
Add Python QA code? [Y/n] n
Add C++ QA code? [Y/n] n
Adding file 'RemoteCarIIBaseBand_impl.h'...
Adding file 'RemoteCarIIBaseBand_impl.cc'...
Adding file 'RemoteCarIIBaseBand.h'...
Editing swig/remotecar_swig.i...
Adding file 'remotecar_RemoteCarIIBaseBand.xml'...
Editing grc/CMakeLists.txt...
</code></pre>
<h3>写io_signature</h3>
在lib/RemoteCarIIBaseBand_impl.cc文件中:
<pre><code>RemoteCarIIBaseBand_impl::RemoteCarIIBaseBand_impl(double samp_rate,bool run, int command)
      : gr::sync_block("RemoteCarIIBaseBand",
          gr::io_signature::make(0,0,0),
          gr::io_signature::make(1,1,sizeof(float)))
</code></pre>
<h3>添加所需变量</h3>
在lib/RemoteCarBaseBand_impl.h里加入
<pre><code>namespace gr {
  namespace remotecar {

    class RemoteCarIIBaseBand_impl : public RemoteCarIIBaseBand
    {
     private:
         double d_samp_rate;
         bool bool_run;

         int n_pre;
         int n_command;

         int current_pre;
         int current_command;

         int current_sample_index;

     public:
      RemoteCarIIBaseBand_impl(double samp_rate,bool run, int command);
      ~RemoteCarIIBaseBand_impl();

      // Where all the action really happens
      int work(int noutput_items,
           gr_vector_const_void_star &amp;input_items,
           gr_vector_void_star &amp;output_items);
    };

  } // namespace remotecar
} // namespace gr
    ....
</code></pre>
<h3>work</h3>
整个类的生命周期内一直存在， GNURadio的调度器会调用work函数，索取noutput_items个结果
<h3>生成grc</h3>
<pre><code>$ gr_modtool makexml RemoteCarIIBaseBand
GNU Radio module name identified: remotecar
Warning: This is an experimental feature. Don't expect any magic.
Searching for matching files in lib/:
Making GRC bindings for lib/RemoteCarIIBaseBand_impl.cc...
Overwrite existing GRC file? [y/N] y
</code></pre>
<h3>grc中On Off的设置</h3>
参考: gnuradio/gr-wxgui/grc/wxgui_scopesink2.xml
<pre><code>&lt;block&gt;
  &lt;name&gt;Remotecariibaseband&lt;/name&gt;
  &lt;key&gt;remotecar_RemoteCarIIBaseBand&lt;/key&gt;
  &lt;category&gt;REMOTECAR&lt;/category&gt;
  &lt;import&gt;import remotecar&lt;/import&gt;
  &lt;make&gt;remotecar.RemoteCarIIBaseBand($samp_rate,$run, $command)&lt;/make&gt;
  &lt;param&gt;
    &lt;name&gt;Sample Rate&lt;/name&gt;
    &lt;key&gt;samp_rate&lt;/key&gt;
    &lt;type&gt;real&lt;/type&gt;
  &lt;/param&gt;
  &lt;param&gt;
    &lt;name&gt;Run&lt;/name&gt;
    &lt;key&gt;run&lt;/key&gt;
    &lt;value&gt;True&lt;/value&gt;
    &lt;type&gt;bool&lt;/type&gt;
    &lt;option&gt;
    &lt;name&gt;Off&lt;/name&gt;
    &lt;key&gt;False&lt;/key&gt;
    &lt;/option&gt;
    &lt;option&gt;
    &lt;name&gt;On&lt;/name&gt;
    &lt;key&gt;True&lt;/key&gt;
    &lt;/option&gt;
  &lt;/param&gt;
  &lt;param&gt;
    &lt;name&gt;Command&lt;/name&gt;
    &lt;key&gt;command&lt;/key&gt;
    &lt;type&gt;int&lt;/type&gt;
  &lt;/param&gt;
  &lt;source&gt;
    &lt;name&gt;out&lt;/name&gt;
    &lt;type&gt;float&lt;/type&gt;
  &lt;/source&gt;
&lt;/block&gt;
</code></pre>
<h3>Stage 1通关测试</h3>
<pre><code>    int
    RemoteCarIIBaseBand_impl::work(int noutput_items,
              gr_vector_const_void_star &amp;input_items,
              gr_vector_void_star &amp;output_items)
    {
    float *out = (float *) output_items[0];

    for (int i = 0;i &lt; noutput_items; i++){
            out[i] = 1.5;
    }

    // Tell runtime system how many output items we produced.
    return noutput_items;
    }
</code></pre>
<img src="写模块-Stage1.png" alt="" />
<h3>加入基带信号生成部分</h3>
在lib/RemoteCarBaseBand_impl.cc里加入代码
<pre><code>RemoteCarIIBaseBand_impl::RemoteCarIIBaseBand_impl(double samp_rate,bool run, int command)
      : gr::sync_block("RemoteCarIIBaseBand",
          gr::io_signature::make(0,0,0),
          gr::io_signature::make(1,1,sizeof(float)))
    {
    bool_run = run; // output on off
    d_samp_rate = samp_rate; 

    n_command = command; // command code 
    n_pre = 4; // pre pulse number

    current_command = 0;
    current_pre = 0;

    current_sample_index = 0;

    }

    ...

    int
    RemoteCarIIBaseBand_impl::work(int noutput_items,
              gr_vector_const_void_star &amp;input_items,
              gr_vector_void_star &amp;output_items)
    {
    float *out = (float *) output_items[0];

    for (int i = 0;i &lt; noutput_items; i++){
            if (bool_run) {
                    if (current_pre &lt; n_pre) {
                        if (current_sample_index &lt; d_samp_rate * 0.00055 * 3) {
                                out[i] = 1;
                                current_sample_index += 1;
                        }
                        else if (current_sample_index &lt; d_samp_rate * 0.00055 * 4){
                                out[i] = 0;
                                current_sample_index += 1;
                        } else { // a long pre pulse generated.
                            current_sample_index = 0;
                            current_pre += 1;
                        }
                    }
                    else if (current_command &lt; n_command) {
                        // 4 pre long pulse generated, then generate other short pulse.
                        if (current_sample_index &lt; d_samp_rate * 0.00055 ) {
                                out[i] = 1;
                                current_sample_index += 1;
                        }
                        else if (current_sample_index &lt; d_samp_rate * 0.00055 * 2){
                                out[i] = 0;
                                current_sample_index += 1;
                        } else { // a short command pulse generated
                            current_sample_index = 0;
                            current_command += 1;
                        }

                    }
                    else {
                            // 1 frame generated
                          current_pre = 0;
                          current_command = 0;
                          current_sample_index = 0;
                    }
            } else { // muted
                    out[i] = 0;
            }

    }

    // Tell runtime system how many output items we produced.
    return noutput_items;
    }
</code></pre>
<h3>回调函数</h3>
如果没有回调函数，那么生成的模块不能在gnuradio-companion里被WX GUI Slider实时的修改参数。

为了能够实时地控制小车，我们需要加入两个回调函数。

在lib/RemoteCarBaseBand_impl.h里加入set_run和set_command函数的声明
<pre><code>namespace gr {
  namespace remotecar {

    ....

      // Where all the action really happens
      int work(int noutput_items,
           gr_vector_const_void_star &amp;input_items,
           gr_vector_void_star &amp;output_items);
      void set_run(bool run);
      void set_command(int command);
    };

  } // namespace remotecar
} // namespace gr
    ....
</code></pre>
还需要在include/remotecar/RemoteCarIIBaseBand.h加入set_run和set_command的声明
<pre><code>   class REMOTECAR_API RemoteCarIIBaseBand : virtual public gr::sync_block
    {
     public:
      typedef boost::shared_ptr&lt;RemoteCarIIBaseBand&gt; sptr;

      static sptr make(double samp_rate,bool run, int command);
      virtual void set_run(bool run) = 0;
      virtual void set_command(int command) = 0 ;
    };
</code></pre>
在lib/RemoteCarIIBaseBand_impl.cc里加入set_run和set_command的实现
<pre><code>    void RemoteCarIIBaseBand_impl::set_run(bool run) {
        bool_run = run;
    }

    void RemoteCarIIBaseBand_impl::set_command(int command) {
        n_command = command;
    }
</code></pre>
最后，在grc/remotecar_RemoteCarIIBaseBand.xml文件里加入
<pre><code>  &lt;make&gt;remotecar.RemoteCarIIBaseBand($samp_rate,$run, $command)&lt;/make&gt;
  &lt;callback&gt;set_run($run)&lt;/callback&gt;
  &lt;callback&gt;set_command($command)&lt;/callback&gt;
  ....
</code></pre>
<h3>编译</h3>
<pre><code>mkdir build
cd build
cmake ../ &amp;&amp; make &amp;&amp; sudo make install &amp;&amp; sudo ldconfig
</code></pre>
然后重启gnuradio-companion即可搭建一个简单的示例来跑通小车

<img src="../img/RemoteCar-Example.png" alt="" />
<h3>加入一个简单的GUI</h3>
如果觉得这样操作不舒服，我们可以用Qt写一个简单的键盘控制的方向盘。

<img src="../img/RemoteCar-qt_wheel.png" alt="" />
<h3>gr_modtool 用法概览</h3>
<pre><code>gr_modtool newmod blahblah
gr_modtool add -t general square_ff
make test / ctest -V -R blahblah
gr_modtool add -t sync square2_ff
set_history()
input_items.size()  / output_items.size()
Sync Interpolator
gr_modtool makexml square2_ff
forecast()
gr::sync_block, gr::sync_interpolator or gr::sync_decimator instead of gr::block
set_output_multiple() 攒够固定倍数才输出
Hierarchical Block
    gr_modtool.py add -t hier hierblockcpp_ff
    Delete blocks from the source tree: gr_modtool rm REGEX
 Disable blocks by removing them from the CMake files: gr_modtool disable REGEX
Python: gr_modtool add -t sync -l python square3_ff
</code></pre>
<h1>HackRF硬件分析及射频指标测试</h1>
<h2>HackRF硬件分析</h2>
<h3>芯片简介</h3>
<h4>RFFC5072</h4>
Product Description The RFFC5071 and RFFC5072 are re-configurable frequency conversion devices with integrated fractional-N phased locked loop (PLL) synthesizer, voltage con- trolled oscillator (VCO) and either one or two high linearity mixers. The fractional-N synthesizer takes advantage of an advanced sigma-delta modulator that delivers ultra-fine step sizes and low spurious products. The PLL/VCO engine combined with an external loop filter allows the user to generate local oscillator (LO) signals from 85MHz to 4200MHz. The LO signal is buffered and routed to the integrated RF mix- ers which are used to up/down-convert frequencies ranging from 30MHz to 6000MHz. The mixer bias current is programmable and can be reduced for applica- tions requiring lower power consumption. Both devices can be configured to work as signal sources by bypassing the integrated mixers. Device programming is achieved via a simple 3-wire serial interface. In addition, a unique programming mode allows up to four devices to be controlled from a common serial bus. This eliminates the need for separate chip-select control lines between each device and the host controller. Up to six general purpose outputs are provided, which can be used to access internal signals (the LOCK signal, for example) or to control front end components. Both devices operate with a 2.7V to 3.3V power supply
<h4>MAX2837</h4>
The MAX2837 direct-conversion zero-IF RF transceiver is designed specifically for 2.3GHz to 2.7GHz wireless broadband systems. The MAX2837 completely inte- grates all circuitry required to implement the RF trans- ceiver function, providing RF-to-baseband receive path; and baseband-to-RF transmit path, VCO, frequency synthesizer, crystal oscillator, and baseband/control interface. The device includes a fast-settling sigma- delta RF synthesizer with smaller than 20Hz frequency steps and a crystal oscillator, which allows the use of a low-cost crystal in place of a TCXO. The transceiver IC also integrates circuits for on-chip DC offset cancella- tion, I/Q error, and carrier-leakage detection circuits. Only an RF bandpass filter (BPF), crystal, RF switch, PA, and a small number of passive components are needed to form a complete wireless broadband RF radio solution. The MAX2837 completely eliminates the need for an external SAW filter by implementing on-chip monolithic filters for both the receiver and transmitter. The baseband filters along with the Rx and Tx signal paths are optimized to meet stringent noise figure and linearity specifications. The device supports up to 2048 FFT OFDM and imple- ments programmable channel filters for 1.75MHz to 28MHz RF channel bandwidths. The transceiver requires only 2μs Tx-Rx switching time, which includes frequency transient settling. The IC is available in a small, 48-pin thin QFN package measuring only 6mm x 6mm x 0.8mm.
<h4>MAX5864</h4>
The MAX5864 ultra-low-power, highly integrated analog front end is ideal for portable communication equipment such as handsets, PDAs, WLAN, and 3G wireless termi- nals. The MAX5864 integrates dual 8-bit receive ADCs and dual 10-bit transmit DACs while providing the high- est dynamic performance at ultra-low power. The ADCs’ analog I-Q input amplifiers are fully differential and accept 1V P-P full-scale signals. Typical I-Q channel phase matching is ±0.1° and amplitude matching is ±0.03dB. The ADCs feature 48.5dB SINAD and 69dBc spurious-free dynamic range (SFDR) at f IN = 5.5MHz and f CLK = 22Msps. The DACs’ analog I-Q outputs are fully differential with ±400mV full-scale output, and 1.4V com- mon-mode level. Typical I-Q channel phase match is ±0.15° and amplitude match is ±0.05dB. The DACs also feature dual 10-bit resolution with 71.7dBc SFDR, and 57dB SNR at f OUT = 2.2MHz and f CLK = 22MHz. The ADCs and DACs operate simultaneously or indepen- dently for frequency-division duplex (FDD) and time-divi- sion duplex (TDD) modes. A 3-wire serial interface controls power-down and transceiver modes of opera- tion. The typical operating power is 42mW at f CLK = 22Msps with the ADCs and DACs operating simultane- ously in transceiver mode. The MAX5864 features an internal 1.024V voltage reference that is stable over the entire operating power-supply range and temperature range. The MAX5864 operates on a +2.7V to +3.3V ana- log power supply and a +1.8V to +3.3V digital I/O power supply for logic compatibility. The quiescent current is 5.6mA in idle mode and 1μA in shutdown mode. The MAX5864 is specified for the extended (-40°C to +85°C) temperature range and is available in a 48-pin thin QFN package.
<h2>射频指标概述</h2>
<h2>HackRF 射频指标</h2>
最大发射功率 ~ 10dBm 64QAM发射EVM ~ 1.5% 复采样带宽 20MHz
<h1>高级话题</h1>
<h3>如何使用两个HackRF</h3>
Here's a tricky method about duplex.

If you plug in two hackrf device, hackrf_info will only show one hackrf device.

But, if you plug in one first, run something with this hackrf to occupy it. Then plug in another hackrf device, then run another program , and the 'duplex' works.
<h3>SciPy</h3>
<pre><code>f = scipy.fromfile(open("myFile.bin"), dtype=scipy.complex64)
</code></pre>
<h3>时钟同步</h3>
<h3>手工打造HackRF</h3>
大约需要2天造一块
<h3>如何对HackRF进行刷机</h3>
<h3>对HackRF做贡献</h3>
报Bug

提交补丁

https://github.com/mossmann/hackrf/pull/108

HackRF One外壳设计
<h3>Grid Positioning</h3>
http://gnuradio.org/redmine/projects/gnuradio/wiki/GNURadioCompanion#Grid-Positioning
<h3>message source</h3>
http://gnuradio.org/redmine/projects/gnuradio/wiki/TutorialsCoreConcepts#Streams-vs-Messages-Passing-PDUs
<h3>PPM校准</h3>
简单的方法: 找一个已知的FM频率，在gqrx直接修改ppm，直到该FM电台的标称频率刚好与实际值一致
<pre><code>kalibrate_rtl
</code></pre>
<h3>滤波器</h3>
<h3>KiCAD 图形Diff</h3>
compare image1 image2 -compose src diff.png compare image1 image2 -compose src diff.pdf

另: 由pdf提取png convert example.pdf -density 300 example.png 其中300为DPI数值

或

diffpdf
<h3>串扰问题</h3>
经常会在FM收听时发现某台(例如103.9MHz)，在103.9M+采样频率，例如103.9+8 = 111.9MHz处有一个镜像台

此外,

It looks like you're getting the hang of it, but here is an answer to your earlier question about how to predict the bad spurs.

The IF is the intermediate frequency the MAX2837 is tuned to. The RF is the radio frequency of interest at the antenna port. The LO is the local oscillator frequency of the RFFC5072. (Technically there is another LO in the MAX2837, but it is the same frequency as IF. When I refer to LO, I am talking about the LO in the RFFC5072.)

RF = |IF+LO| or RF = |IF - LO|

Which one (the sum or the difference) depends on the configuration of the image reject filter stage.

Bad spurs occur when LO or an integer multiple of LO is within 10 MHz (or half of your baseband filter bandwidth) of RF. This happens due to leakage of the LO into the RF side of the RFFC5072.

Bad spurs occur when LO or an integer multiple of LO is within 10 MHz (or half of your baseband filter bandwidth) of IF. This happens due to leakage of the LO into the IF side of the RFFC5072.

To be safe, it is probably best to keep LO harmonics 20 MHz or further away from RF or IF. Right now our automatic tuning code (which is already fairly complicated) does not take LO leakage into account.
<h3>DC Offset</h3>
<pre><code>    DC Removal
        DC Blocker
        osmocom source: DC OFFSET
    DC Blocker

    HackRF Jawbreaker性能分析
    GNURadio Scheduler 调度
    gcc-arm-none-eabi
    HackRF开发环境搭建
        gcc-arm-none-eabi
        DFU
    DFU
    GNURadio写模块
        gr-modtool
    osmocom模块分析
    HackRF One vs Jawbreaker

    gqrx代码分析
    hackrf usb代码分析
        opencm3
    KiCAD
    代码分析
        gqrx代码分析
        hackrf usb代码分析
            opencm3
    SMT贴装
    手工制造/制造工艺
        KiCAD
        SMT贴装
        QFN封装及介绍
        PCB工艺
    QFN封装及介绍
    PCB工艺
    PyBombs

    osmocom source: DC OFFSET
    rtlsdr
    Raspberry Pi / ARMv6h
    嵌入式系统跑
        Raspberry Pi / ARMv6h
        ODroid /ARMv7h
    ODroid /ARMv7h

    kalibrate_rtl
    HackRF on Android平板
</code></pre>
<h3>RF IF 干扰</h3>
<a href="https://github.com/mossmann/hackrf/issues/109">参阅</a>

并参阅files/WhySpurExists

example: Center = 1277MHz IF = 2560MHz

1283 - 1277 = 6

When Sampling Rate = 20M: Spur = 1283MHz = 2560 - 1277

When Sampling Rate = 8M: 1277 + 8/2 = 1281 &lt; 1283 so it is sub sampling: Spur = 1277 - [ 8 - (1283 - 1277) ]= 1277 - 2 = 1275 M

NOTE: the spur can be filtered by MAX2837 bandwidth filter.

When Sampling Rate = 10M: 1277 + 10/2 = 1282 &lt; 1283 sub sampling: Spur = 1277 - [10 - (1283 - 1277)] = 1277 - 4 = 1273
