---
title: Android手机刷recovery
date: 2016-12-19 21:19:11
tags:
---

##以前觉得android刷机是件很麻烦的事，现在倒不觉得了。

&emsp;只要手机刷入第三方的recovery，一切都好办了，无论是`root`还是刷`google play`。

&emsp;recovery开源的有两大阵营，`twrp`和`cwm`。

[TWRP GitHub](https://github.com/TeamWin/Team-Win-Recovery-Project)

[TWRP下载](http://teamw.in/project/twrp2/)

查找适合你手机机型的recovery

[https://twrp.me/Devices/](https://twrp.me/Devices/)

![Search](http://ohjvpki1b.bkt.clouddn.com/twrp_recovery.png)

CyanogenMod源码中附带CWM源码,使用CyanogenMod可生成recovery镜像文件。

找到适合自己机型的recovery后，手机进入fastboot模式(手机都有这个模式，进入该模式方法不一样而已)，数据线链接电脑，装好驱动，在Android工具目录下执行以下指令。
	
	fastboot flash recovery recovery.img
成功后会显示，成功提示。

然后点电源键强制关机，后重启，进入recover模式。然后就可以刷任何`zip`文件了

##补充

有些手机可能还需要解锁

可以参考我另一篇文章

[http://www.cnblogs.com/QUSIR/p/6179506.html](http://www.cnblogs.com/QUSIR/p/6179506.html)

