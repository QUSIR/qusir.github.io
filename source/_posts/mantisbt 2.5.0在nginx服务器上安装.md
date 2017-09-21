---
title: mantisbt 2.5.0在nginx服务器上安装
date: 2017-06-15 17:06:20
tags:
---


##说明
由于安装2.5.0.版本失败，检测配置没有错，但是无法生成`config_inc.php`文件所以考虑安装2.0.0版本然后再覆盖安装。

##环境
ubuntu14.04  nginx  mysql

##安装mantisbt-2.0.0
	
	wget https://sourceforge.net/projects/mantisbt/files/mantis-stable/2.0.0/mantisbt-2.0.0.tar.gz/download -O mantisbt.tar.gz
	
	tar xzvf mantisbt.tar.gz
	
	cp mantisbt /var/www/
	
	http://你的网址/admin/install.php

![msm8909_gpio5.png](http://ohjvpki1b.bkt.clouddn.com/mantisbt-pre-installation-check.jpg.jpg)

##安装过程视频教程

[How to Install Mantis Bug Tracker v2 with Nginx and PHP 7](http://ohjvpki1b.bkt.clouddn.com/How%20to%20Install%20Mantis%20Bug%20Tracker%20v2%20with%20Nginx%20and%20PHP%207.mp4)
##参考
[http://www.cnblogs.com/snooper/archive/2009/09/07/1561715.html](http://www.cnblogs.com/snooper/archive/2009/09/07/1561715.html)

[https://tjosm.com/6226/install-mantisbt-v2-ubuntu-php7-nginx-virtualmin/](https://tjosm.com/6226/install-mantisbt-v2-ubuntu-php7-nginx-virtualmin/)

##安装mantisbt-2.5.0

直接覆盖2.0.0版本

	tar xzvf mantisbt2.5.0.tar.gz
	cp -r mantisbt2.5.0 /var/www/mantisbt