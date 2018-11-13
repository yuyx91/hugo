---
title: rem布局实践
date: 2016-11-17 11:23:19
tags: "css"
---
手机端和微信端做的项目不少，一直是`@media`媒体查询和`vw`混用，蒙混过关，但是也会有很多兼容性的问题，并不能做到完全响应。所以最近关注一下比较流行的rem布局。  
<!--more-->
一般比较简单的`rem`布局是这样的，设置html的`font-size:100px`，组件的单位相对于`font-size`进行调整，也就是`rem`。然后根据设备的宽度，使用js动态调整`font-size`，以达到响应式的布局。  
简单的例子：
```js
    (function(){
        var f = document.documentElement.clientWidth; 
        if(f > = 640){
            document.documentElement.style.fontSize = '100px';
        }esle{
            document.documentElement.style.fontSize = 100 * (f/640) + 'px';
        }
    })()
```
这种布局简单易懂，小屏幕下组件和字体大小同比例缩小，使得布局和大屏幕一致，但是这样效果并不理想，因为小屏幕下字体应该一致结果反而变小了，影响阅读效果，这也是我的疑惑。  
参考<a href="http://www.jianshu.com/p/b00cd3506782#" target="_blank">手机端页面自适应解决方案—rem布局</a>  
完整代码：
```js
    (function (doc, win) {
        var docEl = doc.documentElement,
            //横屏效果
            resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
            recalc = function () {
                var clientWidth = docEl.clientWidth;
                if (!clientWidth) return;
                if(clientWidth>=640){
                    docEl.style.fontSize = '100px';
                }else{
                    docEl.style.fontSize = 100 * (clientWidth / 640) + 'px';
                }
            };

        if (!doc.addEventListener) return;
        win.addEventListener(resizeEvt, recalc, false);
        doc.addEventListener('DOMContentLoaded', recalc, false);
    })(document, window);
```
以上代码直接放在js中，并设置`viewport`。  
今天又看到作者的进阶版方案：具体<a href="http://www.jianshu.com/p/985d26b40199" target="_blank">手机端页面自适应解决方案—rem布局（进阶版）</a>  
该方案使用相当简单，把这段 原生JS 放到 HTML 的 head 标签中即可（注:不要手动设置`viewport`，该方案自动帮你设置）  
这里也贴上源码：  
```js
    ! function(e) {
            function t(a) {
                if(i[a]) return i[a].exports;
                var n = i[a] = {
                    exports: {},
                    id: a,
                    loaded: !1
                };
                return e[a].call(n.exports, n, n.exports, t), n.loaded = !0, n.exports
            }
            var i = {};
            return t.m = e, t.c = i, t.p = "", t(0)
        }([function(e, t) {
            "use strict";
            Object.defineProperty(t, "__esModule", {
                value: !0
            });
            var i = window;
            t["default"] = i.flex = function(e, t) {
                var a = e || 100,
                    n = t || 1,
                    r = i.document,
                    o = navigator.userAgent,
                    d = o.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i),
                    l = o.match(/U3\/((\d+|\.){5,})/i),
                    c = l && parseInt(l[1].split(".").join(""), 10) >= 80,
                    p = navigator.appVersion.match(/(iphone|ipad|ipod)/gi),
                    s = i.devicePixelRatio || 1;
                p || d && d[1] > 534 || c || (s = 1);
                var u = 1 / s,
                    m = r.querySelector('meta[name="viewport"]');
                m || (m = r.createElement("meta"), m.setAttribute("name", "viewport"), r.head.appendChild(m)), m.setAttribute("content", "width=device-width,user-scalable=no,initial-scale=" + u + ",maximum-scale=" + u + ",minimum-scale=" + u), r.documentElement.style.fontSize = a / 2 * s * n + "px"
            }, e.exports = t["default"]
        }]);
        flex(100, 1);
```
这段代码好像是混淆过的，具体原理就是根据设备的dpr动态调整，达到高清的目的，大屏幕和小屏幕都能实现很好的效果。