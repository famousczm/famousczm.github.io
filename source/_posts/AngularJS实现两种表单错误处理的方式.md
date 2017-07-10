---
title: AngularJS实现两种表单错误处理的方式
date: 2017-07-09 19:59:44
tags: AngularJS
---
AngularJS 的自动双向数据绑定的确非常强大，对表单的处理也十分出色。它能和 HTML5 的混合使用更是让我震惊不已，这里我利用 AngularJS 的强大简便地实现表单的两种错误处理方式。

表单错误处理即在输入表单数据时实时判断数据的正确与否并将错误提示显示给用户。第一种是当输入框失焦时才显示错误信息；第二种是当点击提交按钮时才显示错误信息。
<!--more-->

##### 失焦才显示错误信息

自定义一个 Directive 命名为 `ngFocus`，`app.js`代码如下：
```
var app = angular.module('myApp', ['ngMessages']);

app.directive('ngFocus', [function(){
	return {
		restrict: 'A',
		require: 'ngModel',
		link: function(scope, element, attrs, ctrl){
			ctrl.$focused = false;
			element.bind('focus', function(evt){
				scope.$apply(function(){
					ctrl.$focused = true;
				});
			}).bind('blur', function(evt){
				scope.$apply(function(){
					ctrl.$focused = false;
				});
			});
		}
	};
}]);
```
设置 Directive 为属性类型，并引入 ngModel Directive，定义数据变量 `$focused`，用来记录当前是否获得焦点。然后在当前元素上绑定 `focus`和`blur`两个事件，根据不同情况分别设置`$focused`的值。

由于要使用 ngMessages 这个模块，所以在创建 app 模块时要引入这个模块。

`index.html`的代码如下：
```
<form name="signup_form" novalidate ng-submit="signupForm()">
	<fieldset>
		<legend>Signup</legend>
		<div class="row">
			<div class="large-12 columns">
				<label>Your name</label>
				<input type="text"
				  placeholder="Name"
				  name="name"
				  ng-model="signup.name"
				  ng-minlength="3"
				  ng-maxlength="20" required ng-focus />
			</div>
			<div class="error"
                 ng-messages="signup_form.name.$error"
                 ng-messages-multiple
                 ng-if="!signup_form.name.$focused && signup_form.name.$dirty">
				<div ng-messages-include="templates/other.html"></div>
			</div>
		</div>
        <button type="submit" class="button radius" ng-disabled="signup_form.$invalid">
			Submit
		</button>
    </fieldset>
</form>
```
这里注意的是表单里无论是 form 还是 input 元素都要有 **name** 属性，因为后面获取输入框信息的时候要用到。

`novalidate`取消了浏览器对表单的默认验证，我们自己来设置对表单的验证。

`ng-submit`是提交时执行的函数，这里先不管

`input` 输入框可设置指令 `ng-minlength` 和 `ng-maxlength` 来规定输入的字符串长度，结合 H5 的 required 属性，规定输入框不能为空。再加上刚自定义的 Directive `ng-focus`，html 中的命名跟 JS 中的不一样，JS 对大小写敏感而 html 不敏感，所以转化的时候要注意。

最底的那个 div 就是错误信息了，我们要控制它的显示，当输入框有输入数据并且失焦的时候我们才去检查它并显示错误提示，其它情况隐藏。要是以往用直接用 JS 写要操作 DOM 很麻烦。而用 AngularJS 超级方便，用 `ng-if`就可以像平时使用的 if-else 语句一样选择显示了，使用 `form名字.input名字.变量名`的格式获取想要的数据，这里当输入框数据被更改过而且失焦的时候 显示错误信息。

`ng-messages="signup_form.name.$error"`获取错误信息，错误信息包括，`required`、`minlength` 和 `maxlength`，当输入框数据不满足设置条件时，对应的项就会为 true。可以通过打印`signup_form.name.$error`来深入理解。

`ng-messages-multiple`可以显示多条错误信息

提交按钮中设置了 `ng-disabled="signup_form.$invalid"`，当表单有错误输入时禁止提交，等到全部输入正确后才允许提交

里面包含一个 div ，为了能复用相同的代码，使用`ng-messages-include`来引入外部文件。这个 other.html 的代码如下：
```
<small class="error" ng-message="required">不能为空</small>
<small class="error" ng-message="minlength">长度不能少于3位</small>
<small class="error" ng-message="maxlength">长度不能大于20位</small>
```
`ng-message`使相关的错误显示对应的错误信息，值为 true 时才显示。

记得要引入外部文件 **angular-messages.min.js**，并且放在 **angular.js** 后面不然会报错 。我这里还使用了 **foundation** 框架，为了少写点样式。

##### 提交才显示错误信息

`app.js`的代码如下：
```
var app = angular.module('myApp', ['ngMessages']);

app.controller('myController', function($scope){
	$scope.submitted = false;    //submitted为true时才显示错误信息
	$scope.signupForm = function(){
		if($scope.signup_form.$valid){
			//正常提交
		}else{
			$scope.signup_form.submitted = true;
		}
	}
});
```

在控制器中定义变量 `submitted`，提交前为 false，提交后有错误为 true。当表单提交后执行函数 `signupForm`，判断表单是否有错，没错正常提交，有错将 submmitted 改为 true。

`index.html`的代码如下：
```
<form name="signup_form" novalidate ng-submit="signupForm()" ng_controller="myController">
	<fieldset>
		<legend>Signup</legend>
		<div class="row">
			<div class="large-12 columns">
				<label>Your name</label>
				<input type="text"
				  placeholder="Name"
				  name="name"
				  ng-model="signup.name"
				  ng-minlength="3"
				  ng-maxlength="20" required ng-focus />
			</div>
			<div class="error" ng-messages="signup_form.name.$error" ng-messages-multiple ng-if="signup_form.submitted && signup_form.name.$dirty">
				<div ng-messages-include="templates/other.html"></div>
			</div>
		</div>
        <button type="submit" class="button radius">
			Submit
		</button>
    </fieldset>
</form>
```
基本与上一种方法差不多，不过要把判断语句 ng-if 改为判断`signup_form.submitted`，然后提交按钮去掉 ng-disabled，因为已经没有意义了。

就是这样。