---
title: orangepi开启yuv摄像头驱动
date: 2017-08-30 17:06:20
tags:
---

##说明
&ensp;使用orangepi外接USB摄像头进行拍照，发现没有`/dev/video0`，可能没哟将yuv驱动编译进内核，需要外部加载进去。
##添加驱动

&emsp;一开始没有发现`/lib/modules/3.4.39/`目录下`uvcvideo.ko`，害得自己找来orangepi内核重新编译了一遍。

1.插入驱动模块

	sudo insmod /lib/modules/3.4.39/uvcvideo.ko

2.查看是否识别了驱动

	ls /dev/video0

3.添加系统启动时候加载驱动

	sudo vim /etc/rc.local 

添加以下语句
	
	sudo insmod /lib/modules/3.4.39/uvcvideo.ko