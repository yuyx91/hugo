---
title: 原生Js创建ajax发送表单
date: 2016-11-16 09:47:31
tags: "ajax"
---
一直使用jquery的ajax和后台交互，对ajax的原理不是很了解，而且轻量化的手机端不适合使用jquery操作，研究一下原生的ajax有助于对http的深入理解。 

<!--more-->
以下代码例子只是我自己用node EXPRESS做后台跑通了，具体实践还要结合情况。  
先贴一下前端代码：
```js
        function login(){
            var n = document.getElementById("username").value，
                p = document.getElementById("password").value,
                //转换成JSON格式，否则默认发送的是对象，node后台会解析错误
                data = JSON.stringify({
                    uname: n,
                    upwd: p
                });

            function ajax() {
                var xhr = new XMLHttpRequest(),
                    url = "/login";
                xhr.open("post", url, true);
                //http Header 改成了json，否则后台报错，以后需要了解
                xhr.setRequestHeader("CONTENT-TYPE", "application/json");
                //接受状态的默认写法
                xhr.onreadystatechange = function() {
                    if(xhr.readyState == 4 && xhr.status == 200) {
                        //res是我后台返回的数据，需要转成js对象
                        var res = JSON.parse(xhr.responseText);
                        if(res.status == "success") {
                            alert(res.data);
                            location.href = "/"
                        }
                    }
                }
                xhr.send(data);
            }
            ajax();
        }
```
接下来是node后台代码：  
```js
    app.post('/login', function(req, res) {
        var models = require('../models'),
            User = models.User,
            uname = req.body.uname,//获取表单数据
            upwd = req.body.upwd;
        User.findOne({//数据库查询
            "name": uname
        }, function(err, doc) {
            if(doc) {
                if(doc.password == upwd) {
                    req.session.username=uname;
                    return res.send({//服务器返回json
                        status: "success",
                        data:"欢迎用户"+req.session.username
                    });

                } else {
                    return res.send({
                        status: "pwderr",
                        data: "密码错误！"
                    });
                }

            } else {
                res.send({
                    status: "usererr",
                    data: "不存在的用户名!"
                })
            }
        });
    });
```
**http POST发送数据要标明头部json格式，否则发送的是js对象，服务器解析错误，服务器返回的json字符串还要转成js对象，这一点要注意。**