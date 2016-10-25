---
ID: 390
title: "修改build-gnuradio免翻墙免炖github本地安装"
author: paud
date: 2014-03-20 08:38:21
post_excerpt: ""
layout: post
published: true
views:
  - "4980"
duoshuo_thread_id:
  - "1312073613704167476"
---
1.将<a title="测试HackRF One的Ubuntu虚拟机" href="http://www.hackrf.net/2014/03/%e6%b5%8b%e8%af%95hackrf-one%e7%9a%84ubuntu%e9%95%9c%e5%83%8f/" target="_blank">VMWare镜像</a>中的~/sdr/ 复制到你的linux中

2.将以下文本替换掉build-gnuradio里的内容，没错
<pre>echo 'simpower91 modified here'
echo 'Do you want to skip downloading from the internet?[yes]'
read simskip
if [ "$simskip"a = 'no'a ]
then
gitfetch
fi
echo 'simpower91 modified end'</pre>
这部分代码位于function all之中，这就是本大侠改动的地方了。

其实在all之中还有一段重要的代码，如下
<pre>for fcn in uhd_build firmware gnuradio_build rtl_build extras mod_groups mod_udev mod_sysctl pythonpath
do
my_echo Starting function $fcn at: `date`
cd $CWD
$fcn
my_echo Done function $fcn at: `date`
done</pre>
高手写的代码条理就是清晰，每个安装包给出一个单独的function
如果你想跳过一些包，修改这里即可。

3.将所有目录中的MAKECache.txt删掉

4.执行./build-gnuradio

5.如果觉得麻烦，我会将所有打包放在云盘里，<a target="_blank">http://pan.baidu.com/s/1i3uV8sH 点击下载</a>

完整shell代码如下
<pre>#!/bin/bash
#
# Build script for UHD+GnuRadio on Fedora and Ubuntu
#
#
#
# Updates: https://github.com/guruofquality/grextras/wiki
# Updates: https://github.com/balint256/gr-baz.git
#
#
#
# Exit function--Id like to be able to upload stats to a website somewhere...
#
function doexit
{
echo -n "Send success/fail info to sbrac.org?"
read ans
case $ans in
y|Y|YES|yes|Yes)
wget http://www.sbrac.org/bgrstats.php?status="$1"?systype=${SYSTYPE}?sysinfo="`uname -a`" &gt;/dev/null 2&gt;&amp;1
echo Thanks
;;
esac
exit
}
function help {
cat &lt;&lt;!EOF!

Usage: build-gnuradio [--help|-h] [-v|--verbose] [-jN] [-ja]
[-l|--logfile logfile ] [-u|--users ulist] [-m] funcs

-m - Use HEAD of master branch, rather than 3.7.1. Use compatible extras as well
-o - User v3.6.5.1 and compatible extras.

-v|--verbose - turn on verbose logging to stdout

-jN - have make use N concurrent jobs

-ja - have make use N concurrent jobs with auto setting of N
(based on number of cpu cores on build system)

-josh - use Josh Blums enhanced Gnu Radio GIT repo

-u|--users ul - add comma-separated users to 'usrp' group in addition
to calling user ( $USER )

-l|--logfile lf - log messages to 'lf'
-ut - set tag for UHD checkout to
-gt - set tag for Gnu Radio checkout to
-e|--extras - add an item to "extras" to be built after Gnu Radio/UHD/gs-osmosdr
available funcs are:

all - do all functions
prereqs - install prerequisites
gitfetch - use GIT to fetch Gnu Radio and UHD
uhd_build - build only UHD
firmware - fetch firmware/FPGA
gnuradio_build - build only Gnu Radio
mod_groups - modify the /etc/groups and add user to group 'usrp'
mod_udev - add UDEV rule for USRP1
mod_sysctl - modify SYSCTL for larger net buffers
!EOF!

}

if [ $USER = root -o $UID -eq 0 ]
then
echo Please run this script as an ordinary user
echo it will acquire root privileges as it needs them via \"sudo\".
exit
fi

VERBOSE=No
JFLAG=""
LOGDEV=/dev/null
USERSLIST=None
JOSHMODE=False
UTAG=None
GTAG=None
export LC_LANG=C
EXTRAS=""
MASTER_MODE=0
OLD_MODE=0
PULLED_LIST="gnuradio uhd rtl-sdr gr-osmosdr gr-iqbal hackrf gr-baz bladeRF"
which python3 &gt;/dev/null 2&gt;&amp;1
if [ $? -eq 0 ]
then
CMAKE_FLAG1=-DPythonLibs_FIND_VERSION:STRING="2.7"
CMAKE_FLAG2=-DPythonInterp_FIND_VERSION:STRING="2.7"
fi
while :
do
case $1 in
-ja)
cnt=`grep 'processor.*:' /proc/cpuinfo|wc -l`
cnt=`expr $cnt - 1`
if [ $cnt -lt 1 ]
then
cnt=1
fi
JFLAG=-j$cnt
shift
;;

-j[123456789])
JFLAG=$1
shift
;;

-josh)
JOSHMODE=True
shift
;;

-v|--verbose)
LOGDEV=/dev/stdout
shift
;;

