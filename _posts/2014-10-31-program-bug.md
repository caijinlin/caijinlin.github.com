---
layout: post
title: "TargetInvocationException异常"
description: "s实体类反射"
keywords: "TargetInvocationException"
category: 编程
tags: [bug]
---
{% include JB/setup %}

### 未处理TargetInvocationException，调用的目标发生了异常

今天在使用数据库表实体类映射的时候，出现TargetInvocationException错误，单步调试也没能找出问题来源。

后面才知道因为版本的原因，类名发生冲突，导致实例化的类并没用调用到相应的构造函数，导致接收数据失败。

<!-- more -->
	
写在博客上，以防后面再出这样的错误：

![蔡金林的博客](/assets/images/voteresult.png)

重命名类名后，可以发现实例化后的对象，具有与数据库字段相匹配的属性：

![蔡金林的博客](/assets/images/votebug.png)

