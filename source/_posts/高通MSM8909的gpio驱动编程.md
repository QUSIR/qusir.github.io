---
title: 高通MSM8909的gpio驱动编程
date: 2017-06-09 17:06:20
tags:
---


#接收gpio信号输入

##1. 平台相关配置文件msm8909-mtp.dtsi文件中找到gpio_keys节，增加蓝字相关内容
![msm8909_gpio5.png](http://ohjvpki1b.bkt.clouddn.com/msm8909_gpio5.png)

##2. 引脚相关配置文件msm8909-pinctrl.dtsi文件中找到tlmm_gpio_key节，增加或更改红字相关内容
![msm8909_gpio6.png](http://ohjvpki1b.bkt.clouddn.com/msm8909_gpio6.png
)

##3. device\qcom\msm8909\gpio-keys.kl中增加键盘扫描码对应的键盘码
键盘码是Android系统收到底层驱动提交的扫描码后，向App发送的键盘码，比如本例

	key 77    F7  
扫描码对应给App的键盘码是F7，F7对应的值在frameworks\base\core\java\android\view\KeyEvent.java有现成定义:
 public static final int KEYCODE_F7 =137;
另外，在root过的Android设备上直接更改system\usr\keylayout\gpio-keys.kl文件，可以为设备的按键直接更改功能。
    
`议将整个源代码重新编译一下，在进行烧写。`

##gpio口对应关系
msm8909-mtp.dtsi文件
![msm8909_gpio1](http://ohjvpki1b.bkt.clouddn.com/msm8909_gpio1.png)
msm8909-pinctrl.dtsi文件
![msm8909_gpio2.png](http://ohjvpki1b.bkt.clouddn.com/msm8909_gpio2.png)
原理图上对应管脚
![msm8909_gpio3.png](http://ohjvpki1b.bkt.clouddn.com/msm8909_gpio3.png)
硬件手册对应gpio口
![msm8909_gpio4.png](http://ohjvpki1b.bkt.clouddn.com/msm8909_gpio4.png)
##说明
配置好gpio后，重新编译源代码烧写系统，短接该gpio口就可以在java侧收到F7键盘消息，通过该消息监听该gpio口输入。

监听键盘消息参照
[http://www.cnblogs.com/QUSIR/p/6245848.html](http://www.cnblogs.com/QUSIR/p/6245848.html)
