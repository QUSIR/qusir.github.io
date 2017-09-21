---
title: ubuntu16.04搭建xrdp远程桌面链接
date: 2017-03-12 23:24:34
tags:
---


#说明

xrdp支持xfce4和mate桌面,不支持gnome和ubuntu桌面

由于之前安装xfce4桌面后屏幕上显示一条黑线，所以选择放弃使用xfce4桌面使用mate桌面。
##安装mate桌面

	sudo apt-get install mate-core mate-desktop-environment mate-notification-daemon

#安装tightvncserver
	sudo apt-get install tightvncserver
#安装xrdp
	sudo apt-get install xrdp

#配置xrdp
	echo mate-session >~/.xsession
	vim /etc/xrdp/startwm.sh
	#在./etc/X11/Xsession前插入
	mate-session
	#重启xrdp
	cd /etc/init.d/
#卸载xrdp和tightvncserver

	sudo apt-get purge xrdp

	sudo apt-get purge tightvncserver


#要注意地方
	一定要先装tightvncserver后装xrdp,不能够装vnc4server，已改为tightvncserver.
#完成的效果图

![view](http://ohjvpki1b.bkt.clouddn.com/mate_view.png)