-l|--logfile)
case $2 in
/*)
LOGDEV=$2
;;
*)
LOGDEV=`pwd`/$2
;;
esac
shift
shift
rm -f $LOGDEV
echo $LOGDEV Starts at: `date` &gt;&gt;$LOGDEV 2&gt;&amp;1
;;

-u|--users)
USERSLIST=$2
shift
shift
;;

-m|--master)
MASTER_MODE=1
shift
;;

-o|--old)
OLD_MODE=1
shift
;;

-h|--help)
help
exit
;;

-ut)
UTAG=$2
shift
shift
;;

-gt)
GTAG=$2
shift
shift
;;

-e|--extras)
EXTRAS=$EXTRAS" "$2
shift 2
;;

-*)
echo Unrecognized option: $1
echo
help
exit
break
;;
*)
break
;;
esac
done

CWD=`pwd`
SUDOASKED=n
SYSTYPE=unknown
good_to_go=no
for files in /etc/fedora-release /etc/linuxmint/info /etc/lsb-release /etc/debian_version /etc/redhat-release
do
if [ -f $file ]
then
good_to_go=yes
fi
done
if [ $good_to_go = no ]
then
echo Supported systems: Fedora, Ubuntu, Redhat, Debian, Mint, OpenSuse
echo You appear to be running none of the above, exiting
exit
fi

echo This script will install Gnu Radio from current GIT sources
echo You will require Internet access from the computer on which this
echo script runs. You will also require SUDO access. You will require
echo approximately 500MB of free disk space to perform the build.
echo " "
echo This script will, as a side-effect, remove any existing Gnu Radio
echo installation that was installed from your Linux distribution packages.
echo It must do this to prevent problems due to interference between
echo a linux-distribution-installed Gnu Radio/UHD and one installed from GIT source.
echo " "
echo The whole process may take up to two hours to complete, depending on the
echo capabilities of your system.
echo
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
echo NOTE: if you run into problems while running this script, you can re-run it with
echo the --verbose option to produce lots of diagnostic output to help debug problems.
echo This script has been written to anticipate some of the more common problems one might
echo encounter building ANY large, complex software package. But it is not pefect, and
echo there are certainly some situations it could encounter that it cannot deal with
echo gracefully. Altering the system configuration from something reasonably standard,
echo removing parts of the filesystem, moving system libraries around arbitrarily, etc,
echo it likely cannot cope with. It is just a script. It isn\'t intuitive or artificially
echo intelligent. It tries to make life a little easier for you, but at the end of the day
echo if it runs into trouble, a certain amount of knowledge on your part about
echo system configuration and idiosyncrasies will inevitably be necessary.
echo
echo
echo -n Proceed?

read ans
case $ans in
y|Y|YES|yes|Yes)
PROCEED=y
;;
*)
exit
esac

SPACE=`df $HOME| grep -v blocks|grep '%'`
SPACE=`echo $SPACE | awk '/./ {n=NF-2; printf ("%d\n", $n/1.0e3)}'`

if [ $SPACE -lt 500 ]
then
echo "You don't appear to have enough free disk space on $HOME"
echo to complete this build/install
echo exiting
doexit DISKSPACE
fi

total=0
for file in $PULLED_LIST
do
found=0
for instance in ${file}.20*
do
if [ -d $instance ]
then
found=1
sz=`du -s $instance|awk '{print $1}'`
total=`expr $total + $sz`
fi
done
done
total=`expr $total '*' 1024`
total=`expr $total / 1000000`
if [ $total -gt 100 ]
then
echo Your old 'uhd.*' and 'gnuradio.*' etc directories are using roughly $total MB
echo of disk space:
for file in $PULLED_LIST
do
for instance in ${file}.20*
do
if [ -d $instance ]
then
ls -ld $instance
fi
done
done
echo " "
echo -n Remove them'?'
read ans

case $ans in
y|Y|YES|yes|Yes)
for file in $PULLED_LIST
do
for instance in ${file}.20*
do
if [ -d $instance ]
then
echo removing ${instance}
rm -rf ${instance}
fi
done
done
echo Done
;;
esac
fi
rm -rf *.20*.bgmoved

function my_echo {
if [ $LOGDEV = /dev/stdout ]
then
echo $*
else
echo $*
echo $* &gt;&gt;$LOGDEV 2&gt;&amp;1
fi
}

function checkcmd {
found=0
which $1 &gt;/dev/null 2&gt;&amp;1
x=$?
if [ $x -eq 0 ]
then
found=1
fi
for place in /bin /usr/bin /usr/local/bin /sbin /usr/sbin /usr/local/sbin /opt/bin /opt/local/bin
do
if [ -e $place/$1 ]
then
found=1
fi
done
if [ $found -eq 0 ]
then
which $1 &gt;/dev/null 2&gt;&amp;1
if [ $? -eq 0 ]
then
found=1
fi
fi
if [ $found -eq 0 ]
then
my_echo Failed to find just-installed command \'$1\' after pre-requisite installation.
my_echo This very likely indicates that the pre-requisite installation failed
my_echo to install one or more critical pre-requisites for Gnu Radio/UHD
doexit PREREQFAIL-CMD-$1
fi
}

function checklib {
found=0
my_echo -n Checking for library $1 ...
for dir in /lib /usr/lib /usr/lib64 /lib64 /usr/lib/x86_64-linux-gnu /usr/lib/i386-linux-gnu \
/usr/lib/arm-linux-gnueabihf /usr/lib/arm-linux-gnueabi
do
for file in $dir/${1}*.so*
do
if [ -e "$file" ]
then
found=1
fi
done
done
if [ $found -le 0 ]
then
my_echo Failed to find libraries with prefix \'$1\' after pre-requisite installation.
my_echo This very likely indicates that the pre-requisite installation failed
my_echo to install one or more critical pre-requisites for Gnu Radio/UHD
my_echo exiting build
doexit PREREQFAIL-LIB-$1
else
my_echo Found library $1
fi
}

function checkpkg {
my_echo Checking for package $1
if [ `apt-cache search $1 | wc -l` -eq 0 ]
then
my_echo Failed to find package \'$1\' in known package repositories
my_echo Perhaps you need to add the Ubuntu universe or multiverse PPA?
my_echo see https://help.ubuntu.com/community/Repositories/Ubuntu
my_echo exiting build
doexit PREREQFAIL-PKG-$1
fi
}

function prereqs {
sudocheck
my_echo Installing prerequisites.
my_echo "====&gt;" THIS MAY TAKE QUITE SOME TIME "&lt;=====" # # It's a Fedora system # if [ -f /etc/fedora-release ] then SYSTYPE=Fedora case `cat /etc/fedora-release` in *12*|*13*|*14*|*15*|*16*|*17*|*18*) sudo yum -y erase 'gnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y erase 'libgruel-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y erase 'libgruel*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y groupinstall "Engineering and Scientific" "Development Tools" &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y install fftw-devel cppunit-devel wxPython-devel \
boost-devel alsa-lib-devel numpy gsl-devel python-devel pygsl \
python-cheetah python-lxml PyOpenGL qt-devel qt qt4 qt4-devel \
PyQt4-devel qwt-devel qwtplot3d-qt4-devel libusb-devel libusb \
libusb1 libusb1-devel cmake git wget python-docutils \
PyQwt PyQwt-devel qwt-devel gtk2-engines xmlrpc-c-"*" tkinter orc \
orc-devel python-sphinx SDL-devel swig &gt;&gt;$LOGDEV 2&gt;&amp;1
;;

*19*|*20*)
sudo yum -y erase 'gnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y erase 'libgruel-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y erase 'libgruel*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y groupinstall "Engineering and Scientific" "Development Tools" &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y install fftw-devel cppunit-devel wxPython-devel \
boost-devel alsa-lib-devel numpy gsl-devel python-devel pygsl \
python-cheetah python-lxml PyOpenGL qt-devel qt qt4 qt4-devel \
PyQt4-devel qwt-devel qwtplot3d-qt4-devel \
libusbx-devel cmake git wget python-docutils \
PyQwt PyQwt-devel qwt-devel gtk2-engines xmlrpc-c-"*" tkinter orc \
orc-devel python-sphinx SDL-devel swig &gt;&gt;$LOGDEV 2&gt;&amp;1
;;
*)
my_echo Only Fedora release 12 - 20 are supported by this script
doexit WRONGRELEASE
;;
esac

#
# It's a RedHat system
elif [ -f /etc/redhat-release ]
then
SYSTYPE=Redhat
case `cat /etc/redhat-release` in
*elease*6*)
sudo yum -y erase 'gnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y erase 'libgruel-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y erase 'libgruel*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y groupinstall "Engineering and Scientific" "Development Tools" &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo yum -y install fftw-devel cppunit-devel wxPython-devel libusb-devel \
boost-devel alsa-lib-devel numpy gsl-devel python-devel pygsl \
python-cheetah python-lxml PyOpenGL qt-devel qt qt4 qt4-devel \
PyQt4-devel qwt-devel qwtplot3d-qt4-devel libusb libusb-devel \
libusb1 libusb1-devel cmake git wget python-docutils \
PyQwt PyQwt-devel qwt-devel gtk2-engines xmlrpc-c-"*" tkinter orc &gt;&gt;$LOGDEV 2&gt;&amp;1
;;
*)
my_echo Your Redhat system release must be release 6 to proceed
doexit WRONGRELEASE
;;
esac

#
# It's a Mint system
#
elif [ -f /etc/linuxmint/info ]
then
SYSTYPE=Mint
sudo apt-get -y purge 'gnuradio-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel0*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'python-gnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
case `grep RELEASE /etc/linuxmint/info` in
*11*|*12*|*13*|*14*|*15*)
PKGLIST="libfontconfig1-dev libxrender-dev libpulse-dev swig g++
automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4
cmake git-core wget libxi-dev python-docutils gtk2-engines-pixbuf r-base-dev python-tk
liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2"
;;

*)
my_echo Your Mint release must be at least Linux Mint 11 to proceed
doexit WRONGRELEASE
;;
esac
for pkg in $PKGLIST; do checkpkg $pkg; done
sudo apt-get -y --ignore-missing install $PKGLIST &gt;&gt;$LOGDEV 2&gt;&amp;1

#
# It's a Ubuntu system
#
elif [ -f /etc/lsb-release -a ! -f /etc/SuSE-release ]
then
SYSTYPE=Ubuntu
sudo apt-get -y purge 'gnuradio-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel0*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'python-gnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
case `grep DISTRIB_RELEASE /etc/lsb-release` in
*13.*)
PKGLIST="libfontconfig1-dev libxrender-dev libpulse-dev swig g++
automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-default qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4
cmake git-core wget libxi-dev python-docutils gtk2-engines-pixbuf r-base-dev python-tk
liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2"
CMAKE_FLAG1=-DPythonLibs_FIND_VERSION:STRING="2.7"
CMAKE_FLAG2=-DPythonInterp_FIND_VERSION:STRING="2.7"
;;

*11.*|*12.10*)
PKGLIST="libfontconfig1-dev libxrender-dev libpulse-dev swig g++
automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4
cmake git-core wget libxi-dev python-docutils gtk2-engines-pixbuf r-base-dev python-tk
liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2"
CMAKE_FLAG1=-DPythonLibs_FIND_VERSION:STRING="2.7"
CMAKE_FLAG2=-DPythonInterp_FIND_VERSION:STRING="2.7"
;;

*12.04*)
PKGLIST="libfontconfig1-dev libxrender-dev libpulse-dev swig g++
automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost1.48-all-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4
cmake git-core wget libxi-dev python-docutils gtk2-engines-pixbuf r-base-dev python-tk
liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2"
;;

*10.04*|*10.10*)
PKGLIST="libfontconfig1-dev libxrender-dev libpulse-dev swig g++
automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4
cmake git-core wget python-docutils gtk2-engines-pixbuf python-tk libasound2-dev python-gtk2"
;;

*9.10*)
PKGLIST="swig g++ automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost1.38-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools
cmake git wget python-docutils python-tk libasound2-dev"
;;

*9.04*)
sudo apt-get purge python-wxgtk2.6
PKGLIST="swig g++ automake1.9 autoconf libtool python-dev fftw3-dev
libcppunit-dev libboost1.35-dev libusb-dev libusb-1.0-0-dev
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools
cmake git wget python-docutils python-tk libasound2-dev"
;;

*)
my_echo Your Ubuntu release must be at least 9.04 to proceed
doexit WRONGRELEASE
;;
esac
for pkg in $PKGLIST; do checkpkg $pkg; done
my_echo Done checking packages
sudo apt-get -y --ignore-missing install $PKGLIST &gt;&gt;$LOGDEV 2&gt;&amp;1
#
# It's a Debian system
#
elif [ -f /etc/debian_version ]
then
SYSTYPE=Debian
sudo apt-get -y purge 'gnuradio-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel-*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgruel0*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'libgnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo apt-get -y purge 'python-gnuradio*' &gt;&gt;$LOGDEV 2&gt;&amp;1
case `cat /etc/debian_version` in
*6.0*|*wheezy*|*sid*|*7.1*|*7.0*|*7.2*|*7.3*|*7.4*)
PKGLIST="libfontconfig1-dev libxrender-dev libpulse-dev swig g++
automake autoconf libtool python-dev libfftw3-dev
libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77
libsdl1.2-dev python-wxgtk2.8 git-core
libqt4-dev python-numpy ccache python-opengl libgsl0-dev
python-cheetah python-lxml doxygen qt4-dev-tools libusb-1.0-0-dev
libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4
cmake git-core wget libxi-dev python-docutils gtk2-engines-pixbuf r-base-dev python-tk
liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2 libportaudio2 portaudio19-dev
ca-certificates"
;;
*)
my_echo Unsupported Debian version `cat /etc/debian_version`
doexit WRONGRELEASE
;;
esac
for pkg in $PKGLIST; do checkpkg $pkg; done
sudo apt-get -y --ignore-missing install $PKGLIST &gt;&gt;$LOGDEV 2&gt;&amp;1
elif [ -f /etc/SuSE-release ]
then
SYSTYPE=OpenSuSE
if grep -q 12.1 /etc/SuSE-release
then
cool=1
else
my_echo Only OpenSuSE 12.1 is supported by this script
doexit WRONGRELEASE
fi
sudo zypper install cmake cppunit-devel doxygen fftw3-devel \
git gsl-devel libjack-devel libqt4-devel libqwtplot3d-devel \
libSDL-devel libusb-1_0-devel orc portaudio portaudio-devel \
python-cheetah python-devel python-lxml python-wxGTK python-wxWidgets-devel \
qwt-devel wxWidgets-devel boost-devel xmlto texlive-latex &gt;&gt;$LOGDEV 2&gt;&amp;1

else
my_echo This script supports only Ubuntu,Mint,Fedora, Debian and OpenSuse systems
my_echo Your Fedora system must be at least Fedora 12
my_echo Your Ubuntu system must be at least Ubuntu 9.04
my_echo Your Mint system must be at least Mint 11
my_echo Your Debian system must be release 6
doexit WRONGSYSTEM
fi
PATH=$PATH
export PATH

checkcmd git
checkcmd cmake

if [ $SYSTYPE = Fedora ]
then
checklib libusb-0 0
checklib libusb-1 0
else
checklib libusb 2
fi

checklib libboost 5
checklib libcppunit 0
checklib libfftw 5
checklib libgsl 0

my_echo Done
}

function gitfetch {
date=`date +%Y%m%d%H%M%S`
V=3.7/maint
if [ $MASTER_MODE -eq 1 ]
then
V=Master/HEAD
fi
if [ $OLD_MODE -eq 1 ]
then
V=3.6.5.1
fi
echo This script will fetch Gnu Radio version $V from the repositories, along with compatible
echo extras.
echo -n Is this OK?
read ans

case $ans in
Y|y|YES|yes|Yes)
;;
*)
exit
;;
esac

my_echo "Fetching various packages (Gnu Radio, UHD, gr-osmosdr, gr-iqbal, etc)"
my_echo " via the Internet"
my_echo "=======&gt; THIS MAY TAKE QUITE SOME TIME &lt;=========" cd $CWD for dir in ${PULLED_LIST} do if [ -d $dir ] then mv $dir ${dir}.$date fi done # # GIT the gnu radio source tree # my_echo -n Fetching Gnu Radio via GIT... if [ $JOSHMODE = False ] then #git clone --progress http://gnuradio.org/git/gnuradio.git &gt;&gt;$LOGDEV 2&gt;&amp;1
#if [ ! -d gnuradio/gnuradio-core -a ! -d gnuradio/gnuradio-runtime ]
#then
# my_echo "Could not find gnuradio/gnuradio-{core,runtime} after GIT checkout"
# my_echo GIT checkout of Gnu Radio failed!
# doexit FETCH-GR-FAIL
#fi
if [ $OLD_MODE -eq 1 ]
then
cd gnuradio
git checkout v3.6.5.1 &gt;&gt;$LOGDEV 2&gt;&amp;1
cd $CWD
elif [ $MASTER_MODE -eq 0 ]
then
cd gnuradio
git checkout maint &gt;&gt;$LOGDEV 2&gt;&amp;1
cd $CWD
fi

if [ $GTAG != None ]
then
cd gnuradio
git checkout $GTAG &gt;/dev/null 2&gt;&amp;1
git name-rev HEAD &gt;tmp$$ 2&gt;&amp;1
if grep -q "HEAD tags/$GTAG" tmp$$
then
whee=yes
rm -f tmp$$
else
my_echo Could not fetch Gnu Radio tagged $GTAG from GIT
rm -f tmp$$
doexit FETCH-GR-FAIL-$GTAG
fi
cd ..
fi
else
git clone --progress git://gnuradio.org/jblum.git &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ $GTAG != "None" ]
then
git checkout $GTAG
else
git checkout next
fi
if [ ! -d jblum/gnuradio-core ]
then
my_echo GIT checkout of Gnu Radio from jblum.git failed!
doexit FETCH-GR-JOSH-FAIL
fi
fi
my_echo Done

#
# GIT the UHD source tree
#
rm -rf uhd
my_echo -n Fetching UHD via GIT...
git clone --progress http://github.com/EttusResearch/uhd &gt;&gt;$LOGDEV 2&gt;&amp;1

if [ ! -d uhd/host ]
then
my_echo GIT checkout of UHD FAILED
rm -f tmp$$
doexit FETCH-UHD-FAIL
fi
if [ $UTAG != None ]
then
cd uhd
git checkout $UTAG &gt;/dev/null 2&gt;&amp;1
git status &gt;tmp$$ 2&gt;&amp;1
if grep -q "On branch $UTAG" tmp$$
then
whee=yes
rm -f tmp$$
else
my_echo Could not fetch UHD tagged $UTAG from GIT
rm -f tmp$$
fi
cd ..
fi

#
# GIT the RTL-SDR source tree
#
rm -rf rtl-sdr
rm -rf gr-osmosdr
rm -rf gr-baz
rm -rf hackrf
rm -rf bladeRF
my_echo Fetching rtl-sdr "(rtl-sdr, gr-osmosdr, gr-iqbal, hackrf and bladeRF)" via GIT
git clone --progress git://git.osmocom.org/rtl-sdr &gt;&gt;$LOGDEV 2&gt;&amp;1
git clone --progress git://git.osmocom.org/gr-osmosdr &gt;&gt;$LOGDEV 2&gt;&amp;1
git clone --progress git://git.osmocom.org/gr-iqbal.git &gt;&gt;$LOGDEV 2&gt;&amp;1
git clone https://github.com/Nuand/bladeRF.git &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ -d gr-iqbal ]
then
cd gr-iqbal
if [ $OLD_MODE -eq 1 ]
then
git checkout gr3.6
fi
git submodule init &gt;&gt;$LOGDEV 2&gt;&amp;1
git submodule update &gt;&gt;$LOGDEV 2&gt;&amp;1
cd ..
fi
git clone --progress http://github.com/mossmann/hackrf.git &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Done
}

function uhd_build {
#
# UHD build
#
sudocheck
if [ ! -d uhd ]
then
my_echo you do not appear to have the \'uhd\' directory
my_echo you should probably use $0 gitfetch to fetch the appropriate
my_echo files using GIT
doexit BUILD-UHD-NOT-THERE
fi
if [ $UTAG != None ]
then
cd uhd
git checkout $UTAG &gt;/dev/null 2&gt;&amp;1
cd ..
fi
my_echo Building UHD...
my_echo "=============&gt; THIS WILL TAKE SOME TIME &lt;=============" my_echo cd uhd/host if [ ! -d build ] then mkdir build fi cd build make clean &gt;/dev/null 2&gt;&amp;1
cmake $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 ../ &gt;&gt;$LOGDEV 2&gt;&amp;1
make clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make $JFLAG &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ $? -ne 0 ]
then
my_echo UHD build apparently failed
my_echo Exiting UHD build
doexit UHD-BUILD-FAIL1
fi
sudo make $JFLAG install &gt;&gt;$LOGDEV 2&gt;&amp;1
which uhd_find_devices &gt;/dev/null 2&gt;&amp;1
x=$?
if [ $x -ne 0 -a ! -f /usr/local/bin/uhd_find_devices -a ! -f /opt/local/bin/uhd_find_devices ]
then
my_echo UHD build/install apparently failed since I cannot find /usr/local/bin/uhd_find_devices
my_echo after doing make and make install
my_echo Exiting UHD build
doexit UHD-BUILD-FAIL2
fi
sudo ldconfig &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Done building/installing UHD
}

function rtl_build {
#
# RTL build
#
sudocheck
cd $CWD
if [ ! -d rtl-sdr ]
then
my_echo you do not appear to have the \'rtl-sdr\' directory
my_echo you should probably use $0 gitfetch to fetch the appropriate
my_echo files using GIT
doexit BUILD-RTL-NOT-THERE
fi

my_echo -n Building rtl-sdr...
cd rtl-sdr
cmake $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 . &gt;&gt;$LOGDEV 2&gt;&amp;1
make clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make $JFLAG &gt;&gt;$LOGDEV 2&gt;&amp;1

if [ $? -ne 0 ]
then
my_echo rtl-sdr build apparently failed
my_echo Exiting rtl-sdr build
doexit RTL-BUILD-FAIL1
fi
sudo make $JFLAG install &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Done building rtl-sdr

cd $CWD
if [ -d hackrf ]
then
my_echo -n Building hackrf...
cd hackrf
cmake $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 host/ &gt;&gt;$LOGDEV 2&gt;&amp;1
make clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ $? -ne 0 ]
then
my_echo hackrf build failed
my_echo Exiting hackrf build
else
sudo make install &gt;&gt;$LOGDEV 2&gt;&amp;1
fi
my_echo Done building hackrf
cd $CWD
fi
cd $CWD
if [ -d gr-iqbal ]
then
my_echo -n Building gr-iqbal...
cd gr-iqbal
mkdir -p build
cd build
cmake .. $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 &gt;&gt;$LOGDEV 2&gt;&amp;1
make clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ $? -ne 0 ]
then
my_echo gr-iqbal build apparently failed
my_echo Exiting gr-iqbal build
else
sudo make install &gt;&gt;$LOGDEV 2&gt;&amp;1
cd $CWD
my_echo Done building gr-iqbal
fi
fi
if [ -d bladeRF ]
then
my_echo -n Building bladeRF...
cd bladeRF
cd host
cmake . $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 &gt;&gt;$LOGDEV 2&gt;&amp;1
make clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ $? -ne 0 ]
then
my_echo bladeRF build apparently failed
my_echo Exiting bladeRF build
else
sudo make install &gt;&gt;$LOGDEV 2&gt;&amp;1
cd $CWD
my_echo Done building bladeRF
fi
fi

cd $CWD
if [ ! -d gr-osmosdr ]
then
my_echo you do not appear to have the \'gr-osmosdr\' directory
my_echo you should probably use $0 gitfetch to fetch the appropriate
my_echo files using GIT
doexit RTL-BUILD-FAIL2
fi
cd gr-osmosdr
if [ $OLD_MODE -eq 1 ]
then
git checkout gr3.6 &gt;/dev/null 2&gt;&amp;1
fi
my_echo -n Building gr-osmosdr...
cmake . $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 &gt;&gt;$LOGDEV 2&gt;&amp;1
make clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make $JFLAG &gt;&gt;$LOGDEV 2&gt;&amp;1

if [ $? -ne 0 ]
then
my_echo gr-osmosdr build apparently failed
my_echo Exit rtl-sdr/gr-osmosdr build
doexit OSMOSDR-BUILD-FAIL
fi
sudo make $JFLAG install &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Done building gr-osmosdr
sudo ldconfig &gt;&gt;$LOGDEV 2&gt;&amp;1

cd $CWD
my_echo Done building/installing rtl-sdr/gr-osmosdr
}

function firmware {
sudocheck
FOUND_DOWNLOADER=False
dirlist="/usr/local/share /usr/local/lib /usr/local/lib64"
prog=uhd_images_downloader.py

for dir in $dirlist
do
if [ -f $dir/uhd/utils/$prog ]
then
FOUND_DOWNLOADER=True
DOWNLOADER=$dir/uhd/utils/$prog
fi
done

if [ $FOUND_DOWNLOADER = True ]
then
sudo $DOWNLOADER
else
my_echo Could not find images downloader: $prog in any of $dirlist
doexit UHD-FIRMWARE-FAIL
fi
my_echo Done downloading firmware to /usr/local/share/uhd/images
}

function gnuradio_build {
sudocheck

if [ $JOSHMODE = False ]
then
if [ ! -d gnuradio ]
then
my_echo you do not appear to have the \'gnuradio\' directory
my_echo you should probably use $0 gitfetch to fetch the appropriate
my_echo files using GIT
doexit GNURADIO-BUILD-NOT-THERE
fi
if [ $GTAG != None ]
then
cd gnuradio
git checkout $GTAG &gt;/dev/null 2&gt;&amp;1
cd ..
fi
else
if [ ! -d jblum ]
then
my_echo you do not appear to have the \'jblum\' directory
my_echo you should probably use $0 gitfetch to fetch the appropriate
my_echo files using GIT
doexit GNURADIO-BUILD-NO-JOSH
fi
fi

#
# LD stuff
#
my_echo /usr/local/lib &gt;tmp$$
my_echo /usr/local/lib64 &gt;&gt;tmp$$

if grep -q /usr/local/lib /etc/ld.so.conf.d/*
then
my_echo /usr/local/lib already in ld.so.conf.d
else
sudo cp tmp$$ /etc/ld.so.conf.d/local.conf
fi
rm -f tmp$$
my_echo Doing ldconfig...
sudo ldconfig &gt;/dev/null 2&gt;&amp;1

PKG_CONFIG_PATH=/usr/local/lib/pkgconfig

if [ -d /usr/local/lib64/pkgconfig ]
then
PKG_CONFIG_PATH=/usr/local/lib64/pkgconfig
fi

export PKG_CONFIG_PATH

#
# Build Gnuradio
#
if [ $JOSHMODE = False ]
then
cd gnuradio
else
cd jblum
git checkout gr_basic &gt;&gt;$LOGDEV 2&gt;&amp;1
fi

my_echo Building Gnu Radio...
my_echo "=========&gt; THIS WILL TAKE QUITE A WHILE &lt;=============" my_echo " " my_echo ...Doing cmake if [ -d build ] then my_echo ...build directory already here else mkdir build fi cd build make clean &gt;/dev/null 2&gt;&amp;1
my_echo ...Cmaking
cmake -DENABLE_BAD_BOOST=ON -DCMAKE_BUILD_TYPE=RelWithDebInfo $CMAKE_FLAG1 $CMAKE_FLAG2 $CMF1 $CMF2 ../ &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo ...Building
make $JFLAG clean &gt;&gt;$LOGDEV 2&gt;&amp;1
make $JFLAG &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ $? -ne 0 ]
then
my_echo make failed
my_echo Exiting Gnu Radio build/install
doexit GNURADIO-BUILD-FAIL
fi
my_echo ...Installing
sudo rm -rf /usr/local/include/gnuradio/
sudo rm -f /usr/local/lib*/libgnuradio*
sudo make $JFLAG install &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo ldconfig &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Done building and installing Gnu Radio
my_echo -n GRC freedesktop icons install ...
if [ -f /usr/local/libexec/gnuradio/grc_setup_freedesktop ]
then
sudo chmod 755 /usr/local/libexec/gnuradio/grc_setup_freedesktop
sudo /usr/local/libexec/gnuradio/grc_setup_freedesktop install &gt;&gt;$LOGDEV 2&gt;&amp;1
fi
my_echo Done
}

