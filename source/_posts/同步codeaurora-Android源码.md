---
title: 同步codeaurora Android源码
date: 2017-01-21 17:12:57
tags:
---

##说明
codeaurora没有被墙

##源码目录

[https://source.codeaurora.org/quic/la/](https://source.codeaurora.org/quic/la/)

##官网
[https://www.codeaurora.org/](https://www.codeaurora.org/)

##高通源码

[https://wiki.codeaurora.org/xwiki/bin/QAEP/release](https://wiki.codeaurora.org/xwiki/bin/QAEP/release)

##初始化
	repo init -u git://codeaurora.org/platform/manifest.git -b release -m LA.BR.1.2.3-10210-8x09.0.xml --repo-url=git://codeaurora.org/tools/repo.git --repo-branch=caf-stable

说明：参数LA.BR.1.2.3-10210-8x09.0.xml可以通过https://wiki.codeaurora.org/xwiki/bin/QAEP/release获得


##同步

	repo sync -j16