---
title: Ubuntu16.04安装Stacer工具
date: 2017-04-08 22:28:11
tags:
---

##说明
Stacer工具可以显示系统资源使用情况，清除系统垃圾，卸载软件，关闭开启自启动服务。

##安装
###deb 包安装

Debian Linux x86（Ubuntu）
1. Download `Stacer_1.0.4_i386.deb` from the [Stacer releases page](https://github.com/oguzhaninan/Stacer/releases).
2. Run `sudo dpkg --install Stacer_1.0.4_i386.deb` on the downloaded package.
3. Launch Stacer using the installed `Stacer` command.

Debian Linux x64 (Ubuntu)

1. Download `Stacer_1.0.4_amd64.deb` from the [Stacer releases page](https://github.com/oguzhaninan/Stacer/releases).
2. Run `sudo dpkg --install Stacer_1.0.4_amd64.deb` on the downloaded package.
3. Launch Stacer using the installed `Stacer` command.

###源码安装
	git clone https://github.com/oguzhaninan/Stacer.git
	cd Stacer
	npm install && npm start
##卸载

运行`sudo dpkg -r Stacer`

#使用
##仪表板

<img src="http://ohjvpki1b.bkt.clouddn.com/Stacer.png" width = "512" height = "214" alt="Stacer" align=center />
##系统清洁

<img src="http://ohjvpki1b.bkt.clouddn.com/System_Cleaner.png" width = "407" height = "346" alt="System_Cleaner" align=center />
##启动应用程序
<img src="http://ohjvpki1b.bkt.clouddn.com/Startup_Apps.png" width = "407" height = "346" alt="Startup_Apps" align=center />
##服务
<img src="http://ohjvpki1b.bkt.clouddn.com/Services.png" width = "407" height = "346" alt="Services" align=center />
##卸载程序

<img src="http://ohjvpki1b.bkt.clouddn.com/Uninstaller.png" width = "407" height = "346" alt="Uninstaller" align=center />