---
layout: post
title: arguments简析
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2014-12-4
tags: [sample post]
image:
  feature: abstract-1.jpg
---

#arguments对象
##转自JavaScript秘密花园
javascript中每个函数内部都能访问一个特别的变量，那就是**arguments**。这个变量维护着所有传递到这个函数中的参数列表。

```
function foo(a,b,c,d) {
    console.log(arguments); //[1,2,3,4]
    console.log(arguments.length); //4
    console.log(typeof arguments); //Object
}
foo(1,2,3,4);
```

上面代码输出的是一个数组，但实际上arguments变量**不是**一个数组，尽管在语法上它具有数组的相关属性`length`,但它不从Array.prototype继承，实际上它是一个对象。

```
Array.prototype.testArg = "array";
function funcArg() {
    console.log(arguments.testArg);
    console.log(arguments[0]);
}
console.log(new Array().testArg);//array
funcArg(10);//"undefined", 10
```

arguments对象的长度是由实参个数而不是形参个数决定的。

```
function foo(a,b,c,d) {
    console.log(arguments.length);
}
foo(1,2);//2
foo(1,2,3);//3
```