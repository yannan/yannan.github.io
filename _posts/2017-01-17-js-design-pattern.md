---
layout: post
title: 《javascript设计模式》笔记
description: "js design pattern"
modified: 2017-01-17
tags: [sample post]
image:
  feature: abstract-2.jpg
---

*此为《javascript设计模式》的读书笔记，用以记录自己的所学*


### 函数是一等对象
它们可以存储在变量中，可以作为参数传递，可以作为返回值，可以在运行时进行构造，它是用于构建传统的面向对象框架的基础。
创建匿名函数示例：

{% highlight javascript linenos %}
    (function() {
        var foo = 10;
        var bar = 2;
        alert(foo * bar);
    })();
{% endhighlight %}

匿名函数最有趣的用途是用来创建闭包。**闭包**是一个受到保护的变量空间，由内嵌函数生成。Javascript具有函数级的作用域。这意味着定义在函数内部的变量在函数外部不能被访问。Javascript的作用域又是词法性质的，这意味着函数运行于它定义的作用域中，而不是运行的作用域中。

###对象的易变性
在Javascript中，一切都是对象，而且所有对象都是易变的。可以对先前定义的类和实例化的对象进行修改。
