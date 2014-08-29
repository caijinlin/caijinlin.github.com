---
layout: post
title: "快速找到段错误的core方法"
description: "gcc"
keywords: "gcc"
category: linux
tags: [gcc,core]
---
{% include JB/setup %}

### 段错误

####段错误一般表现为段吐核，随着程序的运行，如果边界考虑不全，非法引用了无效成员等，会导致此种情况

<!-- more -->
	
	/*段吐核问题解决演示*/
	#include <stdio.h>

	typedef struct stu
	{
	    int age;
	    char sex;
	}STU;

	int main()
	{
	    STU *stu1=NULL;
	    printf("%d\n",stu1->age);
	    return 0;
	}

![蔡金林的博客](/assets/images/error.png)

####解决方法

	1.ulimit -c unlimited  (解除生成核心转储信息文件的大小限制)

	2.gcc test.c -g (加入-g选项生成调试信息)

	3. ./a.out(有段错误，就会出现core文件)

	4.gdb ./a.out -c core (gdb调试)

	5.在gdb调试命令行下输入bt(back trace),它会显示出现错误的地方