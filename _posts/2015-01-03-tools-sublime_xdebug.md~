---
layout: post
title: "Sublime下XDebug安装及使用"
description:"XDebug"
keywords:"XDebug"
category:工具
tags: [Sublime]
---
{{% include JB/setup %}

###Windows下安装及使用Sublime Text2/3 插件XDebug Client

sublime下配置xdebug,可以调试php程序，能够提高开发速度。今天花两个小时配置了xdebug，发现效率提升不少，更快地定位错误，提高解决问题的能力。以下是安装及使用步骤。

<!-- more -->

####安装步骤

step1.sublime 安装xdebug client

		ctrl+shift+p
		install pacakage
		xdebug client回车

step2.配置php xdebug：找到标签[XDebug],添加如下内容

		xdebug.remote_enable = on
		xdebug.remote_handler = "dbgp"
		xdebug.remote_host = "127.0.0.1"
		xdebug.remote_port = 9000 
		zend_extension="D:\Program Files\phpStudy\php53\ext\xdebug.dll" //找到xdebug.dll的位置

step3.配置sublime xdebug

点击sublime菜单栏的project->save project as,保存后会当前工程生成一个.sublime-project的文件，修改为如下：

	{
	"folders":
	[
		{
			"path": "/D/WWW"
		}
	],
	"settings":	
	{
		"xdebug": 
		{
			"path_mapping": {	}, 
			"url": "http://127.0.0.1/topic/uploadpic.php", 
			"super_globals": true,	
			"close_on_stop": true,	
			"port": 9000 
		}	
	 }
   }

 其中url为你当前要调试的页面，你调试什么页面就改成该页面的地址


####使用步骤

step1.设置断点

	在当前行代码标记：CTRL + F8

step2.打开调试面板

	ctrl+shift+p，然后输入Xdebug, 选择下拉的Xdebug:Start Debugging(Launch Browser),就会在浏览器中打开刚才的url.

![sublime](/assets/images/sublime.png)

step3.开始调试(可以在调试的过程中监控某些值)

	Ctrl+F8: 切换断点；
	Ctrl+Shift+F5: 运行到下一个断点；
	Ctrl+Shift+F6: 单步；
	Ctrl+Shift+F7: 步入；
	Ctrl+Shift+F8: 步出 
	Ctrl+Shift+F9: 开始调试 
	Ctrl+Shift+F10: 关闭调试

![sublime](/assets/images/setwatch.png)







