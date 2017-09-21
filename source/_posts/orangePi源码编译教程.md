---
title: orangePi源码编译教程
date: 2017-02-14 21:36:30
tags:
---

#环境
	ubuntu 12.04.5 64位 8G内存 
	jdk-6u45-linux-x64.bin 64位JDK6
	板子是 orangePC pc
如果内存不够会编译不成功，安装好jdk6配置好环境变量
  
[jdk网盘下载链接](http://pan.baidu.com/s/1eR8pJKA) 密码：jkei
#linux源码编译
下载源码压缩包

网址[http://www.orangepi.org/downloadresources/](http://www.orangepi.org/downloadresources/)

###解压源码
tar -xzvf h3-lichee-1.0.tar.gz
解压出 lichee目录，进入该目录

buildroot: 工程编译脚本
brandy: boot，uboot 源码以及开源交叉编译工具 gcc-linaro
linux-3.4: 内核源码
tools： 工程编译工具
build.sh: 编译脚本
###执行编译指令
在lichee目录执行以下指令

	$ ./build.sh configA

选择
此时系统会提示芯片的选择，对于 OrangePi PC ，选择 sun8iw7p1

此时系统会提示平台的选择，对于 OrangePi PC，选择 dragonboard

此时系统会调试板子的选择，对于 OrangePi PC，选择 dolphin-p1

编译成功后会如下显示

![view](http://ohjvpki1b.bkt.clouddn.com/orangepi_PC_build_OK.png)

###内核镜像文件和库的替换
编译完成之后，将会目录下生成如下文件：

boot： /lichee/tools/pack/chips/sun8iw7p1/bin/boot0_sdcard_sun8iw7p1.bin

uboot： /lichee/tools/pack/chips/sun8iw7p1/bin/u-boot-sun8iw7p1.bin

uImage： /lichee/out/sun8iw7p1/dragonboard/common/uImage

libs： /lichee/linux-3.4/output/lib/modules

将以上生成文件替换原有系统目录下相应的文件

源码网盘下载地址

[百度网盘链接](http://pan.baidu.com/s/1jIp0XkM) 密码：lh9l


#android源码编译

[android源码链接](http://pan.baidu.com/s/1skMg0Lv) 密码：9zsj

[makefile文件链接](http://pan.baidu.com/s/1qYrq9eG) 密码：x9ov

###安装相应软件包
	$ sudo apt-get install git gnupg flex bison gperf build-essential \
	zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
	libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
	libgl1-mesa-dev g++-multilib mingw32 tofrodos \
	python-markdown libxml2-utils xsltproc zlib1g-dev:i386

	$ sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so

###解压源码
	创建目录
	mkdir H3
	移动文件
	mv H3-homlet-1.0.tar.gz ./H3
	解压
	tar -xzvf H3-homlet-1.0.tar.gz
解压完之后会得到两个目录

	android lichee

###lichee源码编译

	$ cd lichee
	$ ./build.sh lunch
![view](http://ohjvpki1b.bkt.clouddn.com/build_lichee.png)

编译成功后打印信息

![view](http://ohjvpki1b.bkt.clouddn.com/build_lichee_ok.png)

###android代码编译

	$ cd android
	$ source ./build/envsetup.sh
	$ lunch dolphin_fvd_p1-eng #选择方案号
	$ extract-bsp #拷贝内核及驱动模块

由于源码解压出来没有makefile文件，需要将makefile文件拷到目录下才能执行make命令
	
	$ make –j8 #后面的数值为同时编译的进程，依赖于主机的配置
	$ pack #打包生成固件

![view](http://ohjvpki1b.bkt.clouddn.com/OrangePi_build_android_pack.png)

