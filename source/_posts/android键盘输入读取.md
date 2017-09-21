---
title: android键盘输入读取
date: 2017-01-21 17:07:41
tags:
---

##android键盘输入读取
&emsp;监控android键盘输入方式有两种，一种在java层实现，重写`onKeyDown`和`onKeyUp`方法。另一种是在jni层实现，监控`/dev/input/event0`键盘输入设备，获取输入数据。第一种方法缺陷是当有多个键盘设备时候无法区分是哪个键盘输入的，第二种方法是需要有该文件的读写权限。

##调试方法
进入adb模式

	adb shell
输入以下指令监控键盘输入

	getevent
![view](http://ohjvpki1b.bkt.clouddn.com/getevent.png)

##方法1实现
代码实现

	 @Override
    public boolean onKeyDown(int keyCode,KeyEvent event){
        switch(keyCode){
            case KeyEvent.KEYCODE_0:
                testview.setText("按下按键0");
                break;
            case KeyEvent.KEYCODE_1:
                testview.setText("按下按键1");
                break;
            case KeyEvent.KEYCODE_2:
                testview.setText("按下按键2");
                break;
            case KeyEvent.KEYCODE_3:
                testview.setText("按下按键3");
                break;
            case KeyEvent.KEYCODE_4:
                testview.setText("按下按键4");
                break;
            case KeyEvent.KEYCODE_5:
                testview.setText("按下按键5");
                break;
            case KeyEvent.KEYCODE_6:
                testview.setText("按下按键6");
                break;
            case KeyEvent.KEYCODE_7:
                testview.setText("按下按键7");
                break;
            case KeyEvent.KEYCODE_8:
                testview.setText("按下按键8");
                break;
            case KeyEvent.KEYCODE_9:
                testview.setText("按下按键9");
                break;
            case KeyEvent.KEYCODE_DEL:
                testview.setText("按下按键*");
                break;
            case KeyEvent.KEYCODE_ENTER:
                testview.setText("按下按键#");
                break;
            default:
                testview.setText("无法识别输入");
                break;

        }

        return super.onKeyDown(keyCode, event);
    }
    /*释放按键事件*/
    @Override
    public boolean onKeyUp(int keyCode,KeyEvent event){
        switch(keyCode){
            case KeyEvent.KEYCODE_0:
                testview.setText("放开按键0");
                break;
            case KeyEvent.KEYCODE_1:
                testview.setText("放开按键1");
                break;
            case KeyEvent.KEYCODE_2:
                testview.setText("放开按键2");
                break;
            case KeyEvent.KEYCODE_3:
                testview.setText("放开按键3");
                break;
            case KeyEvent.KEYCODE_4:
                testview.setText("放开按键4");
                break;
            case KeyEvent.KEYCODE_5:
                testview.setText("放开按键5");
                break;
            case KeyEvent.KEYCODE_6:
                testview.setText("放开按键6");
                break;
            case KeyEvent.KEYCODE_7:
                testview.setText("放开按键7");
                break;
            case KeyEvent.KEYCODE_8:
                testview.setText("放开按键8");
                break;
            case KeyEvent.KEYCODE_9:
                testview.setText("放开按键9");
                break;
            case KeyEvent.KEYCODE_DEL:
                testview.setText("放开按键*");
                break;
            case KeyEvent.KEYCODE_ENTER:

                testview.setText("放开按键#");
                break;
            default:
                testview.setText("无法识别输入");
                break;
        }

        return super.onKeyUp(keyCode, event);
    }
##方法2实现
赋予可读写权限
	
	C:\Users\liang>adb shell
	root@octopus-f1:/ # chmod 777 /dev/input/event6

代码实现

	#include <stdio.h>
	#include <linux/input.h>
	#include <stdlib.h>
	#include <sys/types.h>
	#include <sys/stat.h>
	#include <fcntl.h>
	#include"Logger.h"
	#include<pthread.h>
	
	#include"com_example_liang_myapplication_KeyBoard.h"
	
	#define DEV_PATH "/dev/input/event6"   //difference is possible
	
	static void* pthread_read_keyboard(void*){
	    int keys_fd;
	    char ret[2];
	    struct input_event t;
	    keys_fd=open(DEV_PATH, O_RDONLY);
	    if(keys_fd <= 0)
	    {
	        LOGE("%s device error!\n",DEV_PATH);
	    }
	    for(;;){
	        usleep(500);
	        if(read(keys_fd, &t, sizeof(t)) == sizeof(t))
	        {
	            if(t.type==EV_KEY)
	                if(t.value==0 || t.value==1)
	                {
	                    LOGE("key %d %s\n", t.code, (t.value) ? "Pressed" : "Released");
	                    if(t.code == KEY_ESC)
	                        break;
	                }
	        }
	
	
	    }
	    LOGE("pthread_read_keyboard exit\n");
	    close(keys_fd);
	} 

自己写的
[DEMO](https://github.com/QUSIR/Android_KeyBoard.git)
