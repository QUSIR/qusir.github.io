---
title: 同步webrtc_android源码
date: 2017-03-12 23:22:53
tags:
---

##下载同步工具

	git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
##配置环境变量

	$ export PATH=`pwd`/depot_tools:"$PATH"

##开始同步

	fetch --nohooks webrtc_android
	gclient sync