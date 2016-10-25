---
ID: 969
title: "HACKRF BTLE/BT4.0 低功耗蓝牙 无线 发包器 （可模拟iBeacon、建链、通信等）"
author: jxj
date: 2014-08-05 16:30:05
post_excerpt: ""
layout: post
published: true
views:
  - "6461"
duoshuo_thread_id:
  - "1312073613704167531"
---
<a href="http://sdr-x.github.io/HACKRF%20BladeRF%20BTLE%20BT4.0%20%E6%97%A0%E7%BA%BF%E5%8F%91%E5%8C%85%E5%99%A8%28iBeacon,%20Link%20setup,%20etc.%29/">一个基于HACKRF板子的BTLE/BT4.0无线发包器</a>。(iBeacon/packet-sniffer 验证通过)

源代码：<!--more--> https://github.com/JiaoXianjun/ （BTLE项目）

(关于一些基本的软件无线电概念，Linux操作，代码库git操作，以及软件包安装（apt-get）等，参见：<a href="http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B(tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes)/">这篇文章</a>)

所有链路层包格式都支持（详见 Chapter 2&amp;3, PartB, Volume 6, Core_V4.0.pdf 标准可自行搜索下载） 。

可用来发射你想要的任意BTLE包（支持RAW格式，即直接裸GFSK调制），比如iBeacon包，TI网站上（http://processors.wiki.ti.com/index.php/BLE_sniffer_guide

）给的建链过程包 ，等。配合Packet Sniffer，你想干什么都可以。

视频演示： <http://v.youku.com/v_show/id_XNzUxMDIzNzAw.html>

## build方法

```
cd host
mkdir build
cd build
cmake ../
make
sudo make install (or not install, just use btle_tx in hackrf-tools/src)
```

### 使用方法1：

```
btle_tx packet1 packet2 ... packetX ... rN
```

### 使用方法2：

```
btle_tx packets.txt
```

方法二只是把方法1的参数放到一个文本文件当中，每个参数一行，以#开头的表示注释行。参见hackrf-tools/src里的例子packets.txt。

“packetX”是描述包的字符串。所有包组成一个包序列。

“rN”表示该序列重复发送N次。如果不写rN，则默认发送一次。

### 包描述字符串格式 (“packetX”)

```
channel_number-packet_type-field-value-field-value-...-Space-value
```

每个字符串以信道号开头（0～39信道），接下来是包格式关键字（RAW/iBeacon/ADV_IND/ADV_DIRECT_IND/etc，参见最后的例子） ，接下来是字段“名-值”对，最后一个字段“名-值”是Space-xxx，表示本包后等待xxx毫秒，再发送下一个包。因此可以构成任意发包序列/方案。各字段之间以“-”连接。

### iBeacon包例子

（iBeacon原理：http://www.warski.org/blog/2014/01/how-ibeacons-work/ ）

```
./btle_tx 37-iBeacon-AdvA-010203040506-UUID-B9407F30F5F8466EAFF925556B57FE6D-Major-0008-Minor-0009-TxPower-C5-Space-100 r100
```

以上命令以100ms间隔重复发送iBeacon包，可以在iPhone或者iPad下载“Locate”软件来看发出的iBeacon信息。包描述字符串解释：

```
37-iBeacon-AdvA-010203040506-UUID-B9407F30F5F8466EAFF925556B57FE6D-Major-0008-Minor-0009-TxPower-C5-Space-100
```

 - 37 -- 信道号(BTLE广播信道号为37 38 39)
 - iBeacon -- 包格式关键字. (实际上是标准中的 ADV_IND 格式，见 Core_V4.0.pdf)
 - AdvA -- 广播地址 (即自己的MAC地址) 这里设置为 010203040506 (可用packet sniffer验证)
 - UUID -- 设置为 Estimote公司 固定 UUID: B9407F30F5F8466EAFF925556B57FE6D
 - Major -- major number of iBeacon format. (这里设置为 0008)
 - Minor -- minor number of iBeacon format. (这里设置为 0009)
 - Txpower -- 发射功率 (这里设置为 C5)
 - Space -- 这里设置为包间隔100ms

### TI网站上连接建立的例子

http://processors.wiki.ti.com/index.php/BLE_sniffer_guide

