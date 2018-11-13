---
title: Javascript常用API合集
date: 2017-03-14 10:02:10
tags: "api"
---
收集Javascript常用API合集，来源web前端开发。
<!--more-->
## 节点
### Node节点
#### 节点属性
```js
    Node.nodeName   //返回节点名称，只读
    Node.nodeType   //返回节点类型的常数值，只读
    Node.nodeValue  //返回 Text 或 Comment 节点的文本值，只读
    Node.textContent  //返回当前节点和它的所有后代节点的文本内容，可读写
    Node.baseURI    //返回当前网页的绝对路径

    Node.ownerDocument  //返回当前节点所在的顶层文档对象，即 document
    Node.nextSibling  //返回紧跟在当前节点后面的第一个兄弟节点
    Node.previousSibling  //返回当前节点前面的、距离最近的一个兄弟节点
    Node.parentNode   //返回当前节点的父节点
    Node.parentElement  //返回当前节点的父 Element 节点
    Node.childNodes   //返回当前节点的所有子节点
    Node.firstChild  //返回当前节点的第一个子节点
    Node.lastChild   //返回当前节点的最后一个子节点

    //parentNode 接口
    Node.children  //返回指定节点的所有 Element 子节点
    Node.firstElementChild  //返回当前节点的第一个 Element 子节点
    Node.lastElementChild   //返回当前节点的最后一个 Element 子节点
    Node.childElementCount  //返回当前节点所有 Element 子节点的数目。
```
#### 操作
```js
    Node.appendChild(node)   //向节点添加最后一个子节点
    Node.hasChildNodes()   //返回布尔值，表示当前节点是否有子节点
    Node.cloneNode(true);  // 默认为 false(克隆节点), true(克隆节点及其属性，以及后代)
    Node.insertBefore(newNode,oldNode)  // 在指定子节点之前插入新的子节点
    Node.removeChild(node)   //删除节点，在要删除节点的父节点上操作
    Node.replaceChild(newChild,oldChild)  //替换节点
    Node.contains(node)  //返回一个布尔值，表示参数节点是否为当前节点的后 代节点。
    Node.compareDocumentPosition(node)   //返回一个 7 个比特位的二进制值， 表示参数节点和当前节点的关系
    Node.isEqualNode(noe)  //返回布尔值，用于检查两个节点是否相等。所谓相 等的节点，指的是两个节点的类型相同、属性相同、子节点相同。
    Node.normalize()   //用于清理当前节点内部的所有 Text 节点。它会去除空 的文本节点，并且将毗邻的文本节点合并成一个。
```
### Document节点
### Element节点
## css操作
### class操作
### style操作
## 对象
### Object对象
### Array对象
### Number对象
### String对象
### Math对象
### Json对象
### console对象