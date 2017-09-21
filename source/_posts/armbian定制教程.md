---
title: armbian定制教程
date: 2017-02-14 22:42:39
tags:
---


##时间201702141045
##官方网站

[https://www.armbian.com/](https://www.armbian.com/)

可以下载相应板子对应的镜像

![view](http://ohjvpki1b.bkt.clouddn.com/armbian_download.png)

###下载源码，定制系统生成相对应镜像

	mkdir armbian
	apt-get -y install git
	git clone https://github.com/igorpecovnik/lib --depth 1
	cp lib/compile.sh .
开启代理，不然会生成失败，需要下载相应文件，服务器在国外会比较慢，或是被墙住了

	生成指令
	./compile.sh KERNEL_CONFIGURE=yes


生成过程中选择相对应板子选项，然后再`output/image/`目录下生成相对应文件，然后将该文件烧入SD卡内。

##官方文档
[https://docs.armbian.com/](https://docs.armbian.com/)