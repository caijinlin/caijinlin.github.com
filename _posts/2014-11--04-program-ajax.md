---
layout: post
title: "ajax同步和异步问题"
description: "ajax"
keywords: "ajax"
category: 编程
tags: [ajax]
---
{% include JB/setup %}

ajax同步和异步问题

今天在写写js代码的时候遇到AJAX加载数据都需要考虑代码运行顺序问题。最近的项目用了到AJAX同步。这个同步的意思是当JS代码加载到当前AJAX的时候会把页面里所有的代码停止加载，当这个AJAX执行完毕后才会继续运行ajax后面的其他代码（相当于单线程，代码顺序执行），页面假死状态解除。 

<!-- more -->
	
而异步则这个AJAX代码运行中的时候，ajax后面其他代码一样可以同时运行，相当于多线程（线程之间同步执行，线程内部顺序执行）。 

jquery的async:false,这个属性 默认是true：异步，false：同步。

  $.ajax({ 

         type: "post", 

         url: "path", 

         cache:false, 

         async:false, 

         dataType: "json", 

         success: function(data){ 

          } 

  });


在这里，async默认的设置值为true，这种情况为异步方式，就是说当ajax发送请求后，在等待server端返回的这个过程中，前台会继续 执行ajax块后面的脚本，直到server端返回正确的结果才会去执行success，也就是说这时候执行的是两个线程，ajax块发出请求后一个线程 和ajax块后面的脚本（另一个线程）例：

  $.ajax({  

           type:"POST", 

           url:"del.aspx", 

           dataType:"html", 

            success:function(result){   //function1()

               f1(); 

               f2(); 

          } 

           failure:function (result) {  

              alert('Failed');  

           }, 

    } 

    function2(); 

在上例中，当ajax块发出请求后，他将停留function1(),等待server端的返回，但同时（在这个等待过程中），前台会去执行function2(),也就是说，在这个时候出现两个线程，我们这里暂且说为function1() 和function2()。

当把asyn设为false时，这时ajax的请求时同步的，也就是说，这个时候ajax块发出请求后，他会等待在function1（）这个地方，不会去执行function2()，知道function1()部分执行完毕        

####总结:如果ajax块请求对后面代码无影响时，可用异步；如果ajax块请求对后面代码有影响，或者存在数据交换，可用同步


