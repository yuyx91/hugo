---
title: 关于Node中的require和import
date: 2016-12-23 09:14:57
tags:
---
一直疑惑为什么vue2.0都支持ES6语法，使用import处理模块，为什么Node7.0还不支持import？其实ES6的module模块的js引擎还在开发中，目前还没有浏览器支持import。具体可以看[chrome浏览器modules开发进度](https://www.chromestatus.com/features/5365692190687232"chrome浏览器modules开发进度")。而Node是使用chromeV8引擎，所以也不支持import，很多ES6语法最终都要通过转码。
<!--more-->
#### require时代的模块
node以及npm中的module遵循CommonJS规范，requirejs遵循AMD，seajs遵循CMD，虽各有不同，但是都是为了保持统一的代码风格。
```js
    // a.js
    //----------------- node --------------
    module.exports = {
        a : function() {},
        b : 'xxx'
        };

   //--------------- AMD or CMD ---------------
   define(function(require,exports,module){
        module.exports = {
            a :function(){},
            b : 'xxx'
        };
    });
```
可以看出，为了保持统一的代码风格，除了在浏览器端的模块中要使用一个define函数来提供的闭包以外，其他代码可以完全一致。
```js
    //b.js
    //--------------node--------------
    var m = require('./a');
    m.a();

    //-------------AMD or CMD --------------
    define(function(require,exprots,module){
        var m = require('./a');
        m.a()
        });
```
在使用上也非常相似。
#### ES6中的module
ES6发布的module并没有直接采用CommonJS，也没有采用requirejs。
##### export导出模块接口
```js
    export { name1, name2, …, nameN };
    export { variable1 as name1, variable2 as name2, …, nameN };
    export let name1, name2, …, nameN; // also var
    export let name1 = …, name2 = …, …, nameN; // also var, const

    export default expression;
    export default function (…) { … } // also class, function*
    export default function name1(…) { … } // also class, function*
    export { name1 as default, … };

    export * from …;
    export { name1, name2, …, nameN } from …;
    export { import1 as name1, import2 as name2, …, nameN } from …;
```
export在导出接口的时候，必须和模块内部的变量具有一一对应的关系。建议在模块的末尾用一个export导出所有的接口，例如：
```js
    export {fun as default,a,b,c};
```
##### import导入模块
import的语法和require不同，而且import必须放在文件的最开始，且前面不允许有其他逻辑代码，这与其他所有编程语言风格一致。
```js
    import $ from 'jquery';
    import * as _ from '_';
    import {a,b,c} from './a';
    import {default as alias, a as a_a, b, c} from './a';
```
##### as 关键字 
as简单的来说就是取一个别名。export中可以用，import中也可以用：
```js
    //a.js
    var a = function(){};
    exprot {a as fun};

    //b.js
    import {fun as a} from './a';
    a();
```
##### default 关键字
在export的时候，可能会用到default，其实它是别名的语法糖：
```js
    //d.js
    export default function(){}
     //等效于：
    function a(){};
    export {a as default};
```
在import中也可以这样用：
```js
    import a from './d';
    //等效于：
    import {default as a} from './d'
```
这种语法糖的好处就是，import的时候，可以省去花括号{}。
#### 该用require还是import（区别）？
require的使用非常简单，它相当于module.exports的传送门，module.exports后面的内容是什么，require的结果就是什么。而且require理论上可以运行在代码的任何地方，甚至不需要赋值给某个变量之后再使用：
```js
    require('./a')();//a模块是一个函数，立即执行a模块
    var data = require('./a').data;//a模块导出的是一个对象
    var a = require('./a')[0];//a模块导出的是一个数组
```
但是import则不同，它是编译时的（require是运行时的），它必须放在文件开头，而且使用格式也是确定的。它不会将整个模块运行后赋值给某个变量，而是选择import的接口进行编译，这样在性能上比require好很多。  
<table><tr><th>命令</th><th>规范</th><th>加载方式</th><th>特点</th></tr><tr><td>require</td><td>CommonJS/AMD</td><td>运行时加载</td><td>社区方案，提供了服务器/浏览器的模块加载方案，非语言层面的标准。只能在运行时确定模块的依赖关系及输入/输出的变量，无法进行静态优化。</td></tr><tr><td>import</td><td>ESMAScript6+</td><td>编译时加载</td><td>语言规格层面支持模块功能。支持编译时静态分析，便于JS引入宏和类型检验。动态绑定。</td></tr></table>