```
./btle_tx 37-ADV_IND-TxAdd-0-RxAdd-0-AdvA-90D7EBB19299-AdvData-0201050702031802180418-Space-1000 37-CONNECT_REQ-TxAdd-0-RxAdd-0-InitA-001830EA965F-AdvA-90D7EBB19299-AA-60850A1B-CRCInit-A77B22-WinSize-02-WinOffset-000F-Interval-0050-Latency-0000-Timeout-07D0-ChM-1FFFFFFFFF-Hop-9-SCA-5-Space-1000 9-LL_DATA-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-DATA-X-CRCInit-A77B22-Space-1000
```

以上命令模拟设备1和设备2建立连接，并可被packet sniffer捕捉到（packet sniffer设置为37号广播信道，最后会根据建链过程携带的配置信息自动跳到9号数据信道接收到数据包）

第一包：设备1在37号信道发送广播包 ADV_IND，携带自己MAC 地址 。

第二包：设备2在扫描状态收到设备1的广播包后，发送建链请求包 CONNECT_REQ，携带一些建链信息，例如：自身MAC地址（InitA），建链目标MAC地址（AdvA），接入地址（AA，供对方将来使用），CRC初始化值（供对方将来使用），跳频信道map和方案（ChM和Hop字段）供对方选择数据信道，等。

第三包：设备1根据设备2的建链请求携带的信息，在数据信道9上发送一个链路层空数据包，表示建链完成。DATA字段后的单个“X”表示无数据。

三包时间间隔1s（1000ms）。

支持的所有包格式描述字符串举例：

### RAW packets

(All bits will be sent to GFSK modulator directly)

13-RAW-AAD6BE898E5F134B5D86F2999CC3D7DF5EDF15DEE39AA2E5D0728EB68B0E449B07C547B80EAA8DD257A0E5EACB0B

#### ADVERTISING CHANNEL packets:

```
37-IBEACON-AdvA-010203040506-UUID-B9407F30F5F8466EAFF925556B57FE6D-Major-0008-Minor-0009-TxPower-C5
37-ADV_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-AdvData-00112233445566778899AABBCCDDEEFF
37-ADV_DIRECT_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-InitA-0708090A0B0C
37-ADV_NONCONN_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-AdvData-00112233445566778899AABBCCDDEEFF
37-ADV_SCAN_IND-TxAdd-1-RxAdd-0-AdvA-010203040506-AdvData-00112233445566778899AABBCCDDEEFF
37-SCAN_REQ-TxAdd-1-RxAdd-0-ScanA-010203040506-AdvA-0708090A0B0C
37-SCAN_RSP-TxAdd-1-RxAdd-0-AdvA-010203040506-ScanRspData-00112233445566778899AABBCCDDEEFF
37-CONNECT_REQ-TxAdd-1-RxAdd-0-InitA-010203040506-AdvA-0708090A0B0C-AA-01020304-CRCInit-050607-WinSize-08-WinOffset-090A-Interval-0B0C-Latency-0D0E-Timeout-0F00-ChM-0102030405-Hop-3-SCA-4
```

#### DATA CHANNEL packets:

```
9-LL_DATA-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-DATA-X-CRCInit-A77B22
9-LL_CONNECTION_UPDATE_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-WinSize-02-WinOffset-000F-Interval-0050-Latency-0000-Timeout-07D0-Instant-0000-CRCInit-A77B22
9-LL_CHANNEL_MAP_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-ChM-1FFFFFFFFF-Instant-0001-CRCInit-A77B22
9-LL_TERMINATE_IND-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-ErrorCode-00-CRCInit-A77B22
9-LL_ENC_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-Rand-0102030405060708-EDIV-090A-SKDm-0102030405060708-IVm-090A0B0C-CRCInit-A77B22
9-LL_ENC_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-SKDs-0102030405060708-IVs-01020304-CRCInit-A77B22
9-LL_START_ENC_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
9-LL_START_ENC_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
9-LL_UNKNOWN_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-UnknownType-01-CRCInit-A77B22
9-LL_FEATURE_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-FeatureSet-0102030405060708-CRCInit-A77B22
9-LL_FEATURE_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-FeatureSet-0102030405060708-CRCInit-A77B22
9-LL_PAUSE_ENC_REQ-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
9-LL_PAUSE_ENC_RSP-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-CRCInit-A77B22
9-LL_VERSION_IND-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-VersNr-01-CompId-0203-SubVersNr-0405-CRCInit-A77B22
9-LL_REJECT_IND-AA-60850A1B-LLID-1-NESN-0-SN-0-MD-0-ErrorCode-00-CRCInit-A77B22
```
