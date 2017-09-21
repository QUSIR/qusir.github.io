---
title: poky raspbian教程
date: 2017-01-21 17:09:05
tags:
---

快速链接
===========
Git仓库web前端：
    http://git.yoctoproject.org/cgit/cgit.cgi/meta-raspberrypi/
邮件列表（yocto邮件列表）：
    yocto@yoctoproject.org
问题管理（Github问题）：
    https://github.com/agherzan/meta-raspberrypi/issues


内容：
=========
1.说明
Yocto BSP层 - 树莓派
    2.A. 如何使用它
    2.B. 图片
3.可选构建配置
    3.A. 压缩的部署文件
    3.B. GPU内存
    3.C. 添加购买的许可编解码器
    3.D. 禁用过扫描
    3.E. 设置超频选项
    3.F. 视频摄像机支持V4L2驱动程序
    3.G. 启用离线合成支持
    3.H. 在控制台支持上启用kgdb
    3.I. 引导到U-Boot
    3.J. 图像与Initramfs
    3.K. 设备树支持
    3.L. 使能SPI总线
    3.M. 使能I2C
    3.N. 启用PiTFT支持
    3.O. 启用UART支持
4.额外的应用程序
    4.A. omxplayer
5.板卡配置
    5.A. 音频路由
源代码和镜像
7.贡献
    7.A. 邮件列表
    7.B. Github问题
8.维护人员


1.说明
==============

这是RaspberryPi设备的通用硬件特定BSP覆盖。

更多信息，请访问：
                   http://www.raspberrypi.org/（官方网站）

meta-raspberrypi的核心BSP部分应该与不同的工作
OpenEmbedded / Yocto分布和层堆栈，例如：
* Distro-less（仅适用于OE-Core）。
* Angstrom。
* Yocto / Poky（测试的主要焦点）。

Yocto BSP层 - 树莓派
================================

此图层取决于：

URI：git：//git.yoctoproject.org/poky
branch：master
修订：HEAD

URI：git：//git.openembedded.org/meta-openembedded
层：元 - 元，元多媒体
branch：master
修订：HEAD

2.A. 如何使用它
==================
一个。source poky / oe-init-build-env rpi-build
b。将所需的图层添加到bblayers.conf：
    -  meta-raspberrypi
C。将local.conf中的MACHINE设置为所支持的主板之一：
    -  raspberrypi
    -  raspberrypi0
    -  raspberrypi2
    -  raspberrypi3
d。bitbake rpi-hwup-image
e。dd到SD卡生成的sdimg文件（如果使用rpi-sdimg.xz，请使用xzcat）
F。启动RPI。

2.B. 图片
-  ===========
* rpi-hwup-image
    硬件图像
* rpi-basic-image
    基于rpi-hwup-image和一些添加的功能（例如：splash）
* rpi-test-image
    基于rpi-basic-image的图像，其中包含了大部分的包
    层和一些媒体样本。

3.可选构建配置
=============================

存在一组用户可以影响构建的不同参数的方式。
我们在这里列出与这个BSP密切相关或特定于它的那些。对于其余的
请检查：http：//www.yoctoproject.org/docs/latest/ref-manual/ref-manual.html

3.A. 压缩的部署文件
==============================
1.在local.conf中覆盖IMAGE_FSTYPES
    IMAGE_FSTYPES =“tar.bz2 ext3.xz”
2.在local.conf中覆盖SDIMG_ROOTFS_TYPE
    SDIMG_ROOTFS_TYPE =“ext3.xz”
3.在local.conf中覆盖SDIMG_COMPRESSION
    SDIMG_COMPRESSION =“xz”
*容纳以上的值到您自己的需要（例如：ext3 / ext4）。

3.B. GPU内存
===============
变量：详细信息
GPU_MEM：GPU内存（兆字节）。设置ARM和之间的内存分割
              GPU。ARM获取剩余的内存。最小16.默认64。
