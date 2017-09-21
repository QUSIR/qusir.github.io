---
title: linux git实现代理
date: 2016-12-19 21:30:53
tags:
---

##说明
&emsp;Git 目前支持的三种协议 `git://`、`ssh://` 和 `http://`，使用`git://` 和 `http://`比较多,`ssh://`忽略，翻墙后可以直接加快同步google和github代码。

方式是通过Shadowsocks实现翻墙再使用[connect](https://github.com/QUSIR/connect)工具实现代理转换。 

##安装配置shadowsock-qt5
###安装
	
	sudo add-apt-repository ppa:hzwhuang/ss-qt5
	sudo apt-get update
	sudo apt-get install shadowsocks-qt5

###配置

![](http://ohjvpki1b.bkt.clouddn.com/Shadowsocks-Qt5config.png)

使用socks5的1080端口

##安装connect-proxy

在ubuntu 14.04 64位系统下

	sudo apt-get install connect-proxy

##`git://`协议代理

###创建socks5proxywrapper文件添加如下语句

	#!/bin/sh
	connect -S 127.0.0.1:1080 "$@"

注意：是1080端口

###赋予可执行权限
	
	chmod +x socks5proxywrapper

###配置git
打开git配置文件

	vim .gitconfig

添加以下语句

	[core]
        gitproxy = /path/to/socks5proxywrapper

说明：也就是创建socks5proxywrapper文件存放目录。

##`https://`代理

###配置git执行以下语句

	git config --global http.proxy 'socks5://127.0.0.1:1080'
	git config --global https.proxy 'socks5://127.0.0.1:1080'

###查看配置查看里面是否有相关选项
	
	cat ~.gitconfig

[参考教程](https://segmentfault.com/q/1010000000118837)