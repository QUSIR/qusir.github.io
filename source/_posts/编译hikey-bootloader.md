---
title: 编译hikey bootloader
date: 2017-01-21 17:20:17
tags:
---


##交叉编译工具链：
##下载：
	http://releases.linaro.org/15.02/components/toolch...
	http://releases.linaro.org/15.02/components/toolch...
##安装：
	mkdir arm-tc arm64-tc
	tar --strip-components=1 -C ${PWD}/arm-tc -xf gcc-linaro-4.9-*-x86_64_aarch64-linux-gnu.tar.xz
	tar --strip-components=1 -C ${PWD}/arm64-tc -xf gcc-linaro-4.9-*-x86_64_arm-linux-gnueabihf.tar.xz
	export PATH="${PWD}/arm-tc/bin:${PWD}/arm64-tc/bin:$PATH"

##bootloader代码下载:
	git clone -b hikey --depth 1 https://github.com/96boards/edk2.git linaro-edk2
	git clone -b hikey --depth 1 https://github.com/96boards-hikey/arm-trusted-firmware.git
	git clone -b hikey --depth 1 https://github.com/96boards/LinaroPkg.git
	git clone --depth 1 https://github.com/96boards/l-loader.git
	git clone git://git.linaro.org/uefi/uefi-tools.git

##编译UEFI
	export AARCH64_TOOLCHAIN=GCC49
	export EDK2_DIR=${PWD}/linaro-edk2
	export UEFI_TOOLS_DIR=${PWD}/uefi-tools
	
	cd ${EDK2_DIR}
	${UEFI_TOOLS_DIR}/uefi-build.sh -c ../LinaroPkg/platforms.config -b RELEASE -a ../arm-trusted-firmware hikey
	
	cd ../l-loader
	ln -s ${EDK2_DIR}/Build/HiKey/RELEASE_GCC49/FV/bl1.bin
	ln -s ${EDK2_DIR}/Build/HiKey/RELEASE_GCC49/FV/fip.bin
	arm-linux-gnueabihf-gcc -c -o start.o start.S
	arm-linux-gnueabihf-gcc -c -o debug.o debug.S
	arm-linux-gnueabihf-ld -Bstatic -Tl-loader.lds -Ttext 0xf9800800 start.o debug.o -o loader
	arm-linux-gnueabihf-objcopy -O binary loader temp
	python gen_loader.py -o l-loader.bin --img_loader=temp --img_bl1=bl1.bin
	sudo PTABLE=aosp-8g bash -x generate_ptable.sh
	python gen_loader.py -o ptable-aosp-8g.img --img_prm_ptable=prm_ptable.img

##镜像输出(l-loader目录下)：
fip.bin l-loader.bin ptable-aosp-8g.img

##烧写
	sudo python hisi-idt.py --img1=l-loader.bin -d /dev/ttyUSB0
	sudo fastboot flash ptable ptable-aosp-8g.img
	sudo fastboot flash fastboot fip.bin


注意:
这里编译的boot_fat.uefi.img要和上面编译的fip.bin一起烧写,否则无法启动

如果不想烧写上面编译的fip.bin的话则需要手动修改生成boot_fat.uefi.img：
手动打包过程如下(将你自己编译的内核,DT和RAMDISK替换进去下面下载的prebuild的boot_fat.uefi.img后就可以了):

	wget http://builds.96boards.org/releases/reference-plat...
	wget https://builds.96boards.org/snapshots/reference-pl... -O grubaa64.efi
	mkdir boot-fat
	dd if=/dev/zero of=boot-fat.uefi.img bs=512 count=131072
	sudo mkfs.fat -n "BOOT IMG" boot-fat.uefi.img
	sudo mount -o loop,rw,sync boot-fat.uefi.img boot-fat
	sudo cp hi6220-hikey.dtb boot-fat/hi6220-hikey.dtb
	sudo cp AndroidFastbootApp.efi boot-fat/fastboot.efi
	sudo cp grubaa64.efi boot-fat/grubaa64.efi
	sudo umount boot-fat
	sudo mv boot-fat.uefi.img hikey-boot-linux-VERSION.uefi.img
	rm -rf boot-fat


##出错 fatal error: uuid/uuid.h: No such file or directory


	解决
	
	 sudo apt-get install uuid-dev