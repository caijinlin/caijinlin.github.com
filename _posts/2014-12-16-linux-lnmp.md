---
layout: post
title: "lnmp环境及navicat配置安装"
description:"lnmp"
keywords:"Nginx,mysql"
category:linux
tags: [Nginx,php]
---
{{% include JB/setup %}

####lnmp环境安装及解决遇到的问题

今天搭建了2台linux服务器lnmp环境，安装的过程中也是遇到一些问题。总结一下：如果nginx，mysql，php安装了多次，即使卸载，但是进程仍然在运行，所以每次重新安装的时候，最好kill掉原进程。在解决问题的过程中，还学了一些平时没怎么用的linux命令，算是另外的收获吧，折腾一天也值了。

<!-- more -->

####shell命令一键安装lnmp：

CentOS系统下执行：wget -c http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz && tar zxf lnmp1.1-full.tar.gz && cd lnmp1.1-full && ./centos.sh

Debian系统下执行：wget -c http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz && tar zxf lnmp1.1-full.tar.gz && cd lnmp1.1-full && ./debian.sh

Ubuntu系统下执行：wget -c http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz && tar zxf lnmp1.1-full.tar.gz && cd lnmp1.1-full && ./ubuntu.sh

更多了解访问<a href="http://lnmp.org/install.html" target="_blank">lnmp官网</a>

测试lnmp安装是否成功：

	service nginx status;
	service mysql status;

####navicat软件下载(数据库管理软件,可以连接远程数据库)

<a href="http://pan.baidu.com/s/1qW6uj5y" target="">点击下载</a>
	
####遇到的问题

#### 1.mysql: Unknown OS character set 'GB18030'. 
     mysql: Switching to the default character set 'latin1'.

问题所在：操作系统编码问题

解决方案：

	step1.更改操作系统字符集(shell命令)

		vim /etc/sysconfig/i18n   将LANG="zh_CN.GB18030"改成LANG="zh_CN.UTF-8"

	step2.设置环境变量(shell命令)

		exoport LANG = "zh_CN.UTF-8"

	step3.立刻生效环境更改

		source /etc/profile

####2.mysql登录

	mysql -u root p123456   <!-- 假设root为数据库用户名，123456为数据库密码-->

####3.使用数据库管理工具navicat连接远程数据库出现无法连接的问题

	![远程连接错误](assets\images\Linkerror.png)

排查错误：
	
1.远程服务器端口3306是否打开
	
	step1:客户机上 telnet ipAddress 3306，看服务器的3306端口是否被打开

2.远程服务器是否不允许从远程登陆

	运行shell命令：netstat -an | grep 3306

	如果上图红色部分为127.0.0.1，则说明只能本机访问，不被允许远程访问

解决方案：登录服务器mysql，设置允许客户机192.168.30.105以用户root,密码root登陆

	GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.30.105' IDENTIFIED BY 'root' WITH GRANT OPTION;



