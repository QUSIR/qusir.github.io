---
title: 加速android源码编译
date: 2016-12-19 21:28:44
tags:
---
## 添加缓存环境变量 :
在 ~/.bashrc 环境变量文件中 添加 ` export USE_CCACHE=1 `环境变量,  加速随后的编译过程; 
##分配缓存磁盘大小
为 ccache 指定磁盘中的一部分大小, 用于缓存, 使用` prebuilts/misc/linux-x86/ccache/ccache -M 50G` 命令,大小可以自己设定，20G也可以。

##命令执行位置
在 Android 源码根目录执行 `prebuilts/misc/linux-x86/ccache/ccache -M 50G` 命令;