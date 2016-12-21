---
date: 2016-06-17
title: "使用国内的镜像源来加速PyBOMBS安装GNURadio"
layout: post
author: scateu
---

 - <http://mirrors.tuna.tsinghua.edu.cn/help/pybombs>
 - <http://mirrors.aliyun.com/pybombs>

PyBOMBS 镜像使用帮助
====================

[PyBOMBS](http://gnuradio.org/redmine/projects/pybombs/wiki) (Python Build Overlay Managed Bundle System) 是 [GNU Radio](http://gnuradio.org/) 的包管理系统。

从头开始一键安装GNU Radio在Thinkpad X230上实测大约只需要40分钟，下载非常快，主要的时间就剩编译了。 而且安装的都是最新的版本。

另外，PyBOMBS会帮你解决依赖的问题，省得每次敲一堆make cmake命令了。

以前自己拖代码回来经常会被重置，而且耗时要几个小时。


**使用示例**

	sudo pip install pybombs
	rm -rf ~/.pybombs
	pybombs recipes add gr-recipes git+https://mirrors.tuna.tsinghua.edu.cn/pybombs/recipes/gr-recipes.git
	pybombs recipes add gr-etcetera git+https://mirrors.tuna.tsinghua.edu.cn/pybombs/recipes/gr-etcetera.git
	mkdir gnuradio-prefix
	cd gnuradio-prefix
	pybombs prefix init
	pybombs install gnuradio
	. ./setup_env.sh
	gnuradio-companion

	pybombs install rtl-sdr hackrf bladerf gr-osmosdr gr-bluetooth gr-ieee-80211

**更新**

由于 PyBOMBS 的 recipes 只能通过 git 仓库进行发布。而我们暂时不想维护一个复杂的 git 分支合并历史。所以更新时，需要将 recipe 仓库删除，然后再重新添加回来。(见[讨论](http://lists.gnu.org/archive/html/discuss-gnuradio/2016-06/msg00170.html))

	pybombs recipes remove gr-recipes
	pybombs recipes remove gr-etcetera
	pybombs recipes add gr-recipes git+https://mirrors.tuna.tsinghua.edu.cn/pybombs/recipes/gr-recipes.git
	pybombs recipes add gr-etcetera git+https://mirrors.tuna.tsinghua.edu.cn/pybombs/recipes/gr-etcetera.git

 - 本镜像使用 <http://github.com/scateu/pybombs-mirror> 脚本进行构建。


感谢[清华大学TUNA镜像源](https://mirrors.tuna.tsinghua.edu.cn)和[阿里云开源镜像站](http://mirrors.aliyun.com)提供镜像支持。


## Extra: USRP B200

在安装完uhd:

```bash
pybombs install uhd
```

之后，还需要做以下事情: ([参考](https://rfpoweramp.com/2014/06/10/setting-up-the-usrp-b200/))

```bash
. ./setup_env.sh
uhd_images_downloader   #下载固件


# 安装udev rules 否则会提示: USB open failed: insufficient permissions.
sudo cp ./lib/uhd/utils/uhd-usrp.rules /etc/udev/rules.d/99-uhd-usrp.rules
sudo udevadm control --reload-rules #重新载入
sudo udevadm trigger                #触发一下，省去插拨USB
```

