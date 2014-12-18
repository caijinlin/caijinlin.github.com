---
layout: post
title: "轻松实现多台服务器数据库备份"
description: "linux"
keywords: "Nginx,mysql"
category: linux
tags: [Nginx,php,mysql]
---
{% include JB/setup %}

####linux下两台服务器数据库导入导出

两台服务器环境均为lnmp(linux+nginx+mysql+php),今天一直想实现服务器A备份数据库test，并导入到服务器B中，传统的方法是通过linux共享实现的，具体可以参考<a href="http://www.2cto.com/os/201108/102000.html" target="_blank">linux服务器共享</a>

<!-- more -->

####我的解决办法

充分利用lnmp环境，可以通过ip直接访问的特点，使用mysqldump备份A服务器数据库到网站根目录下，然后在服务器B中通过wget命令进行下载，最后使用mysql自带命令source导入B机数据库，其本质就是实现数据库同步。具体实现及常见问题解决办法如下：

假设A机ip为192.168.12.1 ,B机为192.168.12.2,，假设数据库用户和密码均为root将A机数据库test导入到B机数据库test，使用<a href="http://pan.baidu.com/s/1eQ3qPf8" target="_blank">xshell软件</a>通过ssh协议连接两台linux服务器，方便操作.

####备份A机数据库test

	cd www  /*切换到网站根目录*/
	mysqldump -uroot -proot test > test.sql

####下载A机数据库test到B机
	
	wget -c http://192.168.12.1/test.sql

注：如果192.168.12.1的80端口没有打开，可能会出现一下错误：

	正在连接 192.168.12.1:80... 失败：没有到主机的路由

排查错误：

定位A机，查看防火墙80端口是否对外开放

	/etc/init.d/iptables status  或者 service  iptables status;

以下情形没有打开80端口，3306和22端口是开放的

![防火墙设置](/assets/images/firewallPort.png)

这时需要自行进行防火墙设置，可以开启80端口

	/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT 
	#/sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT
	/etc/init.d/iptables save

为了安全起见，我没有开启80端口，而是先关闭防火墙，让B机执行wget命令后，再开启防火墙

	/etc/init.d/iptables stop   (A机操作)

此时，在B机执行命令，就没有问题了，下载的文件在当前目录
	
	wget -c http://192.168.12.1/test.sql

再回到A机，重新开启防火墙
	
	/etc/init.d/iptables restart 

####导入数据库到B机

在当前目录，登录到mysql后，使用source命令进行导入

	mysql -uroot -proot;
	use test;
	source test.sql;