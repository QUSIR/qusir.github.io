---
title: xposed各种机型安装方法
date: 2017-05-27 17:06:20
tags:
---


##说明
之前我一直以为xpose框架是针对特殊机型，现在才发现其实针对不同的android版本装xpose版本，跟机型没有关系。前提是是手机已root和安装了第三方recovery，比如TWRP。

##刷入recovery方法

参照我的另一篇博客

[Android手机刷recovery](http://www.cnblogs.com/QUSIR/p/6179999.html)

##刷入recovery后就可以很方便root

一种通用root方法，不针对机型。

[各种Android手机Root方法](http://www.cnblogs.com/QUSIR/p/6180236.html)

##以recovery方式刷入下xposed framework
[Xposed Framework下载地址](http://dl-xda.xposed.info/framework/)
其中，sdk21，sdk22，sdk23，分别对应Android 5.0，5.1， 6.0.
根据，手机ROM版本和处理器类型选择Xposed Framework刷机包。

比如，中兴Blade a1移动版(5.1, arm64)，我选择了刷机包xposed-v86-sdk22-arm64.zip 和卸载包xposed-uninstaller-20150831-arm64.zip
下载之后，将这两个压缩包，拷贝到SD卡根目录下。

###安装
1.重启手机,进入Recovery界面。(adb reboot recovery)
![view](http://ohjvpki1b.bkt.clouddn.com/xposed3.jpg)
2.选择【安装刷机包】进入下级页面，选择【从SD卡选择ZIP文件】

3.在SD卡根目录找到Xposed Framework刷机包(xposed-v86-sdk22-arm64.zip)，并选择。
![view](http://ohjvpki1b.bkt.clouddn.com/xposed4.jpg)
4.滑动底部的滑动条，确认刷入，等待提示刷机完成。

5.重启手机，等待进入桌面。
###卸载
1.如果刷入Xposed Framework刷机包之后，无限重启，进不去桌面怎么办？

2.那就按照下面提示，卸载掉Xposed Framework。

3.重启手机,进入Recovery界面。(adb reboot recovery)

4.选择【安装刷机包】进入下级页面，选择【从SD卡选择ZIP文件】

5.在SD卡根目录找到Xposed Framework卸载刷机包(xposed-uninstaller-20150831-arm64.zip)，并选择。
滑动底部的滑动条，确认刷入，等待提示刷机完成。

6.重启手机，等待进入桌面。
###Xposed Installer
这是一个管理Xposed模块的官方应用。通过它，你可以随时禁用或者启用Xposed模块，然后重启手机。
对于Android 5.0以上的手机，请前往XDA论坛主题贴下载附件 XposedInstaller_3.0_alpha4.apk，并安装。
下载地址：http://forum.xda-developers.com/showthread.php?t=3034811
如果你看到以下界面，恭喜你，Xposed Framework安装完成。
![view](http://ohjvpki1b.bkt.clouddn.com/xposed.png)

##参考

[http://www.snowdream.tech/2016/09/02/android-install-xposed-framework/](http://www.snowdream.tech/2016/09/02/android-install-xposed-framework/)