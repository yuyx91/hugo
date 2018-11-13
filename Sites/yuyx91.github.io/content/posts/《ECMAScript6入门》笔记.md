---
title: "《ECMAScript6入门》笔记"
date: 2016-11-25T11:53:13+08:00
tags: "ES6"
---
最近学习了vue2.0，顺便记一下ES6的语法  
<!--more-->  
### let和const命令
#### let命令
##### 基本用法
ES6新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的代码块内有效。for循环的计数器，就很适合使用`let`命令。
```js
    for (let i = 0 ;i < 10; i ++){}
    console.log(i);//ReferenceError: i is not defined
```
上面的代码中，计时器`i`只在`for`循环内有效，在循环外引用就会报错。
##### 不存在变量提升
`let`不像`var`那样会发生变量提升的现象。所以，变量一定要在声明之后使用，否则报错。
```js
    console.log(foo);//undefined
    console.log(bar);//ReferenceError

    var foo = 2;
    let bar = 2;
```
上面代码中，变量`foo`用`var`命令声明，会发生变量提升，即脚本开始运行时，变量`foo`已经存在了，但是没有值，所以会输出`undefined`。变量`bar`用`let`命令声明，不会发生变量提升。这表示在声明它之前，变量`bar`是不存在的，这时如果用到它，就会抛出一个错误。
##### 暂时性死区
只要块级作用域内存在`let`命令，它所声明的变量就绑定这个区域，不再受外部的影响。在使用`let`命令声明变量之前，该变量都是不可用的。在语法上称为“暂时性死区”。
```js
    if (true) {
        //死区开始
        tmp = 'abc'; // ReferenceError
        console.log(tmp); // ReferenceError

        let tmp; // 死区结束
        console.log(tmp); // undefined

        tmp = 123;
        console.log(tmp); // 123
    }
```
##### 不允许重复声明
`let`不允许在相同作用域内，重复声明同一个变量。
```js
    function func(arg){
        let arg;//报错
    }

    function func(arg){
        {
            let arg;//不报错
        }
    }
```
因此，不能在函数内部重新声明参数。
#### 块级作用域
##### 为什么需要块级作用域
ES5只有全局作用域和函数作用域，没有块级作用域。`let`实际上为Javascript新增了块级作用域。
```js
    function f1(){
        let n = 5;
        if(true){
            let n = 10;
        }
        console.log(n);//5
    }
```
块级作用域的出现，实际上使得立即执行匿名函数（IIFE）不再必要了。
```js
    // IIFE 写法
    (function(){
        var tem = ...;
        ...
        }());
    // 块级作用域写法
    {
        let tmp = ...;
        ...
    }    
```
##### 块级作用域与函数声明
ES5中规定，函数只能在顶层作用域和函数作用域中声明，不能在块级作用域中声明。
```js
    //情况一
    if(true){
        function f(){}
    }

    //情况二
    try{
        function f() {}
    }catch(e){}
```
上面代码的两种函数声明，在ES5中都是非法的。  
ES6引入了块级作用域，明确允许在块级作用域中声明函数。并且ES6规定，块级作用域中，函数声明语句的行为类似于`let`,在块级作用域之外不可引用。**考虑到环境导致的行为差异过大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。**
```js
    //函数声明语句
    {
        let a = "secret";
        function f(){
            return a;
        }
    }

    //函数表达式
    {
        let a = "secret";
        let f = function (){
            return a;
        };
    }
```
另外，还有一个需要注意的地方。ES6的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错。
#### const命令
##### `const`声明一个只读的变量。一旦声明，常量的值就不能改变，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。
```js
    const PI = 3.1415;
    PI = 3;
    //TypeError: Assignment to constant variable.
    const foo;
    //SyntaxError: Missing initializer in const declaration
```
##### `const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。
```js
    if(true) {
        const MAX = 5;
    }
    MAX //Uncaught ReferenceError: MAX is not defined
```
##### `const`声明的常量，也和`let`一样不可重复声明。
```js
    var message = "hello!";
    let age = 25;
    //以下两行都会报错
    const message = "Goodbye!";
    const age = 30;
```
对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。
```js
    const foo = {};
    foo.prop = 123;

    foo.prop;//123

    foo = {};// TypeError: "foo" is read-only
```
如果真的想将对象冻结，应该使用`Object.freeze`方法
```js
    const foo = Object.freeze({});
    //严格模式时，该行会报错
    foo.prop = 123;
```
ES5只有两种声明变量的方法：`var`命令和`function`命令。ES6除了添加`let`和`const`命令，还有`import`命令和`class`命令。所以，ES6一共有6种声明变量的方法。
#### 顶层对象的属性
顶层对象，在浏览器环境指的是`window`对象，在Node指的是`global`对象。ES5之中，顶层对象的属性与全局变量是等价的。
```js
    var a = 1;
    //如果在Node的REPL环境，可以写成global.a
    //或者采用通用的方法，写成this.a
    window.a//1

    let b = 1;
    window.b//undefined
```
从ES6开始，全局变量将逐步与顶层对象的属性脱钩。
#### 顶层对象
ES5的顶层对象，本身也是一个问题，因为它在各个实现里面是不统一的。  
- 浏览器里面，顶层对象是`window`，但Node和Web Worker没有`window`。
- 浏览器和Web Worker里面，`self`也指向顶层对象，但是Node没有`self`。
- Node里面，顶层对象是`global`，但其他环境都不支持。
