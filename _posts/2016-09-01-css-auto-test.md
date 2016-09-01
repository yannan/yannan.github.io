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

这个项目包包含了CasperJS, resembleJS, 以及PhantomJS的版本。然而，这个内置的PhantomJS只能在windows平台上工作。对于其他操作系统的安装，参见[PhantomJS安装指南](http://phantomjs.org/download.html)。除了PhantomJS，你需要确保你使用的是内置的软件版本(如果你没有改任何东西，你使用的就是内置的包)。我最初想使用之前已经通过NPM安装的CasperJS包，为此我浪费了很长的时间去让PhantomCSS依赖最新版本的CasperJS运行，即使内置的包能运行的好好的。

### 怎么去做

确保一切都是正常工作的情况下运行测试用例两次：

{% highlight linenos %}
phantomjs demo/testsuite.js
{% highlight%}

第一次你应该会看到下面的结果：

{% highlight linenos %}
Must be your first time?
Some screenshots have been generated in the directory ./screenshots
This is your 'baseline', check the images manually. If they're wrong, delete the images.
The next time you run these tests, new screenshots will be taken.  These screenshots will be compared to the original.
If they are different, PhantomCSS will report a failure.
 
THE END.
{% highlight%}

第二次运行：

{% highlight linenos %}
PhantomCSS found: 4 tests.
None of them failed. Which is good right?
If you want to make them fail, go change some CSS - weirdo.
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



  

