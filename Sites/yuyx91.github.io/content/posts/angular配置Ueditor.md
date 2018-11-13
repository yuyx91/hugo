---
title: angular配置Ueditor
date: 2017-03-10 10:38:50
tags: "angular"
---
angular中使用Ueditor注意事项：
把Ueditor文件夹复制到资源目录中，index.html中引入ueditor.config.js，ueditor.min.js;
不能直接在html文件中实例化Ueditor，需要在controller中进行。
```js
    //先销毁上一个editor实例；
    UE.delEditor("editor");
    //实例化
        var ue = UE.getEditor('editor');
        if($scope.notice.value){
            $scope.title= $scope.notice.value.Title;
            ue.ready(function () {
                // 带入编辑内容内容
                // editor准备好之后才可以使用
                ue.setContent($scope.notice.value.Content);
            });
        }
```