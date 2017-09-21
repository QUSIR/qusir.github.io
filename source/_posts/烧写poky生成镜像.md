---
title: 烧写poky生成镜像
date: 2017-01-21 17:11:46
tags:
---

sudo dd if= rpi-hwup-image-raspberrypi0-20170117030238.rootfsrpi-sdimg of=/dev/sdc bs=4M oflag=sync status=noxfer

说明
rpi-hwup-image-raspberrypi0-20170117030238.rootfsrpi-sdimg为生成镜像

/dev/sdc
为SD卡路径

烧写完成之后
要用串口看调试，因为很多东西没有安装配置，无法输出到显示，默认账号root  没有密码