---
title: orangepi zero gpio控制
date: 2017-09-20 17:06:20
tags:
---


#orangepi zero接口图
![](http://ohjvpki1b.bkt.clouddn.com/opiz-pins-26-0.jpg)

![](http://ohjvpki1b.bkt.clouddn.com/orangepi_pin.png)

#编译安装WiringOP
源码
[WiringOP-zero.zip](http://ohjvpki1b.bkt.clouddn.com/WiringOP-zero.zip)

解压
	
	unzip WingOP-zero.zip

编译安装

	cd WingOP-zero
	./build

#测试是否安装成功

	gpio -v
	gpio readall

#例子程序，控制gpio0闪烁
	vim test_gpio.c

##demo code

	#include <wiringPi.h>
	int main(void)
	{
	  wiringPiSetup() ;
	  pinMode (0, OUTPUT) ;
	  for(;;) 
	  {
	    digitalWrite(0, HIGH) ; delay (500) ;
	    digitalWrite(0,  LOW) ; delay (500) ;
	  }
	}

##编译demo
	
	 gcc -Wall -o test_gpio test_gpio.c -lwiringPi -lpthread

##执行程序

	sudo ./test_gpio