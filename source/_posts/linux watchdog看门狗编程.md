---
title: linux watchdog看门狗编程
date: 2017-08-30 17:06:20
tags:
---


##说明
由于防止linux系统下程序突然意外终止或是陷入死循环等情况，启用看门狗机制，出现问题的时候机器重启。

##初始化看门狗

查看liux系统下是否有 `/dev/watchdog`控制句柄

	ls /dev/watchdog

代码段

	int fd = open("/dev/watchdog", O_WRONLY);
	if(fd == -1){
	    printf("open watchdog error \n\n\n");
	    return false;
	}
	int timeout;
	timeout = 15;
	ioctl(fd, WDIOC_SETTIMEOUT, &timeout); //设置超时
	printf("The timeout was set to %d seconds\n", timeout);

##喂狗
	ioctl(this->fd, WDIOC_KEEPALIVE);

使用定时的方式运行以上语句，延时时间必须小于超时时间。