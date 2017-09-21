---
title: hikey替换android内核
date: 2017-01-21 17:18:49
tags:
---

##配置编译器
	mkdir ~/arm64-tc
	
###输入如下命令下载编译需要用到的组件压缩包 
	wget https://releases.linaro.org/14.09/components/toolchain/binaries/
	gcc-linaro-aarch64-linux-gnu-4.9-2014.09_linux.tar.xz
	
###然后输入如下命令解压上面下载的压缩包 
	
	tar --strip-components=1 -C ~/arm64-tc -xf gcc-linaro-aarch64-linux-gnu-4.9-2014.09_linux.tar.xz
	
###然后设置环境变量 
	
	export PATH=~/arm64-tc/bin:$PATH

##目前有两种版本选择，一种为Landing Team版本，另外一种为Reference Platform Build版本 

##下载Landing Team版本内核源码 
	git clone https://github.com/96boards/linux.git
	
	然后输入如下命令检出最新内核版本15.11 
	
	cd linux
	git checkout -b working-hikey 96boards-hikey-15.11

##如果使用Reference Platform Build版本内核源码，输入如下命令下载 

	git clone https://github.com/rsalveti/linux.git
	
	然后输入如下命令检出最新内核 
	
	git checkout reference-hikey-rebase

##最后输入如下命令开始编译内核 

	export ARCH=arm64
	export CROSS_COMPILE=aarch64-linux-gnu-
	export LOCALVERSION="-linaro-hikey"
	make distclean 
	make defconfig 
	make -j8 Image modules hi6220-hikey.dtb 2>&1 | tee build-log.txt

##使用以下命令编译内核模块 

	export PWD=`pwd`
	export INSTALL_MOD_PATH="$PWD/installed-modules"
	mkdir $INSTALL_MOD_PATH
	make -j8 modules_install




生成如下文件

	arch/arm64/boot/dts/hisilicon/hi6220-hikey.dtb
	arch/arm64/boot/Image-dtb

复制到前面新建的目录下面，即AOSP的/kernel/hikey-linaro目录。

确保我们此时的环境是HiKey的编译环境，即

	$ source build/envsetup.sh 
	$ lunch hikey-userdebug
最后，编译boot.img文件

	$ make bootimage -j4

这一步时间很短，我花了13S就完成了。

其实会生成2个文件，即 
/out/target/product/hikey/boot.img 
和 
/out/target/product/hikey/ramdisk.img
刷机

有了前面编译AOSP并且刷机的经验，这一过程就比较简单了。

根据使用HiKey进行开发里所说的，我们首先关机状态下将j15的跳线5-6闭合。然后开机进入fastboot模式。

然后将设备连接至虚拟机，可以使用如下命令查看设备是否已连接：

	$ fastboot devices


连接上之后，我们只刷boot分区，命令如下(假定你在AOSP根目录)

	fastboot flash boot out/target/product/hikey/boot.img

大约不到2s，刷机完成。

##问题与解决

刷机完成之后，开机进入系统，发现Kernel version的信息和之前的没有变化。也就是说我们刚才刷入的依然是Google prebuilt的kernel。

首先我对比了新生成的boot.img文件和之前的boot.img文件(幸好虚拟机有备份)，发现md5一模一样，也就是说并没有生成新的boot.img文件。

回想之前编译完成kernel之后，要手动复制2个文件到指定目录(hi6220-hikey.dtb和Image-dtb)。在AOSP根目录使用find命令搜索这两个文件，发现除了out目录和我们前边创建的/kernel/hikey-linaro目录外，在下面的目录还有这两个文件

/device/linaro/hikey-kernel

其实这个目录里的文件就是Google prebuilt的kernel文件。 
我们将前边编译生成的hi6220-hikey.dtb和Image-dtb文件复制到上面的目录中(你可以将原来的文件备份到一个新建的目录里边以防止刷编译的kernel失败之后可以快速切换回来)。

然后，我们再重新执行上面的编译boot.img文件步骤，生成新的boot.img文件。 
由于前面的失败经历，这次长了个心眼，将新老boot.img文件的md5对比了一下，发现确实不一样。

真正编译成功后我们再执行前面的刷机步骤，开机之后发现Kernel version的信息确实变了：
