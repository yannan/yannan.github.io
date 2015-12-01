---
layout: post
title: Flexbox布局学习
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2014-12-24
tags: [sample post]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

##遇到的问题
###自适应布局
  在用传统布局做自适应网页时时常会遇到这样的问题   
 <iframe height='268' scrolling='no' src='//codepen.io/yannan/embed/PqgYyX/?height=268&theme-id=17785&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='http://codepen.io/yannan/pen/PqgYyX/'>PqgYyX</a> by yannan (<a href='http://codepen.io/yannan'>@yannan</a>) on <a href='http://codepen.io'>CodePen</a>.
</iframe>   
当设置了边框或者外边距之后以后，4个25%宽度的框就无法在一行排列，当然我们也可以通过减小宽度来解决这个问题，但如果要排列的过多的话计算起来就过于麻烦。这种情况下我们就考虑采用flexbox来进行布局。
##如何使用flexbox
只需在支持flexbox属性的浏览器中这样设置   

    <div class="container">  
      
    </div> 
```
    .container {  
      display:flex;/*将父级元素设置为flex*/  
    }
```
当然防止不必要的换行还有别的方法：  
    
      .container ul li{  
       float:left;   
       list-style:none;   
       background-color:red;  
       width:25%;  
       height:200px;  
       border:solid 1px #ccc;  
       box-sizing:border-box;  
       -moz-box-sizing:border-box;  
       -webkit-box-sizing:border-box;  
       }  

利用box-sizing属性。