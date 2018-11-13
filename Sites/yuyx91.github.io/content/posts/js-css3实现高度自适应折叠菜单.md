---
title: js+css3实现高度自适应折叠菜单
date: 2016-12-29 14:04:18
tags: "demo"
---
虽然使用jquery实现的折叠菜单非常简便，直接使用slideUp()和slideDown()方法，但是手机端使用jquery太不方便，可以考虑使用原生js+css3实现。  
<!--more-->
#### 实现思路
- 每个内容块设置`max-height`和`transition`，`max-height`可以设置适当的高度，不需要很大，否则会回弹的很快。
- 点击时添加`class`，`max-height`设置为0。  

#### 代码
css
```css
    dd{
        transition: max-height .6s ease;
        max-height: 200px;
        overflow: hidden;
    }
    dd.hide{
        max-height: 0;
    }
```
js
```js
    function hide (e) {
        var dd=e.nextElementSibling;
        dd.className=="hide"?dd.className="":dd.className="hide"
    }
```
#### 效果
点击查看效果<a href="http://yuyaxin.win/demo/show-hide.html" title="show-hide" target="_blank">DEMO:show-hide</a>