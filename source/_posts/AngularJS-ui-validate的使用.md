---
title: AngularJS ui-validate的使用
date: 2017-07-23 20:02:22
tags: AngularJS
---
今天在做项目的时候要用到自定义表单验证，于是用到了 **AngularUI** 的 `ui-validate`插件，遇到了一些问题，解决之后写下这篇博客抒发心中的苦笑不得。

先下载 <a href="https://github.com/angular-ui/ui-validate" target="_blank">ui-validate</a>

然后导入到 html 中。在 js 里用
`angular.module('myApp', ['ui.validate']);`引入，然后就可以愉快地使用 `ui-validate`指令来自定义表单验证了。

举个栗子：

验证电话号码是否正确
```
<input type="text"
       name="phoneNumber"
       ui-validate="{wrongphone: 'wrongPhoneNumber($value)'}">
<div ng-show="form.phoneNumber.$error.wrongphone">
     错误信息
</div>
```
相应的在控制器中定义`wrongPhoneNumber`方法的内容：
```
$scope.wrongPhoneNumber = function(value){
    if(value.length == 0 || value.length != 11){
        return false;
    }

    var myreg = /^((\(\d{2,3}\))|(\d{3}\-))?13\d{9}$/;
    if(!myreg.test(value)){
        return false;
    }

    return true;
}
```
`ui-validate`指令中以键值对的方式写一个对象，这个键就是加入到 `$error`中的新属性，这个值就是一个表达式，可以写函数，我就是在这一步遇到了莫名其妙的问题，我定义了方法，但是它怎么都没反应，检测不了错误。我将方法的定义检查了一万次，确定没错，苦苦思索无果，最后偶然的试验下才知道，**这个表达式里的结果为 false 时，属性才会加入到 $error 中显示 true**。有点绕，试验一次就知道了。

大概就是如此，还有一点就是传进去的那个 `$value`的值其实就是输入框输入的值，那么在控制器中就不用使用 ng-model 绑定输入框然后操作数据那么麻烦了，我试过好像还有点延迟不太同步的现象。

真正把 AngularJS 用在项目中深切感觉到其强大便利之处，虽然这个摸索过程艰辛无比，但熟悉了之后，发现一切都变得那么轻便快捷，不过 HTML 代码明显变多且复杂了...
