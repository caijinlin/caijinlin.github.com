---
layout: post
title: "mongodb安装及使用(二)"
description: "mongodb"
keywords: "mongodb"
category: php
tags: [php,mongodb]
---
{% include JB/setup %}

###Windows下mongodb安装及使用

对于开发人员来说，安装环境应该是一件很简单的事情，可每次都搞得这么臃肿，关键在于没能理会其精华。如果你的阅历还不够，可能会有很多坑在等着你。今天在安装Mongodb的时候就遇到了用MongoVUE连接远程mongodb的坑。

<!-- more -->

上一篇文章讲述<a href="http://caijinlin.github.io/php/2015-01/linux-mongodb.html" target="_blank">linux下mongodb安装及使用</a>，这一篇讲windows下Mongodb安装以及所遇到的坑。

step1.Download

<a href="http://pan.baidu.com/s/1bnjKMkf">点击下载</a>

step2.Install

解压下载文件到D:\mongodb(比如)

step3.Start Server(在D:\mongodb下)

    mkdir data
    mkdir logs
    cd bin
    mongod --dbpath D:\mongodb\data  (启动服务器端)

step4.Start Client(在D:\mongodb下)

    cd bin
    mongo
    show dbs;
    use admin
    db.addUser("root",123456)     //添加mongo用户，可以作为登录名
    db.auth("root",'123456');     //认证是否登录成功

step5.Install Servicet(在D:\mongodb下)

每次开机运行都需要在（cmd）下面手动运行Mongodb,所以将Mongo数据库安装成为Windows服务.
   
   D:\mongodb\bin>mongod --logpath D:\mongodb\logs\MongoDB.log --logappend --dbpath D:\mongodb\data --directoryperdb --serviceName MongoDB --install

<font style="font:red">注意：必须为管理员权限， 否则无法创建服务</font>

安装服务成功后：
   
    启动MongoDB：net start MongoDB
    停止MongoDB：net stop MongoDB
    删除MongoDB：sc delete MongoDB

###安装MongoVUE

为了使mongodb可视化，安装Mongodb图形化管理工具,<a href="http://pan.baidu.com/s/1hqpaxMk" target="_blank">点击下载Mongodb客户端</a>

<font style="font:red">注意：连接时几个选项:</font>

    Server：主机名或者ip
    Port：端口，一般为27017
    Username:当连本机时，可以不填；远程就填刚才在Mongo中创建的admin用户，不是计算机用户
    Password:当连本机时，可以不填；远程就填刚才在Mongo中创建的admin密码，不是计算机密码
