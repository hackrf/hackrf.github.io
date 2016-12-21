---
title: "bladeRF 固件与 FPGA 注记"
date: 2014-08-27
author: alick
layout: post
---

使用 bladeRF 板卡时我们会遇到两个“镜像”：固件 (firmware) 镜像与 FPGA 镜像。二者是两个不同的概念。但是业界叫法不一，有时候会把二者混为一谈。一般而言，固件指的是嵌入到硬件设备中的软件，存放在只读存储器 (ROM) 或者闪存 (flash) 中，一般不易修改，修改的操作称为“刷新”(flashing)。固件这个名词最初和微代码相关，不过 bladeRF 里源代码是嵌入式 C 程序。FPGA 全名为可编程门阵列，其门电路、寄存器连接可以编程重构，其源程序一般是硬件描述语言 (HDL)，通过综合 (synthesis) 等步骤得到二进制文件。在 bladeRF 板卡上，FPGA 只是一块 Altera 芯片。在没有内置非挥发存储时，FPGA 镜像需要每次上电时重新加载，bladeRF 就是这种情况。所以在拿到板卡时，上面已有固件，但还没有 FPGA 镜像。下面本文会具体说明在使用 bladeRF 时如何刷新固件、加载/更新 FPGA 镜像、以及如何自动加载 FPGA 镜像。注意，有时为了避免混淆，会称 FPGA 镜像为 FPGA 比特流 (bitstream)，或者 FPGA 配置（因为它就是配置了门电路等组件的连接）。

## 刷新固件

注意：刷新固件前请先取消 FPGA 自动加载，以避免可能的冲突。FPGA 自动加载的细节参见下文。

Nuand 官方提供固件的源码，我们可以自行编译，不过这需要一套嵌入式开发工具链。Nuand 也提供构建好的[固件镜像](http://www.nuand.com/fx3.php)，可以直接下载使用。要刷新固件，只需在命令行执行：

```bash
bladeRF-cli -f bladeRF_fw_vX.Y.Z.img -v verbose
```
其中 X.Y.Z 为具体的版本号。命令完成后，需要断电重启了 bladeRF。然后可以用 `bladeRF-cli` 工具检查：

```bash
$ bladeRF-cli -e version

  bladeRF-cli version:        0.11.1-git-c631100
  libbladeRF version:         0.16.2-git-c631100

  Firmware version:           1.7.1-git-ca697ee
  FPGA version:               Unknown (FPGA not loaded)
```

看下其中 Firmware version（固件版本）是否更新。

事实上，还有另外一种刷新固件的方法，是通过进入设备的启动加载 (Bootloader) 模式（类似 Android 的 Recovery），输入一些命令完成的。不过这种方式步骤繁琐，一般不推荐使用，建议在上述简单方法遇到错误时考虑采用。具体步骤参加[维基](https://github.com/Nuand/bladeRF/wiki/Upgrading-bladeRF-firmware)。

## 加载 FPGA 镜像

注意到上面的示例中，FPGA version（FPGA 版本）显示为 Unknown（未知），FPGA 未加载。目前 bladeRF 使用两种 FPGA。要加载正确的 FPGA 镜像，首先需要确定手头 bladeRF 板卡的 FPGA 尺寸。它可以根据当初购买的价格判断，但更靠谱的方法是使用命令行查看：

```bash
$ bladeRF-cli -i
bladeRF> info

  Serial #:                 4f977f01eec48f5068c2ee3aeba41ba9
  VCTCXO DAC calibration:   0x8b63
  FPGA size:                40 KLE
  FPGA loaded:              yes
  USB bus:                  4
  USB address:              5
  USB speed:                SuperSpeed
  Backend:                  libusb
  Instance:                 0
```

交互模式下用 info 命令，或者直接在命令行下 `bladeRF-cli -e info`，在输出中寻找 FPGA size，就可以看到 FPGA 尺寸信息。这里示例显示板卡使用 40 KLE FPGA。[事实上](https://github.com/Nuand/bladeRF/wiki/Getting-Started%3A-Verifying-Basic-Device-Operation#Loading_the_FPGA)，FPGA 尺寸还可以通过查看 FPGA 芯片上的 EP4CExxxF23C8N 字样中 xxx 的部分来获得。

Nuand 官方提供了预先构建的 [FPGA 镜像](http://www.nuand.com/fpga.php)，免除用户手工编译之苦。40 KLE FPGA 对应 hostedx40.rbf 文件，以此类推。要加载 FPGA 镜像，只需使用命令`bladeRF-cli -l /path/to/fpga/file`，或者交互模式下 `load fpga /path/to/fpga/file`，其中 `/path/to/fpga/file` 为 FPGA 镜像所在路径。FPGA 镜像成功加载后，板卡上的三个 LED 灯会亮起，交互模式下 version 命令可以看到 FPGA 版本不再是未知。

不过，正如前文所说，每次重新加电后，FPGA 镜像都需要重新加载。下面说下如何自动加载 FPGA 镜像。

## 自动加载 FPGA 镜像

bladeRF [维基](https://github.com/Nuand/bladeRF/wiki/FPGA-Autoloading)上提供了两种自动加载 FPGA 镜像的方法。第一种方法基于主机软件，libbladeRF 在打开设备时，会在如下目录自动搜索合适的 FPGA 镜像：

 - `$HOME/.config/Nuand/bladeRF/`
 - `$HOME/.Nuand/bladeRF/`
 - `/etc/Nuand/bladeRF/`
 - `/usr/share/Nuand/bladeRF/`

只需将下载的 FPGA 镜像文件放在上述目录之一（没有可以新建之），即可实现 FPGA 镜像的自动加载。

另一种方式是将 FPGA 镜像写入设备的 SPI 闪存。这种方式的好处是写入后就不再依赖主机，从而可以实现脱机运行。不过这种方式比较慢，加载 x40 镜像需要大约 4 秒钟。注意绝对不要在加载完成，三个 LED 灯亮起前试图使用设备。要使用这种方式，需要执行下面命令：

    bladeRF-cli -L /path/to/fpga/file

一般说来，如果没有脱机工作的需求，还是推荐第一种自动加载方法。

前面提到，刷新固件时最好取消自动加载。对于第一种方法，只需把 FPGA 镜像文件临时移走。第二种方法，则需要执行命令 `bladeRF-cli -L X`，以擦除写入的 FPGA 镜像。

## 致谢

本工作由[星天科技](http://www.startest.cn/)赞助。作者:[Alick](https://wp-awesome.rhcloud.com/2014/08/27/bladerf-firmware-fpga-notes/)。
