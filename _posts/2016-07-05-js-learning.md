---
layout: post
title: js学习
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2015-12-4
tags: [sample post]
image:
  feature: abstract-1.jpg
---

##jQuery 事件

    $(document).ready(function() {
	    <!-- 这是代码 -->
    })

可以简写成

    $().ready(function() {
    	<!-- 这是代码 -->
    })

此外，这个工厂函数可以接受另一个函数作为参数，jQuery会在内部执行一次对.ready()的调用，因此下面代码也起同样的作用：

    $(function() {
        <!-- 这是代码 -->
    })

##jQuery与其他库共存

jQuery提供了一个``noConflict()``方法将``$``的使用权交出于其他库，自身使用``jQuery()``调用，代码如下：

    jQuery().noConflict();

可以另外回调函数在内部重新使用``$``，而不会引起冲突：

    jQuery(document).ready(function($) {
    	// 这里可以正常使用$
    })

##jQuery复合事件

jQuery中的``.toggle()``、``.hover()``方法被称为复合事件处理程序。  
``.toggle()``方法支持两个参数，且两个参数都是函数,通过单击事件触发。

    $(document).toggle(function() {
            //函数1
        }, function() {
            //函数2
            });

``.hover()``事件也支持两个函数参数，第一个函数会在鼠标移入对象时执行，第二个函数会在移出时执行。