function do_an_extra {
if [ -e $1 ]
then
my_echo Building extra module $1
cd $1
if [ -f CMakeLists.txt ]
then
mkdir -p build &gt;&gt;$LOGDEV 2&gt;&amp;1
cd build
cmake .. $CMAKE_FLAGS1 $CMAKE_FLAGS2 $CMF1 $CMF2 &gt;&gt;$LOGDEV 2&gt;&amp;1
make &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo make install &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo ldconfig
elif [ - bootstrap ]
then
chmod 755 bootstrap
./bootstrap &gt;&gt;$LOGDEV 2&gt;&amp;1
PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig
./configure &gt;&gt;$LOGDEV 2&gt;&amp;1
make &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo make install &gt;&gt;$LOGDEV 2&gt;&amp;1
sudo ldconfig
else
my_echo Couldnt determine how to make module $1 neither bootstrap nor CmakeLists.txt present
fi
else
my_echo Couldnt build module $1 directory not there
fi
}

function extras {
sudocheck
date=`date +%Y%m%d%H%M%S`
if [ ! "@$EXTRAS@" = "@@" ]
then
for module in $EXTRAS
do
cd $CWD
base=`basename $module .git`
case $module in
git:*|*.git)
mv $base $base.$date.bgmoved &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Doing GIT checkout for extra module $base
git clone $module &gt;&gt;$LOGDEV 2&gt;&amp;1
do_an_extra $base
;;
htt*:*svn*)
mv $base $base.$date &gt;&gt;$LOGDEV 2&gt;&amp;1
my_echo Doing SVN checkout for extra module $base
svn co $module &gt;&gt;$LOGDEV 2&gt;&amp;1
if [ -e $base/trunk ]
then
do_an_extra $base/trunk
else
do_an_extra $base
fi
;;
*)
my_echo Ignoring malformed extra module $module
;;
esac

