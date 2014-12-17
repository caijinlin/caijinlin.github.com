---
layout: post
title: "MYSQL存储引擎"
description: "MYSQL表结构"
keywords: "MySQL存储引擎"
category: MYSQL
tags: [MyISAM,InnoDB,mysql]
---
{% include JB/setup %}

###MYSQL两种最常用的存储引擎

InnoDB和MyISAM是许多人在使用MySQL时最常用的两个表类型，这两个表类型各有优劣，视具体应用而定。基本的差别为：MyISAM类型不支持事务处理等高级处理，而InnoDB类型支持。MyISAM类型的表强调的是性能，其执行数度比InnoDB类型更快，但是不提供事务支持，而InnoDB提供事务支持已经外部键等高级数据库功能。
<!-- more -->

####主要区别

	1.InnoDB不支持FULLTEXT类型的索引。
	2.InnoDB 中不保存表的具体行数，也就是说，执行select count(*) from table时，InnoDB要扫描一遍整个表来计算有多少行，但是MyISAM只要简单的读出保存好的行数即可。注意的是，当count(*)语句包含 where条件时，两种表的操作是一样的。
	3.对于AUTO_INCREMENT类型的字段，InnoDB中必须包含只有该字段的索引，但是在MyISAM表中，可以和其他字段一起建立联合索引。package，搜索ctags就能找到ctags的插件，安装之。
	4.DELETE FROM table时，InnoDB不会重新建立表，而是一行一行的删除。tags文件，那么这个时候就可以定位函数了。(具体参考<a href="https://github.com/SublimeText/CTags#additional-setup-steps" target="_blank">官方API</a>)

####应用场景一般规则

	1.应用程序要使用事务时必须采用InnoDB引擎
	2.应用程序对查询性能要求较高，就要使用MYISAM了。MYISAM索引和数据是分开的，而且其索引是压缩的，可以更好地利用内存。
