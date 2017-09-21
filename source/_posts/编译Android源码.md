---
title: 编译Android源码
date: 2016-12-19 21:29:48
tags:
---

##编译版本要求

![](http://ohjvpki1b.bkt.clouddn.com/%E7%BC%96%E8%AF%91%E7%89%88%E6%9C%AC%E8%A6%81%E6%B1%82.png)

##基本安装环境

###ubuntu 14.04 64

	sudo apt-get install git-core gnupg flex bison gperf build-essential \
	  zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
	  lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache \
	  libgl1-mesa-dev libxml2-utils xsltproc unzip

###ubuntu 16.04 64

	sudo apt-get install libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev g++-multilib 
	sudo apt-get install -y git flex bison gperf build-essential libncurses5-dev:i386 
	sudo apt-get install tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386 
	sudo apt-get install dpkg-dev libsdl1.2-dev libesd0-dev
	sudo apt-get install git-core gnupg flex bison gperf build-essential  
	sudo apt-get install zip curl zlib1g-dev gcc-multilib g++-multilib 
	sudo apt-get install libc6-dev-i386 
	sudo apt-get install lib32ncurses5-dev x11proto-core-dev libx11-dev 
	sudo apt-get install libgl1-mesa-dev libxml2-utils xsltproc unzip m4
	sudo apt-get install lib32z-dev ccache

##配置环境变量
	source build/envsetup.sh
##设置编译选项
	lunch aosp_hammerhead-userdebug

##编译
	make -j4
make失败或停止后，可以使用make -k 继续编译


##运行模拟器
在编译完成之后,就可以通过以下命令运行Android虚拟机了,
命令如下:

	source build/envsetup.sh

	lunch(选择刚才你设置的目标版本,比如这里了我选择的是2

	emulator
如果你是在编译完后立刻运行虚拟机,由于我们之前已经执行过source及lunch命令了,因此现在你只需要执行命令就可以运行虚拟机:



既然谈到了模拟器运行,这里我们顺便介绍模拟器运行所需要四个文件:

	Linux Kernel
	system.
	img
	userdate
	.img
	ramdisk.img

如果你在使用lunch命令时选择的是aosp_arm-eng,
那么在执行不带参数的emualtor命令时,Linux Kernel默认使用的是/source/prebuilds/qemu-kernel/arm/kernel-qemu目录下的kernel-qemu文件
;而android镜像文件则是默认使用source/out/target/product/generic目录下的system.img,userdata.img和ramdisk.img,也就是我们刚刚编译出来的镜像文件.


上面我在使用lunch命令时选择的是aosp_arm64-eng,因此linux默认使用的/source/prebuilds/qemu-kernel/arm64/kernel-qemu下的kernel-qemu,
而其他文件则是使用的source/out/target/product/generic64目录下的system.img,userdata.img和ramdisk.img.
当然,emulator指令允许你通过参数制定使用不同的文件,具体用法可以通过emulator --help查看

##模块编译
除了通过make命令编译可以整个android源码外,
Google也为我们提供了相应的命令来支持单独模块的编译.


编译环境初始化(即执行source build/envsetup.sh)之后,我们可以得到一些有用的指令,
除了上边用到的lunch,还有以下:

  
	- croot: Changes directory to the top of the tree.
	 
	- m: Makes from the top of the tree.
	  
	- mm: Builds all of the modules in the current directory.
	  
	- mmm: Builds all of the modules in the supplied directories.
	 
	- cgrep: Greps on all local C/C++ files.
	- jgrep: Greps on all local Java files.
	  
	- resgrep: Greps on all local res/*.xml files.
	  
	- godir: Go to the directory containing a file.

其中mmm指令就是用来编译指定目录.通常来说,每个目录只包含一个模块.比如这里我们要编译Launcher2模块,
执行指令:

mmm packages/apps/Launcher2/
稍等一会之后,
如果提示:

	### make completed success fully ###

即表示编译完成,
此时在out/target/product/gereric/system/app就可以看到编译的Launcher2.apk文件了.


重新打包系统镜像
编译好指定模块后,如果我们想要将该模块对应的apk集成到系统镜像中,
需要借助make snod指令重新打包系统镜像,这样我们新生成的system.img中就包含了刚才编译的Launcher2模块了.
重启模拟器之后生效.

单独安装模块
我们在不断的修改某些模块,总不能每次编译完成后都要重新打包system.img
,然后重启手机吧?有没有什么简单的方法呢?
在编译完后,借助adb install命令直接将生成的apk文件安装到设备上即可,
相比使用make snod,会节省很多事件.



##补充

我们简单的来介绍out/target/product/generic/system目录下的常用目录:
Android系统自带的apk文件都在out/target/product/generic/system/apk目录下;

一些可执行文件(比如C编译的执行),放在out/target/product/generic/system/bin目录下;

动态链接库放在out/target/product/generic/system/lib目录下;

硬件抽象层文件都放在out/targer/product/generic/system/lib/hw目录下.




##SDK编译


如果你需要自己编译SDK使用,很简单,只需要执行命令`make sdk`即可.



##编译fastboot adb

	make fastboot adb


##BUILD
指的是特定功能的组合的特定名称,即表示编译出的镜像可以运行在什么环境.
其中,aosp(Android Open Source Project)代表Android开源项目;arm表示系统
是运行在arm架构的处理器上,arm64则是指64位arm架构;处理器,x86则表示x86架
构的处理器;此外,还有一些单词代表了特定的Nexus设备,下面是常用的设备代码和
编译目标,更多参考官方文档

	|受型号|设备代码|编译目标
	
	
	|Nexus 6P|angler|aosp_angler-userdebug|
	
	|Nexus 5X|bullhead|aosp_bullhead-userdebug|
	
	|Nexus 6|shamu|aosp_shamu-userdebug|
	
	|Nexus 5|hammerhead|aosp_hammerhead-userdebug|


##BUILD TYPE
则指的是编译类型,通常有三种:

	-user:代表这是编译出的系统镜像是可以用来正式发布到市场的版本,
	其权限是被限制的(如,没有root权限,不能dedug等)
	
	-userdebug:在user版本的基础上开放了root权限和debug权限.
	
	-eng:代表engineer,也就是所谓的开发工程师的版本,拥有最大的权限(root等),
	此外还附带了许多debug工具


##清除编译

	make clobber

##理解 Android Build 系统
[外部文档](http://ohjvpki1b.bkt.clouddn.com/%E7%90%86%E8%A7%A3%20Android%20Build%20%E7%B3%BB%E7%BB%9F.pdf)


