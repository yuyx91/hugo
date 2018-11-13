---
title: 学习笔记：GET和POST请求的区别
date: 2016-11-14 09:53:19
tags: "note"
---
<table><tr><th>GET方式</th><th>POST方式</th></tr><tr><td>同一地址不重复执行</td><td>每次都执行</td></tr><tr><td>可被缓存</td><td>不可被缓存</td></tr><tr><td>把参数数据添加到提交表单的action属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到</td><td>通过HTTP POST机制，将表单内各个字段与内容放置在HTML HEADER内一起传送到action属性所指的URL地址，用户看不到这个过程</td></tr><tr><td>服务器端用Request.QueryString获取变量的值</td><td>服务器端用Request.Form获取提交是数据</td></tr><tr><td>传送的数据量较小，不能大于2KB</td><td>传送的数据量较大，一般被默认为不受限制，但理论上，因服务器的不同而异</td></tr><tr><td>安全性非常低</td><td>安全性较高</td></tr><tr><td>method为get时，action后面的参数列表会被忽视</td><td>method为post时，action后面的参数列表不可忽视</td></tr><tr><td>它将数据添加到URL中，通过这种方式传递到服务器，通常利用问号<code>?</code>代表URL地址的结尾与数据参数的开端，后面的参数每一个数据参数以“键=值”的形式出现，参数之间用<code>&</code>区分</td><td>数据是放在HTTP主体中的，其组织方式不止一种，有<code>&</code>连接方式，也有分隔符方式，可隐藏参数，传递大批数据比较方便</td></tr></table>
