---
title: linux终端实现代理
date: 2016-12-19 21:31:41
tags:
---

##ubuntu 14.04安装Shadowsocks-Qt5
	
	sudo add-apt-repository ppa:hzwhuang/ss-qt5
	sudo apt-get update
	sudo apt-get install shadowsocks-qt5


##运行Shadowsocks-Qt5

![Shadowsocks-Qt5config](http://ohjvpki1b.bkt.clouddn.com/Shadowsocks-Qt5config.png)

配置好账号和代理端口，代理端口为1080 socks5


##安装Privoxy

	sudo apt-get install privoxy

##设置配置文件
找到
4.1. listen-address这一节，确认监听的端口号。

	listen-address  localhost:8118


找到5.2. forward-socks4, forward-socks4a, forward-socks5 and forward-socks5t

	forward-socks5   /               127.0.0.1:1080 .

[我修改过后的配置文件](http://ohjvpki1b.bkt.clouddn.com/config)

##重启Privoxy

	sudo /etc/init.d/privoxy restart

##配置环境变量
	sudo vim /etc/profile
在文件末尾添加以下代码

	export http_proxy="127.0.0.1:8118"
	export https_proxy="127.0.0.1:8118"

使环境变量生效

	source /etc/profile

##设置privoxy开机启动
编辑启动项文件

	sudo vim /etc/rc.local

在exit0之前添加如下语句

	sudo /etc/init.d/privoxy start


	