GPU_MEM_256：256MB Raspberry Pi的GPU内存（兆字节）。忽略了
              512MB RP。覆盖gpu_mem。Max 192.默认值未设置。
GPU_MEM_512：512MB Raspberry Pi的GPU内存（兆字节）。忽略了
              256MB RP。覆盖gpu_mem。默认值未设置。
GPU_MEM_1024：1024MB Raspberry Pi的GPU内存（兆字节）。忽略了
              256MB / 512MB RP。覆盖gpu_mem。最大944.默认值未设置。

3.C. 添加购买的许可证编解码器
===============================
要添加您自己的许可，请使用变量KEY_DECODE_MPG2和KEY_DECODE_WVC1
local.conf。例：
KEY_DECODE_MPG2 =“12345678”
KEY_DECODE_WVC1 =“12345678”
您可以提供更多许可证（以逗号分隔）。例：
KEY_DECODE_WVC1 =“0x12345678,0xabcdabcd，0x87654321”

3.D. 禁用过扫描
=====================
默认情况下，GPU在视频输出周围添加一个黑色边框以补偿
切断图像的一部分的电视。要禁用此设置此变量
local.conf：
DISABLE_OVERSCAN =“1”

3.E. 设置超频选项
===========================
Raspberry PI可以超频。到现在超频到“涡轮
模式“由raspbery正式支持，不会失去保修。
检查config.txt有关选项和模式的详细描述。例：
＃Turbo模式
ARM_FREQ =“1000”
CORE_FREQ =“500”
SDRAM_FREQ =“500”
OVER_VOLTAGE =“6”

3.F. 视频摄像机支持V4L2驱动程序
=======================================
设置此变量以启用对摄像机的支持（需要Linux 3.12.4+）
VIDEO_CAMERA =“1”

3.G. 启用离线合成支持
=====================================
设置此变量以启用对dispmanx脱机合成的支持
DISPMANX_OFFLINE =“1”

这将使固件回退到离线合成
Dispmanx元素。通常，在扫描期间，在线完成合成，
但不能处理太多的元素。当离线启用时，屏幕外
缓冲区被分配用于合成。当场景复杂度（数量和大小
的元素）高，合成将发生离线进入缓冲区。

大力推荐Wayland / Weston。

参见：http：//wayland.freedesktop.org/raspberrypi.html

3.H. 在控制台支持上启用kgdb
===================================
要将kdbg控制台（kgdboc）参数添加到内核命令行，
在local.conf中设置此变量：
ENABLE_KGDB =“1”

3.I. 引导到U-Boot
===================
要使u-boot加载内核映像，请在local.conf中设置
KERNEL_IMAGETYPE =“uImage”

这将使kernel.img是u-boot映像，这将加载uImage。
默认情况下，kernel.img是实际的内核映像（例如Image）。

3.J. 图像与Initramfs
=======================
要构建initramfs映像：
    *设置这3个内核变量（例如在linux-raspberrypi.inc中）
        -  kernel_configure_variable BLK_DEV_INITRD y
        -  kernel_configure_variable INITRAMFS_SOURCE“”
        -  kernel_configure_variable RD_GZIP y
    *设置yocto变量（例如在linux-raspberrypi.inc中）
        - INITRAMFS_IMAGE =“ ”
        -  INITRAMFS_IMAGE_BUNDLE =“1”
    *设置meta-rasberrypi变量（例如，在raspberrypi.conf中）
        -  KERNEL_INITRAMFS =“-initramfs”

3.K. 设备树支持
=======================
仅当使用linux-raspberrypi 3.18+时，才支持RPi的设备树
内核。

    *设置KERNEL_DEVICETREE（在conf / machine / raspberrypi.conf中）
        - 在内核安装任务之前将预告片添加到内核映像。
          在创建SDCard映像时，将修改此内核
          引导分区（作为kernel.img）以及DeviceTree blob（.dtb文件）。

注意：对于内核> = 3.18，始终禁用KERNEL_DEVICETREE
      较老的内核版本。

