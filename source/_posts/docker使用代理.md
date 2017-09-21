---
title: docker使用代理
date: 2017-04-08 22:30:33
tags:
---


#说明
	由于docker官方原版镜像被墙，使用Shadowsocks翻墙，通过privoxy由socker转为http协议，实现docker代理。

#创建目录
	mkdir -p /etc/systemd/system/docker.service.d

#创建配置文件
	vim /etc/systemd/system/docker.service.d/http-proxy.conf
写入以下数据

	[Service]
	Environment="HTTP_PROXY=http://proxy.example.com:80/"
`http://proxy.example.com`为代理地址
#更新配置
	sudo systemctl daemon-reload
#检查配置
	systemctl show --property=Environment docker

正常会输出以下信息
	
	Environment=HTTP_PROXY=http://proxy.example.com:80/
#重启docker
	sudo systemctl restart docker
#测试
	 docker pull ubuntu:14.04
	 docker run -it --rm ubuntu:14.04 bash