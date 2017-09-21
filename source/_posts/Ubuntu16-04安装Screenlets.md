---
title: Ubuntu16.04安装Screenlets
date: 2017-01-21 17:03:45
tags:
---


##通过添加软件源的方式安装装

    sudo add-apt-repository ppa:screenlets/ppa
    sudo apt-get update
    sudo apt-get install screenlets
以上方法在ubuntu16.04下不可行  
##以源码的方式安装
    sudo apt install python-beautifulsoup python-wnck python-vte python-tz
    cd /tmp
    wget -O - https://launchpad.net/screenlets/trunk/0.1.6/+download/screenlets-0.1.6.tar.bz2 | bunzip2 -c - | tar xf -
    wget -O - https://launchpad.net/indiv-screenlets/trunk/0.1.6/+download/indiv-screenlets-0.1.6.tar.bz2 | bunzip2 -c - | tar xf -
    cd screenlets-0.1.6
    sudo setuo.py install
    cd ../indiv-screenlets-0.1.6
    sudo setup.py install
    screenlets &
    
##出错解决
在终端下执行screenlets指令在运行过程中会提示出错信息。发现缺少相关库，使用以下指令安装。

	sudo apt-get install python-gconf
    sudo apt-get install libcanberra-gtk-module
    sudo apt-get install python-xdg

##使用方法
打开screenlets双击相应的桌面控件，便会在桌面生成，相应的小程序，点击开始/停止也可以，或者点击登录时自启动，使桌面小程序，开机显示在桌面。

##设置界面
![](http://ohjvpki1b.bkt.clouddn.com/Screenlets1.png)
##效果图

![](http://ohjvpki1b.bkt.clouddn.com/Screenlets2.png)