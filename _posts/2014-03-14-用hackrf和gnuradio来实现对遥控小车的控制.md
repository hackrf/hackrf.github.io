---
ID: 266
title: "用HackRF和GNURadio来实现对遥控小车的控制"
author: scateu
date: 2014-03-14 00:34:51
post_excerpt: ""
layout: post
published: true
duoshuo_thread_id:
  - "1312073613704167462"
views:
  - "6922"
---
<h1>GNURadio模块编写示例 - 遥控小车的信号分析与生成</h1>
全部代码我已经发布到github上，见<a href="https://github.com/scateu/gr-remotecar">scateu/gr-remotecar</a>
<iframe width="320" height="240" style="width: 480px; height: 400px;" src="http://www.tudou.com/programs/view/html5embed.action?type=0&amp;code=wB4po5M5L5w&amp;lcode=&amp;resourceId=359197675_06_05_99" allowtransparency="true" border="0" frameborder="0" scrolling="no"></iframe>
可以参阅http://gnuradio.org/redmine/projects/gnuradio/wiki/OutOfTreeModules
<h2>直接重放</h2>
首先使用频谱仪或者查资料获知其频率在27.145MHz，所以我们取中心频率为27MHz，以8M采样率采回16M个点，时长2秒
<pre><code>hackrf_transfer -r car.iq -f 27000000 -s 8000000 -n 16000000
</code></pre>
重放，注意优化它的发射增益
<pre><code>hackrf_transfer -t car.iq -f 27000000 -s 8000000 -a 1 -l 30 -x 40 
</code></pre>
<h2>使用GNURadio对采集到的iq进行分析</h2>
<h2>控制信号分析结果</h2>
27.145MHz的遥控小车的信号大致可以认为是如下的PPM/AM波形:

PPM意为Pulse Position Modulation，脉冲位置调制

![][20]
<pre class="lang:default decode:true">+----------+     +----------+     +----------+     +----------+     +-----+     +-----+                        
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
+          +-----+          +-----+          +-----+          +-----+     +-----+     +-...                     

|&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |  t  |  t  |  t  |</pre>
&nbsp;

其中的典型值为 TIME0 = 520us TIME2 = 20ms TIME3,TIME4 = [300us,1.3ms]

TIME3的时间长度控制了小车的左右 TIME4的时间长度控制了小车的油门量
<h2>用Python生成基带进行原理验证<code> </code></h2>
<pre class="lang:default decode:true">import struct

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
    main()</pre>
&nbsp;

然后生成了w.iq的原始基带数据.

如何查看它是否正确呢？让我们打开gnuradio-companion来做出一个简单的信号流程来调试一下。

TODO
<h3>信号原理分析</h3>
![][21]

使用gnuradio-companion搭建AM解调，然后输出到WX GUI Scope Sink里，发现信号在27MHz是如下情形:
<pre class="lang:default decode:true">+----------+     +----------+     +----------+     +----------+     +-----+     +-----+                        
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
|          |     |          |     |          |     |          |     |     |     |     |                 
+          +-----+          +-----+          +-----+          +-----+     +-----+     +-...                     

|&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |&lt;-  3t  -&gt;|  t  |  t  |  t  |  t  |</pre>
&nbsp;

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
<h3>模块建立<code> </code></h3>
<pre class="lang:default decode:true">$ gr_modtool new remotecar

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
Editing grc/CMakeLists.txt...</pre>
<h3>写io_signature</h3>
在lib/RemoteCarIIBaseBand_impl.cc文件中:<code> </code>
<pre class="lang:default decode:true">RemoteCarIIBaseBand_impl::RemoteCarIIBaseBand_impl(double samp_rate,bool run, int command)
      : gr::sync_block("RemoteCarIIBaseBand",
          gr::io_signature::make(0,0,0),
          gr::io_signature::make(1,1,sizeof(float)))</pre>
&nbsp;
<h3>添加所需变量</h3>
在lib/RemoteCarBaseBand_impl.h里加入<code> </code>
<pre class="lang:default decode:true">namespace gr {
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
    ....</pre>
&nbsp;
<h3>work</h3>
整个类的生命周期内一直存在， GNURadio的调度器会调用work函数，索取noutput_items个结果
<h3>生成grc<code> </code></h3>
<pre class="lang:default decode:true">$ gr_modtool makexml RemoteCarIIBaseBand
GNU Radio module name identified: remotecar
Warning: This is an experimental feature. Don't expect any magic.
Searching for matching files in lib/:
Making GRC bindings for lib/RemoteCarIIBaseBand_impl.cc...
Overwrite existing GRC file? [y/N] y</pre>
<h3>grc中On Off的设置</h3>
参考: gnuradio/gr-wxgui/grc/wxgui_scopesink2.xml
<pre class="lang:default decode:true"> &lt;block&gt;
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
    &lt;/block&gt;</pre>
&nbsp;
<h3>Stage 1通关测试</h3>
<pre class="lang:default decode:true">    int
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
    }</pre>
&nbsp;

![][22]
<h3>加入基带信号生成部分</h3>
在lib/RemoteCarBaseBand_impl.cc里加入代码<code> </code>
<pre class="lang:default decode:true">RemoteCarIIBaseBand_impl::RemoteCarIIBaseBand_impl(double samp_rate,bool run, int command)
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
    }</pre>
<h3>回调函数</h3>
如果没有回调函数，那么生成的模块不能在gnuradio-companion里被WX GUI Slider实时的修改参数。

为了能够实时地控制小车，我们需要加入两个回调函数。

在lib/RemoteCarBaseBand_impl.h里加入set_run和set_command函数的声明
<pre class="lang:default decode:true">namespace gr {
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
    ....</pre>
还需要在include/remotecar/RemoteCarIIBaseBand.h加入set_run和set_command的声明
<pre class="lang:default decode:true">   class REMOTECAR_API RemoteCarIIBaseBand : virtual public gr::sync_block
    {
     public:
      typedef boost::shared_ptr sptr;

      static sptr make(double samp_rate,bool run, int command);
      virtual void set_run(bool run) = 0;
      virtual void set_command(int command) = 0 ;
    };</pre>
在lib/RemoteCarIIBaseBand_impl.cc里加入set_run和set_command的实现
<pre class="lang:default decode:true">    void RemoteCarIIBaseBand_impl::set_run(bool run) {
        bool_run = run;
    }

    void RemoteCarIIBaseBand_impl::set_command(int command) {
        n_command = command;
    }</pre>
最后，在grc/remotecar_RemoteCarIIBaseBand.xml文件里加入
<pre class="lang:default decode:true">  remotecar.RemoteCarIIBaseBand($samp_rate,$run, $command)
  set_run($run)
  set_command($command)
  ....</pre>
<h3>编译</h3>
<pre class="lang:default decode:true">mkdir build
cd build
cmake ../ &amp;&amp; make &amp;&amp; sudo make install &amp;&amp; sudo ldconfig</pre>
然后重启gnuradio-companion即可搭建一个简单的示例来跑通小车

![][23]
<h3>加入一个简单的GUI</h3>
如果觉得这样操作不舒服，我们可以用Qt写一个简单的键盘控制的方向盘。

![][24]
<h3>gr_modtool 用法概览</h3>
<pre class="lang:default decode:true">gr_modtool newmod blahblah
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
Python: gr_modtool add -t sync -l python square3_ff</pre>
&nbsp;
