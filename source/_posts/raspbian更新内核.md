---
title: raspbian更新内核
date: 2017-01-21 17:11:13
tags:
---




##获取升级所需源码

###1)下载地址：

官方网址：[raspberrypi](https://github.com/raspberrypi)


`firmware`:树莓派的交叉编译好的二进制内核、模块、库、bootloader

`linux`:内核源码

`tools`:编译内核和其他源码所需的工具——交叉编译器等

我们只需要以上三个文件即可，下面的工程可以了解一下

`documentation`:树莓派离线帮助文档，教你如何使用、部署树莓派（树莓派官方使用教程）

`userland`：arm端用户空间的一些应用库的源码——vc视频硬浮点、EGL、mmal、openVG等

`hats：Hardware Attached on Top`，树莓派 B+型板子的扩展板资料

`maynard`：一个gtk写成的桌面环境

`scratch`：一个简易、可视化编程环境

`noobs`:一个树莓派镜像管理工具，他可以让你在一个树莓派上部署多个镜像

`weston`：一个应用程序

`target_fs`：树莓派最小文件系统，使用busybox制作

`quake3`：雷神之锤3有线开发源码firmwareb




点到所需要下载的工程，左上角选版本，右方有一个download ZIP按钮可直接下载（笔者下载完成后，在Linux中解压提示出错，windows又非常慢切内核建议不要在windows环境解压，所以笔者不建议使用这种办法）

##使用Git下载
	$ mkdir raspeberrypi_src
	$ cd raspberrypi_src
	$ git clone git://github.com/raspberrypi/firmware.git
	$ git clone git://github.com/raspberrypi/linux.git
	$ git clone git://github.com/raspberrypi/tools.git

会得到三个文件夹：
firmware linux tools

##编译、提取内核及其模块

###获得内核配置文件
在运行的树莓派中运行：

	$ls /proc/

可看到一个叫config.gz的文件，他是当前的树莓派配置选项记录文件，我们将他拷出，放入我们的内核源码目录树下

	$cp /proc/config /home/pi
我们这里使用前面交过的samba拷出并拷入内核源码目录下，不熟悉的人可参考前面文章

在linux内核源码下执行：

	$zcat config.gz > .config



##配置、编译内核
###修改内核源码makefile ARCH类型和编译器路径
$vi Makefile +195
找到以上类似代码，改为

	ARCH		?= arm
	CROSS_COMPILE	?= ../tools/arm-bcm2708/arm-bcm2708hardfp-linux-gnueabi/bin/arm-bcm2708hardfp-linux-gnueabi-

###查看、修改配置选项
	$make menuconfig


如果不做修改，直接选中exit即可（注意使用键盘操作）

###编译内核镜像
	$make
在arch/arm/boot目录下可以看到一个叫zImage的文件，就是我们新的内核

但是树莓派需要另外一种格式的镜像，需要进行处理一下，执行以下命令

	$cd tools/mkimage

	$./imagetool-uncompressed.py ../../linux/arch/arm/boot/zImage
即可在当前文件夹下看到一个叫：kernel.img的文件，就是我们需要的新内核了

###提取modules
上一步其实不但编译出来了内核的源码，一些模块文件也编译出来了，这里我们提取一下

	$cd raspberrypi_src
	$mkdir modules
	$cd linux
	$ make modules_install INSTALL_MOD_PATH=../modules
即可在modules得到我们需要的模块文件

##升级RPi的kernel、Firmware、lib
将SD卡拔下插在电脑上（可使用读卡器）
###升级内核
将新编好的内核拷入SD卡，改名为：kernel_new.img
打开boot目录下
找到config.txt文件，加入：kernel=kernel_new.img这一行

###升级boot
将firmware/boot/目录下 以下文件拷入SD卡boot目录：fbootcode.bin fixup.dat fixup_cd.dat start.elf

###更新vc库及内核modules
编译出来的modules/lib/modules拷入树莓派文件系统/lib下