3.L. 使能SPI总线
====================
当使用设备树内核时，设置此变量以启用SPI总线
ENABLE_SPI_BUS =“1”

3.M. 使能I2C
===============
当使用设备树内核时，设置此变量以启用I2C
ENABLE_I2C =“1”

3.N. 启用PiTFT支持
=======================
使用PiTFT屏幕的基本支持可以通过添加启用
下面在local.conf中：

MACHINE_FEATURES + =“pitft”
  - 这将启用SPI总线和i2c设备树，它也将设置
    控制台的framebuffer和PiTFT上的x服务器。

注意：为了使这个工作，PiTFT模型的叠加必须构建，
      添加和指定（dtoverlay = 在config.txt）

以下是在meta-raspberrypi中当前支持的PiTFT模型的列表，
模型名应该作为MACHINE_FEATURES在local.conf中添加，如下所示：
    -  MACHINE_FEATURES + =“pitft “。

当前支持的型号列表：
    -  pitft22
    -  pitft28r

3.O. 启用UART
===============

默认情况下，RaspberryPi 1，2和CM将启用UART控制台。

RaspberryPi 3没有默认启用UART，因为这需要一个
固定核心频率和enable_uart将其设置为最小。某些
操作 -  60fps h264解码，高质量去隔行 - 这不是
在ARM上执行可能会受到影响，我们不想这样对用户
谁不想使用串口。需要串口控制台支持的用户
RaspberryPi3必须在local.conf中明确设置：ENABLE_UART =“1”。

参考：https：//github.com/raspberrypi/firmware/issues/553
      https://github.com/RPi-Distro/repo/issues/22

4.额外的应用程序
=============

4.A. omxplayer
==============
omxplayer取决于具有商业许可证的libav。所以为了成为
能够编译omxplayer你将需要whiteflag商业许可证
添加到local.conf：
LICENSE_FLAGS_WHITELIST =“commercial”

5.板卡配置
======================

5.A. 音频路由
==================
加载音频驱动程序

    modprobe snd-bcm2835

测试音频播放

    例如aplay test.wav

请注意，如果没有连接HDMI，则会从3.5英寸插孔连接器发出音频
如预期。但是，如果连接了HDMI显示器，则没有音频输出
插孔连接器。

要通过3.5in插孔连接器强制音频路由

    amixer cset numid = 3 1

amixer cset的选项有：

    0 =自动
    1 =耳机
    2 = hdmi

源代码和镜像
==========================

主要仓库：
    git：//git.yoctoproject.org/meta-raspberrypi
    http://git.yoctoproject.org/git/meta-raspberrypi

Github镜像：
    https://github.com/agherzan/meta-raspberrypi

Bitbucket镜子：
    https://bitbucket.org/agherzan/meta-raspberrypi


贡献
===============

7.A. 邮件列表
=================
我们使用的主要通信工具是邮件列表：
    yocto@yoctoproject.org
    https://lists.yoctoproject.org/listinfo/yocto

随时提出任何问题，但总是在你的电子邮件主题
与“[meta-raspberrypi]”。这是因为我们使用“yocto”邮件列表和
不是一个perticular'meta-raspberrypi'邮件列表。

要贡献这个层，你应该发送补丁以供审查
以上指定的邮件列表。
补丁应该符合开放补丁指南：
http://www.openembedded.org/wiki/Commit_Patch_Message_Guidelines


创建修补程序时，请使用类似：

    git format-patch -s --subject-prefix ='meta-raspberrypi] [PATCH'origin

当发送补丁到邮件列表时，请使用类似：

    git send-email --to yocto@yoctoproject.org 

7.B. Github的问题
==================
为了管理和跟踪meta-raspberrypi问题，我们使用github问题：
    https://github.com/agherzan/meta-raspberrypi/issues

如果你推补丁有一个github问题相关联，请提供
发布号在提交日志就在“签署者”行之前。示例行
为一个错误：
    [问题＃13]


8.维护人员
==============

    安德烈Gherzan 