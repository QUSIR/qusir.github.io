---
title: 使用ngrok实现内网穿透
date: 2017-04-21 17:06:20
tags:
---


##安装依赖项

	sudo apt-get install supervisor
	
	sudo apt-get install mercurial git gcc 

##安装go
	tar -C /usr/local -xzf go1.8.1.linux-amd64.tar.gz
	echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc
	
	使环境变量生效

	source .bashrc 
##同步代码
	git clone https://github.com/inconshreveable/ngrok.git

##生成密匙

	openssl genrsa -out rootCA.key 2048
	openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=lianghuanhan.club" -days 5000 -out rootCA.pem
	openssl genrsa -out device.key 2048
	openssl req -new -key device.key -subj "/CN=lianghuanhan.club" -out device.csr
	openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000
  
##复制密匙

	cp rootCA.pem assets/client/tls/ngrokroot.crt
	cp device.crt assets/server/tls/snakeoil.crt
	cp device.key assets/server/tls/snakeoil.key

##编译源码
	make release-server
	
	make release-client
	
	编译win7 64位客户端
	
	GOOS=windows GOARCH=amd64 make release-client
 
##启动服务器端

	./ngrokd -domain="lianghuanhan.club" -httpAddr=":8088" -httpsAddr=":8089"
 
##启动客户端
	编辑配置文件
 	vim ngrok.cfg 
 	写入以下数据

	server_addr: lianghuanhan:4443
	trust_host_root_certs: false

###linux 64位客户端
	监听http 80端口
	./ngrok -subdomain pub -proto=http -config=ngrok.cfg 80
	监听tcp 22端口
	./ngrok -subdomain pub -proto=tcp -config=ngrok.cfg 22

###windows 64位客户端
	监听远程桌面端口
	ngrok.exe -subdomain pub -proto=tcp -config=ngrok.cfg 3389


##在浏览器中输入：localhost:4040 (在客户端上)

可以查看所有的请求情况！

![view](http://ohjvpki1b.bkt.clouddn.com/ngrok-demo1.jpg)

##注意事项

1.为lianghuanhan.club添加dns解析

添加两条A记录：lianghuanhan.club和*.lianghuanhan.club，指向lianghuanhan.club所在的服务器ip。

2.客户端ngrok.cfg中server_addr后的值必须严格与-domain以及证书中的`"/CN=lianghuanhan.club"`相同，必须先生成证书，拷贝到相应目录，再编译代码。