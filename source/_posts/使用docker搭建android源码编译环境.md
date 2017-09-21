---
title: 使用docker搭建android源码编译环境
date: 2017-06-07 17:06:20
tags:
---


##说明
&emsp;由于工作原因要对android源码进行编译，用于修改底层驱动，对系统进行定制。但是编译过程中要使用特定ubuntu版本和gcc版本，所以会比较麻烦。当时第一反映是安装VMware虚拟机，装一个指定版本然后将源码拷贝进去再编译，该方法是可行的。后来才发现很麻烦，虚拟机很占用空间，于是决定使用docker试试，实验了一番，还真可以，方便占用存储小，启动起来快。

##环境
主机：Ubuntu 16.04.2 LTS  64位  16G内存 500G硬盘  16个内核

docker版本: 17.03.0-ce

docke镜像：ubuntu12.04 64位
##同步镜像

	sudo docker pull registry.cn-shenzhen.aliyuncs.com/qusir/ubuntu12.04_msm8909:0.1

以上镜像我已经定制好了的版本ubuntu12.04 64位，安装好了依赖项和gcc，可以直接使用msm8909高通平台编译。

##修改后保存镜像

###docker提交镜像
先查看运行镜像的ID，不要将镜像停止运行，然后用以下命令

	速冻docker commit 61412230ae46 registry.cn-hangzhou.aliyuncs.com/qusir/liang

###添加镜像
	sudo docker tag 70838701e83a registry.cn-hangzhou.aliyuncs.com/qusir/liang:0.1

###推送镜像
	sudo docker push registry.cn-hangzhou.aliyuncs.com/qusir/liang:0.1
##编译源码
###启动镜像，映射目录

	docker run -it --rm -v /home/liang/data:/data registry.cn-shenzhen.aliyuncs.com/qusir/ubuntu12.04_msm8909 bash

将主机上的`/home/liang/data`的目录映射到docke的`/data`目录下

###编译源码
初始化环境变量

	source ./build/envsetup.sh 
选择编译选项

	lunch

选择

	34. msm8909-userdebug
开始编译

	make -j8

###关闭镜像
	Ctrl+D退出镜像