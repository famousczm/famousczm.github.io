---
title: cannot read property getContext of null
date: 2017-04-01 16:40:35
tags: JS
---
用 *JS* 在 **cavans** 上画图的时候，加载页面没有任何效果，调试显示如下错误：

![](http://i4.buimg.com/1949/4604267099d7513d.png)

![](http://i4.buimg.com/1949/800ea281834b3356.png)

提示 **getContext** 读不到数据，按理 **c** 应该存放 **canvas** 的对象，用：

```
console.log(c)
```
调试返回结果为 **null** ！这下问题就清晰了，原因是在 *HTML* 文件中引入 *JS* 的位置有问题，我这`<script>`语句放在`<head>`中，执行JS语句时 canvans 还没有定义，所以读取为 null 。只要将`<script>`语句放在 **canvas** 后即可！

还有一种解决方法是在 JS 文件中把 JS 语句放进这个语句中：
```
window.onload = function(){
    /*JS CODE*/
}
```
`window.onload`能等待所有的 html 文件执行完毕再加载函数里的 JS 代码

<hr/>
