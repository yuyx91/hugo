---
title: 关于Hexo及部署静态服务器网站
date: 2017-02-22 14:59:53
tags: "Hexo"
---
Hexo 是一种基于Node.js驱动，可以快速搭建并部署到GitHub Pages，Heroku等静态服务器的博客框架。优点是Node.js超快渲染，命令行一建部署，维护方便，插件和主题丰富。
<!--more-->
### 1.安装hexo
安装Hexo之前，需要安装Node.js和git，略过。npm安装hexo：
```
$ npm install -g hexo-cli
```
### 2.Hexo 文件夹介绍：
安装Hexo之后，执行以下命令：
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
新建完成后，指定文件夹的目录如下：
```
.
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
├── themes
├── package.json
└── _config.yml
```
#### _config.yml
网站的配置信息
#### package.json
程序所需的npm组件库
#### scaffolds
模板文件夹，就是新建的Markdown文件中默认填充的内容
#### source
资源文件夹，Markdown和HTML文件会被解析并放到public文件夹
#### themes
主题文件夹，Hexo会根据主题来生成静态页面
### 3.发布文章
#### hexo new
新建一篇文章，会在_post文件夹内生成Markdown文件，然后编辑文件
#### hexo generate、hexo g
生成静态文件到public文件夹，如果没有回自动生成这个文件夹
#### hexo server、hexo s
启动本地服务器，查看网站效果
#### hexo deploy、hexo d
部署网站，git提交到静态服务器
#### hexo clean
清除缓存文件和已经生成的静态文件（public）
### 4.hexo发布到coding
1. 注册<a href="https://coding.net">coding.net</a>,并且新建一个项目，项目名与用户名一致
2. 配置本地git，配置hexo远程服务器提交地址
3. 配置coding的pages服务
4. 购买域名或者直接访问user.coding.me
