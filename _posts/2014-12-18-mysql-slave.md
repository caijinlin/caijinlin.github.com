---
layout: post
title: "mysql主从复制配置"
description: "mysql主从"
keywords: "mysql主从"
category: mysql
tags: [mysql]
---
{% include JB/setup %}

###mysql主从配置步骤

两台服务器的mysql版本一般规定为一致，这样可以避免一些乱七八糟的错误。
建立基本的复制可以总结为一下三个简单步骤:

<!-- more -->

	step1.配置一个服务器为Master;
	step2.配置一个服务器作为Slave;
	step3.将slave连接到Master;

为更好阐述，假设Master服务器为192.168.12.1,Slave服务器为192.168.12.2

####配置Master(在192.168.12.1机器上操作)

step1.添加配置选项(log-bi,log-bin-index,server-id)到mysql配置文件中 

vi /etc/my.cnf

	[mysqld]
	port            = 3306
	socket          = /tmp/mysql.sock
	datadir = /usr/local/mysql/var
	skip-external-locking
	key_buffer_size = 16M
	max_allowed_packet = 1M
	table_open_cache = 64
	sort_buffer_size = 512K
	net_buffer_length = 8K
	read_buffer_size = 256K
	read_rnd_buffer_size = 512K
	myisam_sort_buffer_size = 8M
	log-bin = master-bin
	log-bin-index = master-bin.index
	server-id = 1

登陆到mysql后,命令show variables like 'server%'可以看到服务器id是不是刚才写在配置文件的

step2.在Master创建一个复制用户

登陆到mysql后，执行以下命令(可自行设置新建用户名和密码)

	GREATE USER repl_user;
	GRANT REPLICATION SLAVE ON *.* TO repl_user@'192.168.12.2' IDENTIFED BY '123456';

####配置Slave(在192.168.12.2机器上操作)

step1.添加配置选项(relay-log-index,relay-log,server-id)到mysql配置文件中 ,server_id与Master不同

vi /etc/my.cnf

	[mysqld]
	port            = 3306
	socket          = /tmp/mysql.sock
	datadir = /usr/local/mysql/var
	skip-external-locking
	key_buffer_size = 16M
	max_allowed_packet = 1M
	table_open_cache = 64
	sort_buffer_size = 512K
	net_buffer_length = 8K
	read_buffer_size = 256K
	read_rnd_buffer_size = 512K
	myisam_sort_buffer_size = 8M
	server-id = 2
	relay-log-index = slave-relay-bin.index
	relay-log = slave-relay-bin

####将slave连接到Master(在slave服务器192.168.12.2上操作)

登陆到mysql后执行：(MASTER_USER和MASTER_PASSWORD为之前在MASTER上面创建的用户和密码)
	
	CHANGE MASTER TO MASTER_HOST = '192.168.12.1', MASTER_PORT = 3306, MASTER_USER = 'repl_user', MASTER_PASSWORD = '123456'；

然后启动slave,检测是否配置成功

	START SLAVE;
	SHOW SLAVE STAUS\G;

如果显示结果中，其中两项为yes,则配置成功    

	Slave_IO_Running: Yes
    Slave_SQL_Running: Yes

####常见问题

我在配置的时候，并没有一次性成功。当我启动slave后，发现

	Slave_IO_Running: No
    Slave_SQL_Running: Yes

即从服务器IO线程没有启动，报错：

	Fatal error: The slave I/O thread stops because master and slave have equal MySQL server ids

####解决方法

这应该是比较常见的错误了，我slave上，登陆到mysql,执行show variables like 'server%'命令
发现从服务器的server_id 还是1，为什么没有改过来呢？

	mysql> show variables like 'server%';
	mysql> find / -name "my.cnf";

发现当前目录下my.cnf有多处，可能是上一次安装残留文件

针对此种情形,进行全局设置,并停止slave后，再启动

	mysql> set global server_id = 2;
	mysql> stop slave;
	mysql> start slave;
	mysql> show slave status\G;

此时问题解决，slave服务器Io和Sql线程均顺利启动

####总结

	此时在主服务器运行 netstat -antlp | grep 3306, 发现本机和从服务器192.168.12.2都在监听192.168.12.1，主从复制配置顺利完成。