done
fi
cd $CWD
}

function mod_groups {
sudocheck
#
# Post install stuff
#
# USRP rules for UDEV and USRP group
#
#
# Check for USRP group, and update if necessary
if grep -q usrp /etc/group
then
my_echo Group \'usrp\' already in /etc/group
else
sudo /usr/sbin/groupadd usrp
fi

#
# Check that our calling user is in the USRP group, update if necessary
#
if grep -q usrp.*${USER} /etc/group
then
my_echo User $USER already in group \'usrp\'
else
sudo /usr/sbin/usermod -a -G usrp $USER
cat &lt;&lt;"!EOF!" ******************************************************************************** This script has just modified /etc/group to place your userid '('$USER')' into group 'usrp' In order for this change to take effect, you will need to log-out and log back in again. You will not be able to access your USRP1 device until you do this. If you wish to allow others on your system to use the USRP1 device, you will need to use: sudo usermod -a -G usrp userid For each userid you wish to allow access to the usrp ******************************************************************************** Further !EOF! fi if [ "$USERSLIST" = None ] then foo=bar else ul=`echo $USERSLIST|sed -e 's/,/ /g'` for u in $ul do sudo /usr/sbin/usermod -a -G usrp $u my_echo Added $u to group usrp done fi } function mod_udev { sudocheck # # Check for UHD UDEV rules file, update if exists # if [ -f $CWD/uhd/host/utils/uhd-usrp.rules ] then sudo cp $CWD/uhd/host/utils/uhd-usrp.rules /etc/udev/rules.d/10-usrp.rules sudo chown root /etc/udev/rules.d/10-usrp.rules sudo chgrp root /etc/udev/rules.d/10-usrp.rules fi # # Check for rtl-sdr UDEV rules file, update if exists # rm -f tmp$$ if [ -f $CWD/rtl-sdr/rtl-sdr.rules ] then sudo cp $CWD/rtl-sdr/rtl-sdr.rules /etc/udev/rules.d/15-rtl-sdr.rules sudo chown root /etc/udev/rules.d/15-rtl-sdr.rules sudo chgrp root /etc/udev/rules.d/15-rtl-sdr.rules fi sudo killall -HUP udevd sudo udevadm control --reload-rules } function mod_sysctl { sudocheck # # Modify sysctl.conf as necessary # cat &gt;tmp$$ &lt;&lt;!EOF! # Updates for Gnu Radio net.core.rmem_max = 1000000 net.core.wmem_max = 1000000 kernel.shmmax = 2147483648 !EOF! if grep -q 'Updates for Gnu Radio' /etc/sysctl.conf then my_echo Required updates to /etc/sysctl.conf already in place else my_echo Applying updates to /etc/sysctl.conf cat /etc/sysctl.conf tmp$$ &gt;tmp2$$
chmod 644 tmp2$$
sudo mv tmp2$$ /etc/sysctl.conf
fi

sudo sysctl -w net.core.rmem_max=1000000 &gt;/dev/null 2&gt;&amp;1
sudo sysctl -w net.core.wmem_max=1000000 &gt;/dev/null 2&gt;&amp;1
sudo sysctl -w kernel.shmmax=2147483648 &gt;/dev/null 2&gt;&amp;1

rm -f tmp$$
rm -f tmp2$$

if grep -q usrp /etc/security/limits.conf
then
my_echo usrp group already has real-time scheduling privilege
else
cat &gt;tmp$$ &lt;&lt;!EOF! @usrp - rtprio 50 !EOF! cat /etc/security/limits.conf tmp$$ &gt;tmp2$$
sudo cp tmp2$$ /etc/security/limits.conf
sudo chmod 644 /etc/security/limits.conf
rm -f tmp$$ tmp2$$
my_echo Group \'usrp\' now has real-time scheduling privileges
my_echo You will need to log-out and back in again for this to
my_echo take effect
fi
}

