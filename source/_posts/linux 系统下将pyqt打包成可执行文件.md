---
title: linux 系统下将pyqt打包成可执行文件
date: 2017-06-27 17:06:20
tags:
---

##1.解压源码包，前提安装了setuptools

[官网http://www.pyinstaller.org/](http://www.pyinstaller.org/)

	unzip pyinstaller-python3.zip
##2.安装
	cd pyinstaller-python3
	python setup.py build
	python setup.py install
安装完之后在/usr/local/bin 生成pyinstaller可执行文件

##3.编译.py文件生成可执行文件
	pyinstaller test.py
在目录下生成 build和dist文件夹   可执行文件在dist目录下

![clipboard](http://ohjvpki1b.bkt.clouddn.com/clipboard.png)
