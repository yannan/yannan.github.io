---
layout: post
title: 【译文】使用PhantomCSS对CSS进行回归测试(Using PhantomCSS for Regression Testing Your CSS)
description: "UI自动化测试"
modified: 2016-09-02
tags: [sample post]
image:
  feature: abstract-1.jpg
---

*本文章原文链接[地址](http://mattsnider.com/using-phantomcss-for-regression-testing-your-css/)，水平有限，如有翻译不当之处，请谅解并希望能指出。*

　　我总算有机会玩了一下[PhantomCSS](https://github.com/Huddle/PhantomCSS)，这是一个对CSS进行回归测试的工具，它通过对屏幕截图的比较来判断CSS代码的变化是否对页面产生了影响，在这里我想跟大家分享一下我的经验。我们在Ariba公司将它用于验证当我们将两个巨大的未整合的CSS文件合并成一个简小的模块化的CSS时，是否破坏了页面的视觉效果，另外它能帮助我们更好地书写less文件。

　　PhantomCSS使用了[CasperJS](http://github.com/n1k0/casperjs)，在[PhantomJS](http://github.com/ariya/phantomjs/)（一个具有截图功能的测试框架）的基础上构建，另外采用[ResembleJS](http://huddle.github.com/Resemble.js/)比对图片。

　　通过下载的方式使用所有的子组件时出奇的简单，但当我想使用```npm```命令来使用这些已安装的子组件时，一下子变得困难起来了。

### 准备

　　最简单的开始方式就是通过克隆这个项目包然后通过```cd```命令进入：

{% highlight html linenos %}
git clone https://github.com/Huddle/PhantomCSS.git
cd PhantomCSS
{% highlight%}

这个项目包包含了CasperJS, resembleJS, 以及PhantomJS的版本。然而，这个内置的PhantomJS只能在windows平台上工作。对于其他操作系统的安装，参见[PhantomJS安装指南](http://phantomjs.org/download.html)。除了PhantomJS，你需要确保你使用的是内置的软件版本(如果你没有改任何东西，你使用的就是内置的包)。我最初想使用之前已经通过NPM安装的CasperJS包，为此我浪费了很长的时间去让PhantomCSS依赖最新版本的CasperJS运行，即使内置的包能运行的好好的。

### 怎么去做

确保一切都是正常工作的情况下运行测试用例两次：

{% highlight html linenos %}
phantomjs demo/testsuite.js
{% highlight%}

第一次你应该会看到下面的结果：

{% highlight html linenos %}
Must be your first time?
Some screenshots have been generated in the directory ./screenshots
This is your 'baseline', check the images manually. If they're wrong, delete the images.
The next time you run these tests, new screenshots will be taken.  These screenshots will be compared to the original.
If they are different, PhantomCSS will report a failure.
 
THE END.
{% highlight%}

第二次运行：

{% highlight html linenos %}
PhantomCSS found: 4 tests.
None of them failed. Which is good right?
If you want to make them fail, go change some CSS - weirdo.
{% highlight%}

这个测试用例创建了一个简易的nodeJS服务器，并向服务器发送了一些请求。屏幕的截图会存放在```screenshots```目录下。如果你想知道当回归被检测出来时发生了什么，改变一些```demo/coffeemachine.html```文件的样式或者可见文本，然后再次运行```testsuite.js```。

现在开始尝试你自己的测试用例。创建一个新的测试文件:

{% highlight html linenos %}
touch demo/mytest.js
{% highlight %}

然后将下面的代码写到文件里面(改写自```testsuite.js```)：

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

必须改变casper测试地址，然后将它对你的网站运行：

{% highlight html linenos %}
phantomjs demo/mytest.js
{% highlight %}

### 它是如何工作的

大多数的导航和测试工作是由CasperJS来处理的，因此很有可能你想要的都在这个[文档](http://docs.casperjs.org/en/latest/modules/index.html)。即便如此，在发布PhantomCSS时仍旧使用的是Casper 1.0.2版本，但是这个文档是针对1.1.0版本的。(我没有找到能用的1.02版本的文档，如果你找到的话，请在评论里让我知道)。创建了一个简单的例子(```mytest.js```)，只是打开了一个URL，截了一个屏，做了一些你指定的事情，然后又截了一个屏。通过这个切入点，你应该能够开始创建更具有综合性的测试了。

当PhantomCSS第一次运行时，它将会执行所有指定的命令，获得一系列的截图(通过```phantomcss.screenshot('', '');```命令指定)，但是```phantomcss.screenshot('', '');```命令将会在命令行告诉你没有可以比较的东西存在。你获得的只是依据你的网站当前版本未改变的屏幕截图。现在去改变你的网站的CSS，然后再次运行PhantomCSS，它将会生成新的截图并与原始截图相比较。如果两次的截图有不同的地方，PhantomCSS将会报错并告诉你问题出在哪。

```mytest.js```的上面的模板是用来发现并将组件库关联到PhantomJS，在我们使用CasperJS创建一个网页之前。接下来PhantomCSS初始化，知道截图和错误报告的目录。还有很多其他的参数属性可能需要指定，但这两个对基础测试是最重要的。CasperJS被```.start(url)```命令唤醒。CasperJS使用了回调模式，所以你可以使用链式函数声明(就像上面那样)，或者你可以分别的调用```casper.```命令（比如下面的那个模块）.

### 多说一点

最后，下面是关于CasperJS的一些忠告和陷阱，也许可以帮到你:

   * 在回调函数内部使用```this.echo("your string");```命令在终端中打印出一些信息(或者你也可以使用```require("utils").dump("your string");```)，但是```console.log()```命令不会在终端打印出任何信息。
   * 当你不确定元素是否被选择器正确获取时，使用```this.echo(this.getHTML(selector, true))```
   *在```this.evaluate(callback)```和```this.thenEvaluate(callback)```回调函数内部，你可以运行你所熟悉的JavaScript代码(比如```document.getElementById(id)```)，但是你没有权限对CasperJS的辅助函数这么做。
   *对CasperJS设置监听器，如果你想打印出你所评价的页面触发时的信息和警告。