function all {
my_echo Starting all functions at: `date`
cd $CWD
prereqs
touch -d "15 minutes ago" touch$$
if [ -d uhd -a -d gnuradio ]
then
if [ uhd -ot touch$$ -o gnuradio -ot touch$$ ]
then
echo 'simpower91 modified here'
echo 'Do you want to skip downloading from the internet?[yes]'
read simskip
if [ "$simskip"a = 'no'a ]
then
gitfetch
fi
echo 'simpower91 modified end'
else
my_echo Skipping git fetch, since \'uhd\' and \'gnuradio\' are new enough
fi
else
gitfetch
fi
rm -f touch$$
EXTRAS=$EXTRAS" http://github.com/balint256/gr-baz.git"
EXTRAS=$EXTRAS" http://github.com/guruofquality/grextras.git"
for fcn in uhd_build firmware gnuradio_build rtl_build extras mod_groups mod_udev mod_sysctl pythonpath
do
my_echo Starting function $fcn at: `date`
cd $CWD
$fcn
my_echo Done function $fcn at: `date`
done
my_echo Done all functions at: `date`
}

function pythonpath {
for PYVER in 2.6 2.7
do
for type in "" 64
do
if [ -d /usr/local/lib${type}/python${PYVER}/site-packages/gnuradio ]
then
PYTHONPATH=/usr/local/lib${type}/python${PYVER}/site-packages
fi
if [ -d /usr/local/lib${type}/python${PYVER}/dist-packages/gnuradio ]
then
PYTHONPATH=/usr/local/lib${type}/python${PYVER}/dist-packages
fi
done
done
echo
echo
echo "************************************************************"
echo You should probably set your PYTHONPATH to:
echo " "
echo " " $PYTHONPATH
echo " "
echo Using:
echo " "
echo export PYTHONPATH=$PYTHONPATH
echo " "
echo in your .bashrc or equivalent file prior to attempting to run
echo any Gnu Radio applications or Gnu Radio Companion.
echo "*************************************************************"
}

