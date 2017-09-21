---
title: poky raspbian开发
date: 2017-01-21 17:10:37
tags:
---


##安基本包

Ubuntu and Debian

     $ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat libsdl1.2-dev xterm
                        
Fedora

     $ sudo dnf install gawk make wget tar bzip2 gzip python3 unzip perl patch \
     diffutils diffstat git cpp gcc gcc-c++ glibc-devel texinfo chrpath \
     ccache perl-Data-Dumper perl-Text-ParseWords perl-Thread-Queue perl-bignum socat \
     findutils which SDL-devel xterm
                        
OpenSUSE

     $ sudo zypper install python gcc gcc-c++ git chrpath make wget python-xml \
     diffstat makeinfo python-curses patch socat libSDL-devel xterm
                        
CentOS

     $ sudo yum install gawk make wget tar bzip2 gzip python unzip perl patch \
     diffutils diffstat git cpp gcc gcc-c++ glibc-devel texinfo chrpath socat \
     perl-Data-Dumper perl-Text-ParseWords perl-Thread-Queue SDL-devel xterm
                        

##同步poky源代码
	git clone https://git.yoctoproject.org/git/poky
进入poky目录同步树莓派代码

	git clone https://git.yoctoproject.org/git/meta-raspberrypi

##初始化环境
	source oe-init-build-env raspberrypi

说明：指定目录raspberrypi

##编辑配置文件

	vim ./conf/local.conf 

将

	MACHINE ??= "raspberrypi2"
注释

	#PACKAGECONFIG_append_pn-qemu-native = " sdl"
	#PACKAGECONFIG_append_pn-nativesdk-qemu = " sdl"
	#ASSUME_PROVIDED += "libsdl-native"

文件末尾增加
	GPU_MEN = "16"

说明：16为CPU核数

	vim ./conf/bblayers.conf
修改成以下

	BBLAYERS ?= " \
	  /home/gsta/liang/poky/meta \
	  /home/gsta/liang/poky/meta-yocto \
	  /home/gsta/liang/poky/meta-yocto-bsp \
	  /home/gsta/liang/poky/meta-raspberrypi \
	  "
	##增加树莓派目录
	BBLAYERS_NON_REMOVABLE ?= " \
	  /home/gsta/liang/poky/meta \
	  /home/gsta/liang/poky/meta-yocto \
	  "
	

##生成
	bitbake rpi-basic-image

注意：同步过程很漫长，同步过程中一定要开启git代理和终端代理

整个同步下来大概有18G


##错误处理

	bitbake complains if run as root
	
	root@eb4b9143265d:/work/build-test01# bitbake -k core-image-sato
	ERROR:  OE-core's config sanity checker detected a potential misconfiguration.
	    Either fix the cause of this error or at your own risk disable the checker (see sanity.conf).
	    Following is the list of potential problems / advisories:
	
	    Do not use Bitbake as root.
	ERROR: Execution of event handler 'check_sanity_eventhandler' failed
	ERROR: Command execution failed: Exited with 1
	
	Summary: There were 3 ERROR messages shown, returning a non-zero exit code.
	root@eb4b9143265d:/work/build-test01#
	Workaround:
	
	$ touch conf/sanity.conf