---
title: webpack打包整合angular1项目实践
date: 2017-04-08 08:58:36
tags: "angular"
---
掌握了模块化思想,angular1的学习曲线就不会那么陡峭,一般的angular项目多使用requireJS整合代码,requireJS使用AMD规范,虽然也是模块化引入,但是没有做到打包代码,运行时是逐个加载js,而且需要额外引入requireJS,
webpack模块化采用commomJS规范,和nodeJS语法一样.而且不依赖其他js文件,打包时合并为一个文件,减少HTTP请求.下面来具体讲一讲如何使用webpack一步步构建angular项目:
<!--more-->
## 1.目录结构:
```
    + dist
        - bundle.js
    + lib
        + angular
        + angular-bootstrap
        + angular-ui-router
    + node_modules
    + src
        + css
        + fonts
        + img
        + js
            + controllers
            + services
            - routing.js
            - app.js
    + views
    - .gitignore
    - webpack.config.js
    - index.html
```
- dist : webpack打包输出的目录,生成bundle.js
- lib : angular及angular插件目录
- node_modules : npm 安装目录
- src : 项目资源文件目录
- views : 视图文件目录
- webpack.config.js :　webpack配置文件



## 2.webpack安装及配置
### 安装webpack
使用全局安装:
```
    npm install -g webpack
```
或者在本项目目录下安装
### 配置文件
在项目目录下新建webpack.config.js:
```js
    var webpack=require('webpack');
    module.exports = {
        entry: './src/js/app.js',//入口文件
        output: {//输出文件
            path: __dirname+"/dist",
            filename: 'bundle.js'
        },
        module: {
            loaders: [//加载器
                {test: /\.html$/, loader: 'raw'},
                {test: /\.css$/, loader: 'style!css'},
                {test: /\.scss$/, loader: 'style!css!sass'},
                {test: /\.(png|jpg|ttf)$/, loader: 'url?limit=8192'}
            ]
        },
        //压缩插件
        plugins:[
            new webpack.optimize.UglifyJsPlugin({
                sourceMap: false,
                mangle: false,
                compress: {
                    warnings: false
                }
            })
        ]
    };
```
### 运行打包
打包项目运行命令行:
```
    webpack
```
压缩代码:
```
    webpack -p
```
**需要注意的是:webpack压缩混淆时,会导致angular注入失败,所以添加UglifyJsPlugin压缩插件,开发环境可以先把插件注释,发布时再压缩**
添加监测变化
```
    webpack --watch
```
## 3.index页面入口
使用webpack打包时只是打包controller等文件,angular和第三方插件不进行打包,方便调试也减少一些不必要的bug
```html
    <!doctype html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <link href="src/css/style.css" rel="stylesheet"/>
        <script src=lib/angular/angular.min.js></script>
        <script src=lib/angular/angular-ui-router.min.js></script>
        <script src=lib/angular/angular-file-upload.min.js></script>
        <script src="lib/angular-bootstrap/ui-bootstrap-tpls.min.js"></script>
        <script src=dist/bundle.js></script>
    </head>
    <body ng-app="app">
        <div class="header">
            <div class="navbar">
                <ul class="nav navbar-nav">
                    <li ui-sref-active="active"><a ui-sref="home">首页</a></li>
                    <li ui-sref-active="active"><a ui-sref="dyna">最新动态</a></li>
                    <!--......-->
                </ul>
               </div>
        </div>
        <div ui-view></div>
        <div class="footer"></div>
    </div>
    </body>
    </html>
```
## 4.app.js配置
app.js 是angular主要配置文件,也是webpack打包入口文件:
```js
    var app = angular.module('app', ['ui.router','angularFileUpload','ui.bootstrap']);
    //全局常量
    app.constant('commonConfig',{
        $webroot : 'http://192.168.0.180:8805/'
    });
    //路由
    app.config(require('./routing'));
    //服务,公共方法
    app.service('Common',require('./service/Common'));
    app.service('httpfactory',require('./service/httpfactory'));
    //自定义指令
    app.directive('loginmodal',require('./service/loginmodal'));
```
使用require获取路由及服务等配置
## 5.路由routing.js等写法
使用module.exports将文件暴露出去:
```js
    module.exports = function ($stateProvider, $urlRouterProvider) {
        $stateProvider
            .state('home', {
                url: '/home',
                templateUrl: 'views/home.html',
                controller: require("./controller/HomeCtrl.js")
            })
            .state('dyna',{
                url:'/dyna',
                templateUrl:'views/dyna.html',

            });
        $urlRouterProvider.when("", "/home");
    };
```