function sudocheck {
#
# Check SUDO privileges
#
if [ $SUDOASKED = n ]
then
echo SUDO privileges are required
echo -n Do you have SUDO privileges'?'
read ans
case $ans in
y|Y|YES|yes|Yes)
echo Continuing with script
SUDOASKED=y
sudo grep timestamp_timeout /etc/sudoers &gt;tmp$$
timeout=`cat tmp$$|awk '/./ {print $4}'`
rm -f tmp$$
if [ "@@" = "@$timeout@" ]
then
sudo cp /etc/sudoers tmp$$
sudo chown $USER tmp$$
sudo chmod 644 tmp$$
echo "Defaults timestamp_timeout = 90" &gt;&gt;tmp$$
sudo cp tmp$$ /etc/sudoers
sudo chown root /etc/sudoers
sudo chmod 440 /etc/sudoers
elif [ "$timeout" -lt 90 ]
then
echo You need to have a timestamp_timout in /etc/sudoers of 90 or more
echo Please ensure that your timestamp_timeout is 90 or more
exit
fi
;;
*)
echo Exiting. Please ensure that you have SUDO privileges on this system!
exit
;;
esac
fi
}

PATH=$PATH
export PATH
case $# in
0)
all
my_echo All Done
doexit SUCCESS
esac

for arg in $*
do
cd $CWD
$arg
done
my_echo All Done
doexit SUCCESS</pre>
