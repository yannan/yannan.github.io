---
layout: post
title: 【译文】使用PhantomCSS进行CSS的回归测试(Using PhantomCSS for Regression Testing Your CSS)
description: "UI自动化测试"
modified: 2016-09-01
tags: [sample post]
image:
  feature: abstract-1.jpg
---

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



  

