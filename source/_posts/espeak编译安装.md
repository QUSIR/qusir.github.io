---
title: espeak编译安装
date: 2017-06-07 17:06:20
tags:
---


##说明
&emsp;eSpeak是用于Linux和Windows的英文和其他语言的紧凑型开源软件语音合成器。eSpeak使用“共振峰综合”方法。这允许以小尺寸提供许多语言。讲话清晰，可以高速使用，但并不像基于人类语音记录的较大合成器那样自然或平滑。

注意：在安装espeak之前必须安装portaudio框架，用于驱动声卡。

[espeak官网](http://espeak.sourceforge.net/)
##编译portaudio
	7z x portaudio.7z
	cd portaudio
	./configure
	make 
	sudo make install

##编译espeak
	7z x espeak-1.48.01-source.7z
	cd espeak-1.48.01-source/
	cd src/
	make
	sudo make install
##安装中文和粤语支持
在安装中文支持前，保证espeak编译安装成功

进入`/espeak-1.48.01-source/dictsource$`目录
安装中文

	espeak --compile=zh
安装粤语

	espeak --compile=zh-yue
##测试

默认

	espeak  hello -w hello.wav
粤语

	espeak -vzhy 你好 -w test.wav
中文

	espeak -vzh 你好 -w test.wav
##编译需要的源码

[zhy_list.zip](http://ohjvpki1b.bkt.clouddn.com/zhy_list.zip)

[zh_listx.zip](http://ohjvpki1b.bkt.clouddn.com/zh_listx.zip)

[portaudio.7z](http://ohjvpki1b.bkt.clouddn.com/portaudio.7z)

[espeak-1.48.01-source.7z](http://ohjvpki1b.bkt.clouddn.com/espeak-1.48.01-source.7z)

##遇到问题
###问题1
wavegen.o: In function `WavegenOpenSound() [clone .part.2]':
wavegen.cpp:(.text+0x26c): undefined reference to `Pa_StreamActive'
wavegen.o: In function `WavegenCloseSound()':
wavegen.cpp:(.text+0x58e): undefined reference to `Pa_StreamActive'
collect2: error: ld returned 1 exit status
Makefile:105: recipe for target 'speak' failed
make: *** [speak] Error 1
###解决
	cp portaudio19.h portaudio.h
	make clean
	make

