---
title: 'AngularJS:Uncaught Error: [$injector:modulerr]!问题'
date: 2017-06-27 20:47:56
tags: AngularJS
---
使用控制器的时候，出现如下这种错误信息：
```
angular.min.js:6 Uncaught Error: [$injector:modulerr] http://errors.angularjs.org/1.6.4/$injector/modulerr?p0=myApp&p1=Error%3A%2…tp%3A%2F%2F127.0.0.1%3A8020%2FAngularTest%2Fjs%2Fangular.min.js%3A22%3A179)
    at angular.min.js:6
    at angular.min.js:42
    at q (angular.min.js:7)
    at g (angular.min.js:41)
    at eb (angular.min.js:46)
    at c (angular.min.js:21)
    at Sc (angular.min.js:22)
    at ue (angular.min.js:20)
    at angular.min.js:331
    at HTMLDocument.b (angular.min.js:38)
```
不能获取什么，模块什么的不懂，把压缩版的 Angular.min.js 切换 Angular.js 查看详细错误信息：
```
Uncaught Error: [$injector:modulerr] Failed to instantiate module myApp due to:
Error: [$injector:nomod] Module 'myApp' is not available! You either misspelled the module name or forgot to load it. If registering a module ensure that you specify the dependencies as the second argument.
....
```
'myApp' 模块得不到。这个 myApp 就是声明 AngularJS 应用的 `ng-app="myApp"`，要使用 angular.module 函数来创建模块：
```
var app = angular.module("myApp", []);
```
创建模块后错误变成：
```
angular.js:14525 Error: [$controller:ctrlreg] The controller with the name 'MyController' is not registered.
```
我定义的控制器 MyController 还没注册，使用以下方式注册：
```
app.controller('MyController', function($scope){...});
```
然后就可以了。

一般还有些情况是对应的模块文件没有加载，如 angular-route.js 作为一个单独的模块并没有合并到 angular.js 中，所以要单独加载。

要是到这里还没有解决问题的话，就要看看代码有没有写错了，常见的错误是(我平时常犯的 orz)，没有关闭标签 `<script> </script>`

学习愉快。
