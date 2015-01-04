---
layout: post
title: "ThinkPHP实现二级菜单"
description: "ThinkPHP"
keywords: "二级菜单"
category: php
tags: [ThinkPHP]
---
{% include JB/setup %}

###thinkphp动态装载二级菜单


我们不使用任何php框架的时候，是可以很轻松实现两级菜单的。使用thinkphp时，需要嵌套使用循环并建立关联才能实现。

<!-- more -->

####数据库实体表category,字段为

	categoryId, className, parentId
	分类id,     分类名称,  父类id

####控制层IndexAction.class.php

    function index()
    {
		$m = M('category');
		$list = $m->where('parentId=0')->select(); //获取一级菜单
		foreach($list as $id => $val){
			$list[$id]['class']=$m-where('parentId='.$val['categoryId'])->select();
		}
		$this->assign('list',$list);
		$this->display();
    }

####显示层输出显示index.html

	<volist name="list" io="l1">
		<ul>{$l1.className}</ul>
		<volist name="l1['class']" id="l2">
			<li>{$l2.className}</li>
		</volist>
	</volist>
