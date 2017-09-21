---
title: 各种Android手机Root方法
date: 2016-12-19 21:21:06
tags:
---
##Root的介绍


&emsp;谷歌的android系统管理员用户就叫做root，该帐户拥有整个系统至高无上的权利，它可以访问和修改你手机几乎所有的文件，只有root才具备最高级别的管理权限。我们root手机的过程也就是获得手机最高使用权限的过程。同时为了防止不良软件也取得root用户的权限，当我们在root的过程中，还会给系统装一个程序，用来作为运行提示，由用户来决定，是否给予最高权限。这个程序的名字叫做Superuser.apk。当某些程序执行su指令想取得系统最高权限的时候，Superuser就会自动启动，拦截该动作并作出询问，当用户认为该程序可以安全使用的时候，那么我们就选择允许，否则，可以禁止该程序继续取得最高权限。Root的过程其实就是把su文件放到/system/bin/ `Superuser.apk` 放到system/app下面，还需要设置/system/bin/su可以让任意用户可运行，有set uid和set gid的权限。即要在android机器上运行命令：`adb shell chmod 4755 /system/bin/su`。而通常，厂商是不会允许我们随便这么去做的，我们就需要利用操作系统的各种漏洞，来完成这个过程。

特别说明：我们烧机中的Eng版本并没有Root权限

##说明
&emsp;想手机root必须要在android系统内加入su程序，再安装对su管理的apk程序。

##方法
&emsp;在手机刷了第三方的`recovery`前提下，通过刷zip的方式刷入`su`和`apk`程序。

##步骤

###去官网下载SuperSU压缩包
网址[http://www.supersu.com/](http://www.supersu.com/)

可能需要翻墙
进入该网站后进入download

下载压缩包
![](http://ohjvpki1b.bkt.clouddn.com/qusirSuperSU.png)

进入recovery，刷入zip文件

![](http://ohjvpki1b.bkt.clouddn.com/qusirrecovery_install_zip.jpg)

