---
title: NodeJs打造静态服务器以及ngrok映射外网
date: 2017-05-17 17:39:35
tags: 'NodeJS'
---
1.在文件主目录创建app.js
```js

```
2.配置静态文件夹
```js

```
3.安装ngork
```js
    npm i ngrok --g
```  
    
    
4.启动服务器端口
```text
    node app.js
```  

5.开启ngrok暴露端口
```text
    ngrok http 3000
```  

6.打开ngrok分配的地址  
7.打开localhost://4040,查看请求详情
