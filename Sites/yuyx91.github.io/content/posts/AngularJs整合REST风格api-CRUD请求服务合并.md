---
title: AngularJs整合REST风格api,CRUD请求服务合并
date: 2017-04-25 16:18:15
tags: 'angular'
---
前后端分离项目,前端采用angularJs+webpack,后台采用Spring Boot 微服务框架,REST风格API.
**RUST关键原则**
- 为所有“事物”定义ID
- 将所有事物链接在一起
- 使用标准方法
- 资源多重表述
- 无状态通信

<!--more-->
因为CURD基本请求都差不多,而且后台云平台支持跨域,主要方法`GET`,`DELETE`,`POST`,`PUT`,其中`GET`,`POST`方法可以传ID;
直接上代码:
### REQUEST服务写法
```javascript
module.exports = function ($http, $q, commonConfig) {
    return {
        doRequest: function (url, method, token, data) {
            console.log(url);
            console.log(method);
            console.log(data);
            var _data = {
                method: method,
                url: commonConfig.domain + url,
                data: data,
                headers: {
                    "Accept": "*/*",
                    "accessToken": token
                }
            };
            var Result = $q.defer();
            $http(_data).success(function (res) {
                Result.resolve(res);
            }).error(function (err) {
                Result.reject(err);
            });
            return Result.promise;
        }
    }
};
```
### `GET`和`DELETE`请求写法:
```javascript

module.exports = function ($scope, RequestService, $state) {
    var token = '1';
    $scope.getStudentList=function () {
        RequestService.doRequest('student/student','GET',token).then(function (res) {
            console.log(res);
            $scope.StudentList = res;
        }, function (error) {
            console.log(error);
        });
    };

    $scope.getStudentList();
    //选中数组
    $scope.checkedArry = [];
    //全选功能
    $scope.checkAll = function () {
        if ($scope.check_all) {
            $scope.checkedArry = [];
            angular.forEach($scope.StudentList, function (i) {
                i.checked = true;
                $scope.checkedArry.push(i.id);
            });
        } else {
            angular.forEach($scope.StudentList, function (i) {
                i.checked = false;
                $scope.checkedArry = [];
            });
        }
    };
    //单选功能
    $scope.checkOne = function () {
        angular.forEach($scope.StudentList, function (i) {
            var index = $scope.checkedArry.indexOf(i.id);
            if (i.checked && index === -1) {
                $scope.checkedArry.push(i.id);
            } else if (!i.checked && index !== -1) {
                $scope.checkedArry.splice(index, 1);
            }
        });

        if ($scope.StudentList.length === $scope.checkedArry.length) {
            $scope.check_all = true;
        } else {
            $scope.check_all = false;
        }
    };
    //数据删除
    $scope.delete = function () {
        var list = $scope.checkedArry;
        console.log(list);
        RequestService.doRequest('student/student?list='+list,'DELETE',token).then(function (res) {
            console.log(res);
            $scope.getStudentList();
        }, function (error) {
            console.log(error);
        });
    }
};
```
因为这两个方法无需`data`,`DELETE`也是根据url传id,id是单选按钮和多选按钮控制的,直接将数据传入.
### `POST`和`PUT`写法:
```javascript
module.exports = function ($scope, $stateParams, RequestService) {
  //有参编辑,无参新增
  var token = '1';
  if ($stateParams.id) {
      $scope.getStudentList = function () {
          RequestService.doRequest('student/student/' + $stateParams.id, 'GET', token).then(function (res) {
              console.log(res);
              $scope.StudentList = res;
          }, function (error) {
              console.log(error);
          });
      };
      $scope.getStudentList();
  }
  var data = {};
  $scope.submit = function () {
      RequestService.doRequest('student/student/' + ($stateParams.id ? $stateParams.id : ''), $stateParams.id ? 'PUT' : 'POST', token, data).then(function (res) {
          console.log(res);
      }, function (error) {
          console.log(error);
      });
  };

};
```
首先判断是否有url参数,点击新增是无参,点击编辑传递这条数据的id,请求时也是根据参数判断是`POST`还是`PUT`