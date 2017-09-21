---
title: 交叉编译pjsip
date: 2017-05-12 17:06:20
tags:
---


##说明
&emsp;PJSIP是一种以C语言编写的免费开源多媒体通信库，实现基于标准的协议，如SIP，SDP，RTP，STUN，TURN和ICE。它将信令协议（SIP）与丰富的多媒体框架和NAT穿越功能相结合，成为可移植的高级API，适用于从台式机，嵌入式系统到手机等几乎任何类型的系统。

官方网址为[http://www.pjsip.org/](http://www.pjsip.org/)

##修改编译配置文件
配置文件路径为

pjproject-2.4.5\pjlib\include\pj\config_site.h

添加以下宏定义
	
	#define PJMEDIA_AUDIO_DEV_HAS_ALSA		1
	#undef PJMEDIA_AUDIO_DEV_HAS_PORTAUDIO
	#define PJMEDIA_AUDIO_DEV_HAS_PORTAUDIO		0
	#include <pj/config_site_sample.h>
	
	#define PJMEDIA_RESAMPLE_NONE   1
	#define PJMEDIA_HAS_SPEEX_AEC   0
	#define PJMEDIA_HAS_VIDEO   0
	#define PJMEDIA_CONF_USE_SWITCH_BOARD 1



##编译安装
用的是  pjproject-2.4.5版本

直接在源码目录下执行以下指令

	./configure --prefix=/usr/local/arm_linux_4.2 --host=arm-linux  LIBS=-ldl
	
	make dep
	
	make

##运行例子程序
![view](http://ohjvpki1b.bkt.clouddn.com/build_pjsip.png)

##音频驱动切换

&emsp;Linux音频驱动分为alsa和oss，oss是比较旧的驱动，pjsip支持这两种音频驱动，默认是oss，蓝天门口机的内核使用的是alsa驱动，运行例子程序的时候会出现以下问题。

![view](http://ohjvpki1b.bkt.clouddn.com/pjsip_device.png)
###解决方法
修改config_site.h文件  在pjproject-2.4.5\pjlib\include\pj目录下

增加
	#define PJMEDIA_AUDIO_DEV_HAS_ALSA 1
