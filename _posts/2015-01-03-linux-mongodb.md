---
layout: post
title: "mongodb安装及使用"
description: "mongodb"
keywords: "mongodb"
category: php
tags: [mongodb,linux]
---
{% include JB/setup %}

###linux下mongodb安装及使用

mongodb作为一种把数据存储在磁盘的非关系型数据库NOSql，具有一些关系数据库的特性，可以完成大部分sql语句的功能。并且MongoDB内置的水平扩展机制提供了从百万到十亿级别的数据量处理能力，存储的每一条记录可以看作是一个Document对象，并支持排序，得到了较好的应用。为了更好的学习，自己在linux下也动手配置mongodb了。

<!-- more -->

step1.download

	 wget http://fastdl.mongodb.org/linux/mongodb-linux-i686-2.4.2.tgz

step2.decompression 

	 tar -zxvf mongodb-linux-i686-2.4.2.tgz
	 mkdir /usr/local/mongodb
	 sudo mv -Rf mongodb-linux-i686-2.4.2/* /usr/local/monogodb
	
step3.创建数据库和日志的存储目录：

	 mkdir -p /usr/local/mongodb/data
     mkdir -p /usr/local/mongodb/logs

step4.修改mongodb的数据库和日志存储位置,并设置mongodb为后台启动

	sudo /usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/ --logpath=/usr/local/mongodb/logs/mongdb.log --fork

step5.if the step4 has error like "error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory"

	yum whatprovides libstdc++.so.6
     
	yum install libstdc++-4.4.7-3.el6.i686(对应上一步出现的包名)

step6.如果执行了step5,j就需要重新执行step4;否则不需要

step7.进入mongodb命令行模式

	cd /usr/local/mongodb/bin
	./mongo

step8.<a href="http://www.111cn.net/sys/linux/58162.htm" target="_blank">开启php的Mongodb扩展</a> 就可以在php代码中使用Monggodb了


step9.全面学习Mongodb(<a href="http://blog.csdn.net/yiqijinbu/article/details/9053467" target="_blank">http://blog.csdn.net/yiqijinbu/article/details/9053467</a>)

