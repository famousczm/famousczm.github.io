---
title: AngularJS简单说说我对三大作用域的理解
date: 2017-07-15 15:10:40
tags: AngularJS
---
最近在看《AngualrJS 权威教程》，深感这个框架确实有点消耗脑力，明明每个字都会读，连在一起怎么就不懂了呢 (´_ゝ`) 。看到三大作用域（外部作用域，继承作用域和隔离作用域）那里刚开始有点乱，现在理清了思路简单说说我的理解。

##### 外部作用域

这个很容易理解，就是当前指令的作用域以外的作用域就是外部作用域，例如父级作用域，爷级作用域之类的。
<!--more-->

##### 继承作用域

从父级作用域继承并创建一个新的子作用域，这个子作用域就是继承作用域，它拥有父级作用域定义的的属性，可以在定义指令的时候用 `scope: true`来声明，这样使用该指令的时候就会生成一个继承作用域，如常见的内置指令 `ng-controller`就是这种类型。也就是说继承作用域能访问获取它父级作用域的属性，而父级作用域则不能访问继承作用域的，学过 Java 或其它面向对象语言的朋友应该都不难理解，举个栗子：

HTML
```
<div ng-init="fatherProperty='我在父级作用域生成'">
	           我是父级作用域: {{ fatherProperty }}<br/>
	           我是父级作用域: {{ sonProperty }}
		<div ng-controller="son">
			我是继承作用域: {{ fatherProperty }}<br/>
			我是继承作用域: {{ sonProperty }}
		</div>
	</div>
```
JavaScript
```
var app = angular.module('myApp', []);
app.controller('son', function($scope){
	$scope.sonProperty = "我在子作用域生成";
});
```
结果为：
```
我是父级作用域: 我在父级作用域生成
我是父级作用域:
我是继承作用域: 我在父级作用域生成
我是继承作用域: 我在继承作用域生成
```
一目了然。还有一点是，在继承作用域修改父级作用域的属性值时，父级作用域的属性值会不会也被改变呢？如果也被改变了那么继承作用域继承的是属性引用，如果没被改变就是单纯的属性复制了。栗子：

HTML
```
<div ng-init="fatherProperty='我在父级作用域生成'">
	           我是父级作用域: {{ fatherProperty }}<br/>
		<div ng-controller="son">
			我是继承作用域: {{ fatherProperty }}<br/>
		</div>
	</div>
```
Javascript
```
var app = angular.module('myApp', []);
app.controller('son', function($scope){
	$scope.fatherProperty = "我在继承作用域改变了";
});
```
结果是：
```
我是父级作用域: 我在父级作用域生成
我是子作用域: 我在子作用域改变了
```
OK，属性复制。但也只是限于属性是字符串、数字之类的值而已，当继承的是数组或对象的话，就是属性引用了。栗子：

HTML
```
<div ng-init="fatherProperty={data: '我在父级作用域中生成'}">
	           我是父级作用域: {{ fatherProperty.data }}<br/>
		<div ng-controller="son">
			我是继承作用域: {{ fatherProperty.data }}<br/>
		</div>
	</div>
```
Javascript
```
var app = angular.module('myApp', []);
app.controller('son', function($scope){
	$scope.fatherProperty.data = "我在继承作用域改变了";
});
```
结果是：
```
我是父级作用域: 我在继承作用域改变了
我是继承作用域: 我在继承作用域改变了
```
继承作用域也就差不多这样，也很好理解。

##### 隔离作用域

书上说较难理解的隔离作用域，其实也没多难。外部作用域和继承作用域之间多少有点关系，而隔离作用域如它名字的一样非常内向，很少和别人接触，所以它不像继承作用域一样可以访问它的外部作用域，隔离作用域自己跟自己玩，不能访问外部作用域。

在定义指令时用` scope: {}`来声明，举个栗子：

HTML
```
<div ng-init="fatherProperty={data: '我在父级作用域中生成'}">
	           我是父级作用域: {{ fatherProperty.data }}
		<div my-directive></div>
	</div>
```
Javascript
```
var app = angular.module('myApp', []);
app.directive('myDirective', function() {
	return {
		restrict: 'A',
		scope: {},
		template: '我是隔离作用域: {{ fatherProperty.data }}'
	};
})
```
结果是：
```
我是父级作用域: 我在父级作用域中生成
我是隔离作用域:
```
那么隔离作用域可不可以像 ng-controller 那样可以自己定义属性？

当然可以自己定义属性，不然还怎么玩，定义属性的方法就是在`{}`里面定义，但是不能直接给它赋值，只能接受外界的传值（看来也没有很自闭），怎么传，要用绑定策略，看个栗子：

HTML
```
<div ng-init="fatherProperty={data: '我在父级作用域中生成'}">
	           我是父级作用域: {{ fatherProperty.data }}
		<div my-directive test="{{fatherProperty.data}}"></div>
	</div>
```
Javascript
```
var app = angular.module('myApp', []);
app.directive('myDirective', function() {
	return {
		restrict: 'A',
		scope: {
			test: "@"
		},
		template: '我是隔离作用域: {{ test }}'
	};
});
```
结果是：
```
我是父级作用域: 我在父级作用域中生成
我是隔离作用域: 我在父级作用域中生成
```
可以看到，隔离作用域也能访问外部作用域了，其实更像外部作用域将数据从一个接口处传进隔离作用域里...这隔离作用域不但内向还懒啊哈哈。这个 `@`符号用来接受传进来的字符串然后赋值给属性，但只能接受字符串，所以遇到表达式的时候还要用 { { } } 来翻译成字符串输入。用` =`就不用这么麻烦了，直接写就 ok，如下：

HTML
```
<div ng-init="fatherProperty={data: '我在父级作用域中生成'}">
	           我是父级作用域: {{ fatherProperty.data }}
		<div my-directive test="fatherProperty.data"></div>
	</div>
```
Javascript
```
var app = angular.module('myApp', []);
app.directive('myDirective', function() {
	return {
		restrict: 'A',
		scope: {
			test: "="
		},
		template: '我是隔离作用域: {{ test }}'
	};
});
```
结果同上。

如果直接把 `fatherProperty`这个对象传进去，则隔离作用域内的属性`test`接受的是一个对象，输出时应为 {{ test.data }}。

隔离作用域目前也了解至此。

That's all ~
