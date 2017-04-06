---
title: JS学习小记（二）
date: 2017-04-06 15:52:06
tags: JS
---
##### 获取和替换 p 元素中的文本内容

之前学习的 JS 一直都是修改元素的属性内容，样式什么的，其实 JS 也可以修改元素的文本内容。文档树中的节点并不止元素节点一种，还有属性节点和文本节点。所以要获取元素下的文本内容，就要先使用 **childNode** 属性来获取该元素下的所有子节点信息，然后再使用 **nodeValue** 属性来获取子节点内容。

这里如果直接用：
```
p.nodeValue
```
返回的结果为 null ，因为这里真正需要获得的是 p 元素下的第一个子节点的nodeValue，即 p 元素节点下的文本节点的节点值，而不是 p 元素节点的节点值。所以应该是：
```
p.childNodes[0].nodeValue
```
chileNodes属性返回的是一个数组。如果 p 下没有其它元素节点的话，那么这个文本节点就是它唯一的子节点，用`p.childNodes.length`可看出只有一个子节点。

这样整个文档树就没有什么是我 JS 所不能修改的了哈哈
<!--more-->

##### 没有块级作用域

看《 *Javascript 权威指南* 》的时候，在讲变量的作用域里看到一个比较有趣的 JS 细节，**JS 没有块级作用域**。也就是说，只要在函数体内声明的变量，无论在函数体的哪个地方都是有意义的。举例说：
```
function test(t){
   var i = 0;
   if(t > 0){
      var j = 0;
      for(var k = 0;k < 10;k++){
           console.log(k);
      }
      console.log(k);
   }
   console.log(j);
}
```

这条程序如果在 **jsva** 中，肯定后面两个输出是错误的，变量 **j** 的作用域只是在 if 语句里面，变量 **k** 的作用域只是在 for 循环里面。然而在 JS 中的运行结果是

![](http://i2.muimg.com/1949/b9a88ee708cea6ed.png)

照常输出了

还有一种让我吃惊的情况是，如下例子：
```
var scope = "global";
function f(){
  console.log(scope);
  var scope = "local";
  console.log(scope);
}
f();
```

这个按照我以往的理解，就觉得第一个输出应该是 **global** 因为这个输出在 scope 重定义之前，然而！结果是：

![](http://i4.buimg.com/1949/b806aadcef217901.png)

第一个输出是 **undefine** 这是因为局部变量在整个函数内都是有定义的，不管它在函数里的哪个地方定义。而在未初始化之前，这个局部变量都是 **undefine** 也就是说上面的程序相当于：
```
var scope = "global";
function f(){
  var scope;
  console.log(scope);
  scope = "local";
  console.log(scope);
}
f();
```

