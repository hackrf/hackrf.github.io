---
ID: 1032
title: "LTE小区搜索程序新增bladeRF硬件支持(LTE-Cell-Scanner supports bladeRF)"
author: jxj
date: 2014-11-02 20:09:40
post_excerpt: ""
layout: post
published: true
views:
  - "5323"
duoshuo_thread_id:
  - "1312073613704167537"
---

(关于一些基本的软件无线电概念，Linux操作，代码库git操作，以及软件包安装（apt-get）等，参见：<a href="http://sdr-x.github.io/rtl-sdr-rtl2832%E7%94%B5%E8%A7%86%E6%A3%92%E8%B7%9F%E8%B8%AA%E9%A3%9E%E6%9C%BAstep-by-step%E6%95%99%E7%A8%8B(tutorial%20ADS-B%20aircraft%20tracking%20by%20rtl-sdr%20rtl2832%20gr-air-modes)/">这篇文章</a>)After <a href="https://github.com/JiaoXianjun/LTE-Cell-Scanner">LTE-Cell-Scanner</a> supports rtlsdr hackRF, now it<a href="http://sdr-x.github.io/LTE-Cell-Scanner%20supports%20bladeRF%20now%20%28LTE%E5%B0%8F%E5%8C%BA%E6%90%9C%E7%B4%A2%E7%A8%8B%E5%BA%8F%E6%96%B0%E5%A2%9EbladeRF%E7%A1%AC%E4%BB%B6%E6%94%AF%E6%8C%81%29/"> supports bladeRF</a>! Here is part of README:


-----

An OpenCL accelerated TDD/FDD LTE Scanner (from rtlsdr/hackRF/bladeRF A/D samples to PDSCH output and RRC SIB messages decoded). By Jiao Xianjun (<a href="mailto:putaoshu@gmail.com">putaoshu@gmail.com</a>). Tech blog: <a href="http://sdr-x.github.io">http://sdr-x.github.io</a>


## New features, make and Usages

### 0x01. basic method to build program 

(针对不同硬件的编译开关)

	mkdir build
	cd build
	cmake ../                   -- default for rtlsdr and OpenCL ON;   OR
	cmake ../ -DUSE_BLADERF=1   -- build for bladeRF;    OR
	cmake ../ -DUSE_HACKRF=1    -- build for hackRF
	cmake ../ -DUSE_OPENCL=0    -- disable OpenCL (See notes in later chapter)
	make

CellSearch and LTE-Tracker program will be generated in build/src. Use "--help" when invoke program to see all options.

(You may need some related libraries, such as itpp, fftw, libboost-, Curses, ... etc.)

### 0x02. basic usage 

(If you have OpenCL, make sure those .cl files in LTE-Cell-Scanner/src have been copy to program directory) （使用方法，以及输出示例）

#### 0x02.1 

	CellSearch --freq-start 1890000000 (try to search LTE Cell at 1890MHz)

output:

	...
	Detected a TDD cell! At freqeuncy 1890MHz, try 0
	cell ID: 253
	PSS ID: 1
	RX power level: -17.0064 dB
	residual frequency offset: -48.0366 Hz
		    k_factor: 1
	...
	Detected the following cells:
	Meaning -- DPX:TDD/FDD; A: #antenna ports C: CP type ; P: PHICH duration ; PR: PHICH resource type
	DPX  CID  A     fc  freq-offset RXPWR  C   nRB  P   PR  CrystalCorrectionFactor
	TDD  253  2  1890M         -48h   -17  N  100   N  1/2   0.99999997458380551763

#### 0x02.2 

	LTE-Tracker -f 1890000000 (try to track LTE Cell at 1890MHz)

#### 0x02.3 LTE\_DL\_receiver 

(Matlab script. Decode RRC SIB message in PDSCH by reading captured signal bin file)

#### 0x02.4 LTE\_DL\_receiver 1890 40 40 

(Matlab script. Decode SIB at 1890MHz lively with LNA VGA gain of hackRF 40dB 40dB)

output:

	...
	TDD SFN-864 ULDL-2-|D|S|U|D|D|D|S|U|D|D| CID-216 nPort-2 CP-normal PHICH-DUR-normal-RES-1
	SF0 PHICH1 PDCCH1 RNTI: 
	...
	SF4 PHICH1 PDCCH1 RNTI: SI-RNTI SI-RNTI 
	PDCCH   No.0  4CCE: Localized VRB from RB0 to RB11 MCS-7 HARQ-0 NEWind-0 RV-0 TPC-1 DAI-0
	Calling asn1c decoder (../asn1_test/LTE-BCCH-DL-SCH-decode/progname) for BCCH-DL-SCH-Message.
	../asn1_test/LTE-BCCH-DL-SCH-decode/progname tmp_sib_info.per -p BCCH-DL-SCH-Message
	<BCCH-DL-SCH-Message>
	    <message>
		<c1>
		    <systemInformation>
			<criticalExtensions>
			    <systemInformation-r8>
				<sib-TypeAndInfo>
					<sib2>
					    <radioResourceConfigCommon>
						<rach-ConfigCommon>
						    <preambleInfo>
							<numberOfRA-Preambles><n52/></numberOfRA-Preambles>
	...

See complete README here: <https://github.com/JiaoXianjun/LTE-Cell-Scanner>

See video outside China: <http://youtu.be/rg6ENh-tbJY>

See video in China: <http://v.youku.com/v_show/id_204158978.html>
