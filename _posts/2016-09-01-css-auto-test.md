---
layout: post
title: 【译文】使用PhantomCSS对CSS进行回归测试(Using PhantomCSS for Regression Testing Your CSS)
description: "UI自动化测试"
modified: 2016-09-01
tags: [sample post]
image:
  feature: abstract-1.jpg
---

*本文章原文链接[地址](http://mattsnider.com/using-phantomcss-for-regression-testing-your-css/)*

　　我总算有机会玩了一下[PhantomCSS](https://github.com/Huddle/PhantomCSS)，这是一个对CSS进行回归测试的工具，它通过对屏幕截图的比较来判断CSS代码的变化是否对页面产生了影响，在这里我想跟大家分享一下我的经验。我们在Ariba公司将它用于验证当我们将两个巨大的未整合的CSS文件合并成一个简小的模块化的CSS时，是否破坏了页面的视觉效果，另外它能帮助我们更好地书写less文件。

　　PhantomCSS使用了[CasperJS](http://github.com/n1k0/casperjs)，在[PhantomJS](http://github.com/ariya/phantomjs/)（一个具有截图功能的测试框架）的基础上构建，另外采用[ResembleJS](http://huddle.github.com/Resemble.js/)比对图片。

　　通过下载的方式使用所有的子组件时出奇的简单，但当我想使用```npm```命令来使用这些已安装的子组件时，一下子变得困难起来了。

### 准备

　　最简单的开始方式就是通过克隆这个项目包然后通过```cd```命令进入：

{% highlight linenos %}
git clone https://github.com/Huddle/PhantomCSS.git
cd PhantomCSS
{% highlight%}

{% highlight javascript linenos %}
/*
    Initialise CasperJs
*/
 
phantom.casperPath = 'CasperJs';
phantom.injectJs(phantom.casperPath + '/bin/bootstrap.js');
phantom.injectJs('jquery.js');
 
var casper = require('casper').create({
    viewportSize: {
        width: 1027,
        height: 800
    }
});
 
/*
    Require and initialise PhantomCSS module
*/
 
var phantomcss = require('./phantomcss.js');
 
phantomcss.init({
    screenshotRoot: './screenshots',
    failedComparisonsRoot: './failures'
});
 
/*
    The test scenario
*/
 
var url = 'http://www.mattsnider.com'; // replace with your URL
 
casper.
    start(url).
    // screenshot the initial page load
    then(function() {
        phantomcss.screenshot('<SELECTOR_TO_SCREENSHOT>', '<LABEL_SCREENSHOT>');
    }).
    then(function() {
        // do something
    }).
    // second screenshot
    then(function() {
        phantomcss.screenshot('<SELECTOR_TO_SCREENSHOT>', '<LABEL_SCREENSHOT>');
    });
 
/*
    End tests and compare screenshots
*/
 
casper.
    then(function now_check_the_screenshots() {
        phantomcss.compareAll();
    }).
    run(function end_it() {
        console.log('\nTHE END.');
        phantom.exit(phantomcss.getExitStatus());
    });
{% endhighlight %}



  

