---
title: hikey android源码编译
date: 2017-01-21 17:17:41
tags:
---

##同步源码
	repo init -u https://android.googlesource.com/platform/manifest -b master
	
	repo sync -j24

##下载相应驱动

	wget https://dl.google.com/dl/android/aosp/linaro-hikey-20160226-67c37b1a.tgz
	
	tar xzf linaro-hikey-20160226-67c37b1a.tgz
	
    ./extract-linaro-hikey.sh

##编译源码
	 . ./build/envsetup.sh
	
	 lunch hikey-userdebug
	
	 make -j32