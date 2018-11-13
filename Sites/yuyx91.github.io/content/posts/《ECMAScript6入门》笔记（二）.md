---
title: 《ECMAScript6入门》笔记（二）
date: 2016-12-05 09:27:22
tags: "ES6"
---
ES6还包括变量的扩展、数组的扩展、对象的扩展等。暂时略过，只记一下比较重要的。
<!--more-->
### Promise对象
#### 1.Promise的含义
所谓`Promise`，简单的来说就是一个容器，储存着某个未来才会结束的事件结果。从语法上来说，`Promise`是一个对象，通过它可以获取异步操作的消息。`Promise`提供统一的API，各种异步操作都可以用同样的方法进行处理。  
`Promise`对象有两个特点：  

- 对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`Pending`（进行中）、`Resolved`（已完成）和`Rejected`（已失败），任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的意思就是“承诺”，表示其他手段无法改变。  

- 一旦状态改变，就不会再变，任何时候都可以获取这个结果。`Promise`对象的状态改变，只有两种可能：从`Pending`变为`Resolved`和从`Pending`变为`Rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
#### 2.基本用法
ES6规定，Promise对象是一个构造函数，用来生成Promise实例。  
```js
    var promise = new Promise(function(resolve,reject){
        //...
        if(/*异步操作成功*/){
            resolve(value);
        }else{
            reject(error);
        }
    });
```
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由JavaScript引擎提供，不用自己部署。  
Promise实例生成后，可以用`then`方法分别指定`Resolved`状态和`Reject`状态的回调函数。
```js
    promise.then(function(value){
        //success;
        },function(error){
        //failure;    
        });
```
下面是一个用Promise对象实现Ajax操作的例子。
```js
    var getJSON = function(url) {
        var promise = new Promise(function(resolve, reject){
        var client = new XMLHttpRequest();
        client.open("GET", url);
        client.onreadystatechange = handler;
        client.responseType = "json";
        client.setRequestHeader("Accept", "application/json");
        client.send();

        function handler() {
          if (this.readyState !== 4) {
            return;
          }
          if (this.status === 200) {
            resolve(this.response);
          } else {
            reject(new Error(this.statusText));
          }
        };
    });

      return promise;
    };

    getJSON("/posts.json").then(function(json) {
      console.log('Contents: ' + json);
    }, function(error) {
      console.error('出错了', error);
    });
```
### 异步操作和Async函数
ES6诞生之前，异步编程的方法，大概有下面四种：  
- 回调函数
- 事件监听
- 发布/订阅
- Promise 函数  
ES6将Javascript异步编程带入了一个全新的阶段，ES7的`Async`函数更是提出了异步编程的终极解决方案。
---
#### 1.基本概念
（待补充）
### Class
### Module

