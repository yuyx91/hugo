---
title: 分享几个自己常用的jquery轮播组件
date: 2016-11-22 10:49:55
tags: "jquery"
---
以下是几个自己写的jquery轮播组件，轻巧简便，配合布局可以直接拿来用。  
<!--more-->  
#### 大型banner轮播
以前习惯用bootstrap的轮播，但是需要引入不小的bootstrap.js，索性自己写了两个简单的轮播，一个是左右滑动效果，一个是淡入淡出效果（个人比较喜欢这种）。
##### 左右滑动banner轮播
设计思路是：先设置一个定位用的父盒子，很多时候banner图宽度达到1920，要保证小屏幕也能显示足够的信息。这里有两种方案：一个是图片宽度100%随着屏幕大小改变，不好控制banner的宽高；还有一种是图片居中，图片的主体内容也居中，小屏幕时显示居中的内容区，这样可以防止图片的变形，图片的内容布局就比较重要了。  
代码1：  
```html
    <style type="text/css">
        .banner{
            width: 100%;
            max-width: 1920px;
            margin: 0 auto;
            overflow: hidden;
            position: relative;
        }
        .banner ul{
            position: absolute;
            left: 0;
            width: 99999px;
        }
        .banner ul li{
            width: 1920px;
            float: left;
        }
    </style>>
    <div class="banner">
        <ul>
            <li><img src="banner1"></li>
            <li><img src="banner2"></li>
            <li><img src="banner3"></li>
        </ul>
        <div class="dots">
            <span class="active"></span>
            <span></span>
            <span></span>
        </div>
    </div>
```
接下来的jquery代码：
```js
    $(function() {
        var i = 0,
        w = $(".banner li").width(),
        l = $(".banner li").length - 1,
        Sp = $(".dot span"),
        Ul = $(".banner ul");
        function move() {
            Sp.eq(i).addClass("active").siblings("span").removeClass("active");
            Ul.animate({left: i * -w}, {duration: 500},"easeinout")
        };
        function right() {
            i >= l ? i = 0 : i++;
            move();
        };
        var timer = setInterval(right, 5000);
        Sp.hover(function() {
            clearInterval(timer);
            Ul.stop();
            i = $(this).index();
            move();
        }, function() {
            timer = setInterval(right, 5000)
            });
    });
```
##### 淡入淡出banner轮播
设计思路：将每个图片都设置相对定位，上一张图片隐藏的同时，下一张图片显示  
代码2：
```html
    <style type="text/css">
        .banner-wrap{
            width: 100%;
            height: 500px;
            overflow: hidden;
            position: relative;
        }
        .banner{
            width: 1200px;
            margin: 0 auto;
        }
        .banner ul{
            width: 1920px;
            margin-left: -360px;
            position: relative;
        }
        .banner li{
            position: absolute;
        }
        .dots{
            width: 1200px;
            text-align: center;
            position: absolute;
            bottom: 25px;   
        }
    </style>
    <div class="banner-wrap">
        <div class="banner">
            <ul>
                <li><img src="banner1"></li>
                <li style="display: none"><img src="banner2"></li>
                <li style="display: none"><img src="banner3"></li>
            </ul>
            <div class="dots">
                <span class="active"></span>
                <span></span>
                <span></span>
            </div>
        </div>
    </div>
```
jquery代码：
```js
    $(function() {
    var i = 0,
        Li = $(".banner li"),
        l = Li.length - 1,
        Sp = $(".dots span");
    function imgchange() {
        Li.eq(i).fadeIn("slow").siblings().fadeOut("slow");
        Sp.eq(i).addClass("active").siblings().removeClass("active");
    }
    function change() {
        i == l ? i = 0 : i++;
        imgchange();
    }
    var time = setInterval(change, 5000);
    Sp.hover(function() {
        clearInterval(time);
        Li.stop();
        i = $(this).index();
        imgchange();
    }, function() {
        time = setInterval(change, 5000)
    });
})
```
#### 焦点图轮播
相同的原理也有左右滑动和淡入淡出两种效果，只是宽高固定了，代码同上
#### 图片左右滚动展示
设计思路：图片每隔一段时间滚动一段距离，且可以控制滚动的方向
#### 文字或图片上下左右匀速滚动（待更新）。