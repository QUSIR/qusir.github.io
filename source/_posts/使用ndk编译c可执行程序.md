---
title: 使用ndk编译c可执行程序
date: 2016-12-19 21:30:20
tags:
---
##1.创建工程目录

&emsp;在ubuntu系统下搭建好ndk编译环境,创建test目录

	mkdir test
在test目录下创建jni目录
	cd test
	mkdir jni

##2.编写源代码

vim hello-exe.c

	#include<stdio.h>
	
	int main(){
	 printf("hello\n");
	 return 0;
	}

##3.创建android makefile文件

创建Android.mk和Application.mk文件，在jni目录下

Android.mk

	LOCAL_PATH := $(call my-dir)

	include $(CLEAR_VARS)
	
	LOCAL_CFLAGS += -fPIE  
	LOCAL_LDFLAGS += -fPIE -pie
	

	LOCAL_MODULE	:=hello-exe
	
	LOCAL_SRC_FILES	:=hello-exe.c
	
	include $(BUILD_EXECUTABLE)  

Application.mk

	APP_ABI := all

##说明：
&emsp;如果将程序拷到sd卡内会出现无法修改程序的可执行权限，可以拷贝到手机内部存储再修改。

如果程序执行的时候提示PIE出错则要在android.mk里面添加以下两段字段

	LOCAL_CFLAGS += -fPIE  
	LOCAL_LDFLAGS += -fPIE -pie

[源程序](http://ohjvpki1b.bkt.clouddn.com/ndk%E7%A8%8B%E5%BA%8F%E5%BC%80%E5%8F%91.7z)