---
title: webpack 学习笔记
date: 2016-11-15 09:16:50
tags: "webpack"
---
## Webpack 的特点和优势
<!--more-->
### 代码拆分
Webpack有两种组织模块依赖的方式，同步和异步。异步依赖作为分割点，形成一个新的块。在优化了依赖树后，每一个异步区块都作为一个文件被打包。
### Loader
Webpack本身只能处理原生的Javascript模块，但是loader转换器可以将各种类型的资源转换成Javascript模块。这样，任何资源都可以成为Webpack可以处理的模块。
### 智能解析
Webpack 有一个智能解析器，几乎可以处理任何第三方库，无论它们的模块形式是 CommonJS、 AMD 还是普通的 JS 文件。甚至在加载依赖的时候，允许使用动态表达式 require("./templates/" + name + ".jade")。
### 插件系统
Webpack 还有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求。
### 快速运行
Webpack 使用异步 I/O 和多级缓存提高运行效率，这使得 Webpack 能够以令人难以置信的速度快速增量编译。
## Loader介绍
- Loader 可以通过管道方式链式调用，每个 loader 可以把资源转换成任意格式并传递给下一个 loader ，但是最后一个 loader 必须返回 JavaScript。
- Loader 可以同步或异步执行。
- Loader 运行在 node.js 环境中，所以可以做任何可能的事情。
- Loader 可以接受参数，以此来传递配置项给 loader。
- Loader 可以通过文件扩展名（或正则表达式）绑定给不同类型的文件。
- Loader 可以通过 npm 发布和安装。
- 除了通过 package.json 的 main 指定，通常的模块也可以导出一个 loader 来使用。
- Loader 可以访问配置。
- 插件可以让 loader 拥有更多特性。
- Loader 可以分发出附加的任意文件。  

## Webpack 基本配置
创建配置文件`webpack.config.js`，执行`webpack`命令的时候，默认会执行这个文件
```javascript
    module.export = { 
        entry : 'app.js',//设置入口文件
        output : { 
            path : 'assets/', filename : '[name].bundle.js'
             },
        module : { loaders : [
            // 使用babel-loader解析js或者jsx模块
            { test : /\.js\.jsx$/, loader : 'babel' }, 
            //使用css-loader解析css模块
            { test : /\.css$/, loader : 'style!css' }, 
            // or another way 
            { test : /\.css$/, loader : ['style', 'css'] } 
            ] 
        } 
    };
```
## 开发环境  
```javascript
    $ webpack --progress --colors //编译时输出进度和颜色
    $ webpack --display-error-details //执行并定位错误信息
    $ webpack --config XXX.js //使用另一份配置文件
    $ webpack -p //压缩混淆脚本
    $ webpack -d //生成map映射文件，告知哪些模块最终打包到哪里了
    $ webpack --watch //监听文件变化重新编译
```
当然，使用 webpack-dev-server 开发服务是一个更好的选择。它将在 localhost:8080 启动一个 express 静态资源 web 服务器，并且会以监听模式自动运行 webpack，在浏览器打开 http://localhost:8080/ 或 http://localhost:8080/webpack-dev-server/ 可以浏览项目中的页面和编译后的资源输出，并且通过一个 socket.io 服务实时监听它们的变化并自动刷新页面。
```javascript
    #安装
    $ npm install webpakc-dev-server -g

    #运行
    $ webpack-dev-server --progress --colors
```  
