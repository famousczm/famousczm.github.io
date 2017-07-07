---
title: AngularJS Directive replace中的一些问题
date: 2017-07-07 16:39:55
tags: AngularJS
---
自定义 Directive 中使用 template 和 replace 来定义替换的内容时遇到了
```
Error: [$compile:tplrt] Template for directive 'people' must have exactly one root element.
```
这样的错误，自定义的名为‘ people ’的 directive 必须只能拥有一个根节点。

部分源程序如下：
```
var App = angular.module("myApp", []);

App.directive("people", function(){
	return {
		restrict: "E",
		replace: true,
		template: "<p>姓名:{{data.name}}</p><p>性别:{{data.sex}}</p>"
	};
});

App.controller("PhoneListCtrl", function($scope){
	$scope.data = {
		name: "Jack",
		sex: "男"
	};
});
```
当 replace 为 false （默认）时，template 的内容插入到自定义的 Directive 中；当 replace 为 true 时，template 的内容替换掉自定义的 Directive 且 template 的内容只能有一个根节点，即只能有一个元素。

所以解决方法是删掉一个 p 元素，或者把 replace 改为 false 。

然后我想能不能在 people 元素内把删掉的 p 元素内容加进去即这样：
```
<people><p>性别:{{data.sex}}</p></people>
```
结果是，不行。

不论是 replace 为 true 或者 false，people 里面的内容全部都会被 template 替换掉，目前我还不知道有什么方法可以保存这些内容。

<hr/>

这几天沉迷于《巫师3》，要是有机会参与开发这样的游戏该是多美妙的事情
