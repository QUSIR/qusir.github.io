---
title: mysql删除有空格字符名称的触发器
date: 2017-04-12 17:06:20
tags:
---


之前在mysq添加触发器的过程中，使用名称不规范使产生如下触发器名称：

	door_machine_insert_ trigger

中间存在空格字符

使用以下删除语句时候提示出错

	DROP TRIGGER door_machine_insert_ trigger；


然后改为

	DROP TRIGGER `door_machine_insert_ trigger`;

删除成功