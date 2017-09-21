---
title: 使用android studio查看andrdoid源码
date: 2017-04-12 17:06:20
tags:
---

#说明
为了查看android源码，之前使用Source Insight查看源码，然后发现使用android studio查看源码也挺方便的。


##使环境变量生效
进入android源码目录执行以下指令

	source ./build/envsetup.sh 
编译完整个源码后执行以下指令编译idegen模

	mmm development/tools/idegen/
提示以下信息则编译成功

	...... #### make completed successfully (7 seconds) ####

接着执行以下脚本

	development/tools/idegen/idegen.sh
提示以下信息则编译成功

	Read excludes: 21ms Traversed tree: 194799ms


以上指令执行成功后会在源码目录下生成相应的工程文件`android.ipr`，然后打开android studio打开工程，使用`android.ipr`导入整个源码。