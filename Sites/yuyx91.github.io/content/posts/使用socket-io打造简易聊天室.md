---
title: 使用socket.io打造简易聊天室
date: 2017-05-17 16:51:31
tags: 'socket.io'
---
要理解socket.io ，不得不谈谈websocket;在html5之前，因为http协议是无状态的，要实现浏览器与服务器的实时通讯，如果不使用 flash、applet 等浏览器插件的话，就需要定期轮询服务器来获取信息。这造成了一定的延迟和大量的网络通讯。随着HTML5 的出现，这一情况有望彻底改观，它就是 WebSocket 。
socket.io是对websocket的封装,此外它还把轮询（Polling）机制以及其它的实时通信方式封装成了通用的接口,下面是按照官网例子实现的简易聊天室:
<!--more-->
前台需要引入socket.io的js文件:
```html
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.0.1/socket.io.js"></script>
    <script src="https://cdn.staticfile.org/jquery/3.2.1/jquery.min.js"></script>
    <script>
        $(function () {
            var socket = io();
            //发送表单数据
            $('form').submit(function(){
                socket.emit('chat', $('#m').val());
                $('#m').val('');
                return false;
            });
            //监控chat
            socket.on('chat',function(msg){
                $('#messages').append($('<li>').text(msg));
                //chrome 弹出消息框
                if (window.Notification)
                {
                    if (window.Notification.permission == "granted") {
                        var notification = new Notification('来自聊天室', {
                            body: msg,
                            icon: ""
                        });
                    } else {
                        window.Notification.requestPermission();
                    }
                }
            });
        });
    </script>
```
后台采用nodeJs:
```javascript
    var app = require('express')();
    var http = require('http').createServer(app);
    var io = require('socket.io')(http);

    app.get('/',function(req,res){
        res.sendFile(__dirname + '/index.html');
    });
    //开始连接
    io.on('connection',function(socket){
        console.log('一个用户连接');
        socket.on('disconnect',function(){
            console.log('用户退出')
        });
        //监听chat
        socket.on('chat',function(msg){
            console.log('消息:'+msg);
            io.emit('chat',msg);
        });
    });

    http.listen(3000,function(){
        console.log('listening on *:3000');
    })
```
 //TODO 下一步目标:分配昵称,显示对话时间