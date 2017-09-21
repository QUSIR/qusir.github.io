---
title: ndk编译pjsip
date: 2017-05-12 17:06:20
tags:
---

#环境
pjsip版本`pjproject-2.5.5.tar.bz2`

ndk版本`android-ndk-r10e`

##配置ndk环境变量

	export ANDROID_NDK_ROOT=/home/gsta/liang/android-ndk-r10e

##修改配置文件

vim ./pjlib/include/pj/config_site.h 

	#define PJ_CONFIG_ANDROID 1
	#include <pj/config_site_sample.h>
##配置编译环境

	./configure-android 

##编译
	make dep && make clean && make
##注意：整个编译过程是在linux系统下进行的