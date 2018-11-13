---
title: Javascript基于原型的面向对象
date: 2016-11-18 17:18:03
tags:
---
和java等基于类（class-base）的面向对象的编程语言不同，Javascript没有类的概念，而是基于原型（prototype-base）实现的面向对象。虽然ES6已结引入了类的概念来做模板，通过关键字“class”可以定义类，但ES6的这种写法可以理解为一种语法糖。  
<!--more-->  
如果要理解基于原型实现面向对象的思想，那么就需要理解Javascript中的三个重要概念：构造函数（constructor）、原型（prototype）、原型链（prototype chain）。  
## Javascript对象结构图  
先来看一看javascript对象的结构图：  
<img src="/images/img001.png" title="javascript对象结构图" alt="图片链接失效请联系yuyx91@qq.com">
## 构造函数（constructor）和原型（prototype）
与基于类的面向对象不同，Javascript没有类（class）的概念，取而代之的是构造函数（constructor）。构造函数是用来初始化对象的，每个构造函数都有一个不可枚举的属性，这就是原型（prototype），Javascript就是使用它来实现基于原型的继承及属性共享。  
同时，每个原型（prototype）都包含了不可枚举的constructor属性，这个属性始终指向构造函数。 不论是构造函数还是原型，它们都是对象。  
**Javascript的数据类型包括两类：5种原始类型和对象类型，函数（function）是一种特殊的对象。** 
```js
    function Foo(){};
    console.log(Foo.prototype.constructor === Foo);//true
```
## 原型链（prototype chain）
### 使用new实例化的原型
每个被new实例化的对象都会包含一个`__proto__`属性，它是对构造函数`prototype`的引用。
```js
    function Foo(){};
    var foo = new Foo();
    console.log(foo.__proto__ === Foo.prototype);//true
```
通过上面这段代码，既可证明proto属性是构造函数“prototype”属性的引用
```js
    function Foo(){};
    console.log(Foo.prototype.__proto__ === Object.prototype);//true
```
上面返回true的原因是Foo.prototype是Object预创建的一个对象，是Object创建的一个实例，所以Foo.prototype.__proto__是Object.prototype的引用。  
我们可以看一下原型链的脉络。  
<img src="/images/img002.png" title="原型链的脉络" alt="图片链接失效请联系yuyx91@qq.com">
### 函数（function）对象的原型
在javascript中，函数是一种特殊的对象，所有函数都是构造函数`Function`的实例。
```js
    function Foo(){};
    console.log(Foo.__proto__ === Object.prototype);//false
    console.log(Foo.__proto__ === Function.prototype);//true
```
从上面可以看出，函数Foo.__proto__指向到Function.prototype，说明函数`Foo`是`Function`的一个实例。
```js
    function Foo(){};
    console.log(Foo.__proto__ === Function.prototype);//true
    console.log(Foo.prototype.__proto__ === Object.prototype);//true
```
Foo.prototype是Object预定义的对象，构造方法函数为Object，所以`__proto__`指向`Object.prototype`  
<img src="/images/img003.png" title="原型链的脉络" alt="图片链接失效请联系yuyx91@qq.com">  
从上面的图可以看出，Object、Function、Array等这些函数，它们的构造函数都是Function的实例。
## 基于原型链的面向对象继承
前面讲了很多关于原型、原型链的内容，都是为最后的面向对象实现做铺垫，如果不明白原型链的实现机制，基于原型的对象继承将会很难理解。
### 封装
先使用构造函数声明一个类，在构造函数中给this添加本地属性，并实例化一个对象，这种方式可以为对象声明一个公共的本地属性：
```js
    function Animal(name){
        this.name = name;
        this.sleep = function(){
            console.log(this.name + 'sleep');
        };
    }
    var a = new Animal('cat');
    a.sleep();//cat sleep
```
基于原型链封装对象和继承。
```js
    function Animal(){
        console.log('animal init');
    }

    Animal.prototype.eat = function(){
        console.log('animal eat');
    }

    function Cat(){
        console.log('cat init');
    }

    function Empty(){};
    Empty.prototype = Animal.prototype;
    Cat.prototype = new Empty();
    Cat.constructor = Cat;
    var cat = new Cat();
    cat.eat();

    //output: cat init  animal eat
```
运行上面的例子，输出cat init和animal eat，说明cat继承了Animal.prototype.eat的方法。  
我们来分析一下代码。 
- Animal的prototype中定义了eat方法。
- 将Empty.prototype指向Animal.prototype，所以Empty.prototype中也存在eat方法。
- Cat.prototype == new Empty()，所以Cat.prototype.__proto__ === Animal.__proto__。
- 重新为Cat指定constructor为Cat，否则Cat不存在constructor。  
这样我们就用原型链的方法完成了继承  
参考：<a href="https://github.com/maxzhang/maxzhang.github.com/issues/5" title="基于原型的Javascript面向对象编程" target="_blank">基于原型的Javascript面向对象编程</a>    
