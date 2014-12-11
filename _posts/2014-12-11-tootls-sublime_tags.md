---
layout: post
title: "Sublime函数跳转插件Ctags安装及使用"
description: "Ctags插件"
keywords: "Ctags"
category: 工具
tags: [Sublime]
---
{% include JB/setup %}

###Windows下安装及使用Sublime Text2/3 插件Ctags

sublime确实是一款非常不错的开发软件，用起来很爽，里面集成了很多插件，只要安装即可，
介绍下sublime中ctags插件的安装，安装这个插件之后就可以快速定位某函数了，非常方便。

<!-- more -->

####安装步骤

	1.下载并解压(http://pan.baidu.com/s/1o6umSjg)ctags包</a>中的ctags.exe到系统环境路径（默认压缩在c:\windows\system32就好了） 
	2.若没安装package control在这个插件的话，先安装它。
	3.现在安装开始ctags的插件了，在package control中选择install package，搜索ctags就能找到ctags的插件，安装之。
	4.然后在项目目录下右键选择“Ctags:Rebuild Tags” ，就生成了.tags文件，那么这个时候就可以定位函数了。(具体参考https://github.com/SublimeText/CTags#additional-setup-steps)官方API</a>

####使用步骤

	1.跳转进入：ctrl+t, ctrl+t
	2.退出返回：ctrl+t, ctrl+b
