---
title: jquery实现导航栏滑动底边
date: 2016-11-16 17:14:08
tags: "demo"
---
公司的新网站，加入一些比较炫的效果，利用jquery实现滑动跟随导航栏底边。
<!--more-->  
废话不多说，前端代码：
```html
    <ul class="nav">
        <li class="active"><a href="">首页</a></li>
        <li><a href="">新闻资讯</a></li>
        <li><a href="">关于龙创</a></li>
        <li><a href="">方案与产品</a></li>
        <li><a href="">典型案例</a></li>
        <li><a href="">技术服务</a></li>
        <li><a href="">联系我们</a></li>
    </ul>
    <div class="s-line">
        <div class="container">
            <div class="slide-bottom"></div>
         </div>
    </div>
```
jq代码（匆匆写的，以后再整理）：  
```js
    $(function(){
        var i,l=0;
        $(".nav li").mouseover(function(){
            i=$(this).index();
            l=465+i*105;
            $(".slide-bottom").stop().animate({"left":l+"px"},"fast")
        })
        $(".nav").mouseleave(function(){
            i=$(this).children(".active").index();
            l=465+i*105;
            $(".slide-bottom").animate({"left":l+"px"},"fast")
        })
    })
```
很小的组件，使用jquery实现不够轻量化，也没有办法，就不贴demo了，这里是效果图:<img src="/images/gifs/GIF1.gif" title="鼠标跟随滑动底边" alt="图片链接失效请联系yuyx91@qq.com">
