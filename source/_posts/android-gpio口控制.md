---
title: android gpio口控制
date: 2017-01-21 17:06:20
tags:
---

##android gpio口控制

&emsp;GPIO口控制方式是在jni层控制的方式实现高低电平输出,类似linux的控制句柄方式，在linux系统下将每个设备看作一个文件，android系统是基于linux内核的。

##保证该文件有读写权限

![view](http://ohjvpki1b.bkt.clouddn.com/android_gpio.png)


##用命令控制gpio输出
	输出高电平
	echo 1 > /system/class/gpio_sw/data
	输出低电平
	echo 1 > /system/class/gpio_sw/data
##代码段

	#include <unistd.h>
	#include"Logger.h"
	#include <stdio.h>
	#include <linux/input.h>
	#include <stdlib.h>
	#include <sys/types.h>
	#include <sys/stat.h>
	#include <fcntl.h>
	
	#define DEV_PATH "/sys/class/gpio_sw/PE12/data"   //difference is possible
	
	
	JNIEXPORT jint JNICALL Java_com_example_liang_gpio_1demo_Gpio_Set_1GPIO
	        (JNIEnv *env,jobject){
	    int fd;
	
	    fd = open(DEV_PATH, O_WRONLY);
	    if(fd < 0){
	        LOGE("fail in open file %s", DEV_PATH);
	        return -1;
	    }
	    write(fd, "1", strlen("1"));  //输出高电平
	    sleep(1);  //延时
	    write(fd, "0", strlen("0"));  //输出低电平
	    close(fd);
	    return 0;
	
	}


##使用
&emsp;点击SetGpio按钮输出高低电平变化
![](http://ohjvpki1b.bkt.clouddn.com/gpio_view.jpg)
自己编写
[DEMO](https://github.com/QUSIR/Android_GPIO_DEMO)