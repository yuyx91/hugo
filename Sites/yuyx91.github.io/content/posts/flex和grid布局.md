---
title: flex和grid布局
date: 2017-02-05 08:56:42
tags: "css"
---
传统的布局解决方案，基于盒式模型，它对应那些特殊布局非常不方便，比如垂直居中。2009年，W3C提出了一种新的方案--Flex布局，可以简便、完整、响应式地实现各种页面布局。
<!--more-->
# flex基础语法
## 定义在容器的属性
### display
`display`属性定义了一个弹性盒子容器，此时，它的子元素进入flex文档流，成为伸缩项目。
```css
    .box{
        display:flex;
        }
```
定义行内容器的flex布局：
```css
    .box{
        display:inline-flex;
        }
```
对于Safari浏览器，需要加上`webkit`前缀
```css
    .box{
        display:-webkit-flex;
        display:flex;
        }
```
**此外，请注意以下两点：**
+ CSS的columns在伸缩容器上没有效果
+ float、clear和verticle-align在伸缩项目上没有效果。