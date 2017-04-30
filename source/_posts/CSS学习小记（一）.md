---
title: CSS学习小记（一）
date: 2017-04-29 22:39:07
tags: CSS
---
### user-select属性

在浏览一些网站的文章时，读到一些有趣的文字，想把它复制收藏到自己的收藏集中，发现文字不可以选择，也就不可以复制。这种方法可以保护文章版权的效果，虽然花点时间手打也行，不过也是一定程度上保护了文章的传播。

这种效果是怎样实现的呢，用`user-select`属性就可以实现，在样式中使`user-select:none`即可使用户不能选择元素中的任何内容，包括文本节点。非常方便，`user-select`是用来控制内容的可选择性的属性。由于该属性是 CSS3 新增属性，部分浏览器可能不兼容，使用时最好加上各浏览器内核前缀。

嘛~在使用的时候发现了一招瞬间破解这种禁止复制的方法 orz 。对着要复制的文字右键选择 “**检查**” （我以Chrome为例），弹出开发者控制台，在代码栏中找到 CSS 样式设置，把`user-select:none`改为`user-select:auto`，即可启用复制功能。

哈哈，这什么鬼，同样的方法可应用于查看浏览器保存的密码上！一般我们输入账号密码登录某一网站的时候浏览器会提示你是否要保存密码，或者网站上有复选框可以勾选记住密码，这都是利用浏览器的 **cookie** 来把密码暂存于浏览器中，以便下次登录可以方便点。而那些密码一般都是以黑色圆点方式来显示，但是不要天真地以为这样就看不见密码的内容了。用刚才所说的方法，只要把密码框的`type="password"`改为`type="text"` 即可将那可悲的密码暴露于艳阳下，一览无遗。

这个故事告诉我们，把密码保存于浏览器中虽然方便，但是也是不安全的。大家请多加注意
<!--more-->

### visibility属性

一直使用`display:none`来隐藏元素，用得很爽。现在发现还有另一种方法可以实现隐藏元素的效果，使用`visibility:hidden`就可以了。

那么问题来了，`display:none`和`visibility:hidden`究竟有什么样的区别呢，两种完全不一样的东西总有些不同吧。

区别就是：

`visibility:hidden`隐藏元素，但元素所占的空间并不会消失，还会占着位置在那里，犹如一个隐身人，还摸得着。

`display:none`，则是隐藏元素并不会占位，凭空消失，看不见摸不着，人间蒸发了

网上有人觉得这种用于隐藏元素的属性在浏览器加载脚本时会选择不加载，而到要显示元素时才加载。这种说法我觉得是不正确的，浏览器加载脚本是整个样式文件一起加载进去然后解析显示的，不会在加载前特意分析一遍脚本再选择性加载。

visibility 属性取值为` collapse`时，表示隐藏表格的一行或者一列，默认值为`visible`，对所有元素可见

###CSS3弹性盒子
CSS3 增加了一个新的布局方式：**弹性盒子**。作用是使得当页面布局必须适应不同的屏幕尺寸和不同的显示设备时，元素可预测地运行。

适应不同屏幕尺寸显示器，这本是另前端开发人员十分苦恼的事情，如今弹性盒子的出现可方便解决这个问题，所以弹性盒子绝对是未来的趋势啊。

声明方法是：
```
display:flex;
```
或
```
display:inline-flex;
```

使用弹性布局有一个重要的特点是，其子元素的`float`,`clear`,`vertical-align`属性将全部失效。

使用弹性布局的元素被称为 **Flex Container**（弹性容器），其子元素则被称为 **flex item** （弹性项目），容器的宽称为** main axis** (主轴)，高称为 **cross axis** （侧轴），每条轴又有起点和终点，花样特别多。

弹性布局有六大属性和它们若干的值：

**flex-direction属性**
确定主轴的方向，选择从左开始还是从右开始或是从上开始还是从下开始，默认为 row
```
flex-direction: row | row-reverse | column | column-reverse;
```

**flex-wrap属性**
排在一条轴线上的项目满了的时候，决定向上排列还是向下排列，默认为 no-wrap
```
flex-wrap: no-wrap | wrap | wrap-reverse;
```

**flex-flow属性**
这是`flex-direction`属性和`flex-wrap`属性的合并，也就是说一次定义两个属性的值，默认值为 row nowrap
```
flex-flow: <flex-dirwxtion> || <flex-wrap>
```

**justify-content属性**
定义在当前行上主轴的项目该怎么排布，有左对齐、右对齐、居中、两端对齐和每个项目两侧的间隔相等，默认为 flex-start
```
justify-content: flex-start | flex-end | center | space-between | space-around;
```

**align-items属性**
这个与`justify-content`相对，是定义在当前行上沿侧轴的项目怎么排布，有上对齐、下对齐、中间对齐、项目第一行文字的基线对齐和如果项目未设置高度或设为auto则占满为容器的高度，默认为 stretch
```
align-items: flex-start | flex-end | center | baseline | stretch;
```

align-content属性
定义多跟轴线的对齐方式，所以只有一根轴线的的话则不起作用
```
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
```

以上是容器的属性，还有项目的属性详情可以转战阮一峰老师的博文：<a href="http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html" target="_blank">Flex 布局教程：语法篇</a> 和实战篇的 <a href="http://www.ruanyifeng.com/blog/2015/07/flex-examples.html" target="_blank">Flex 布局教程：实例篇</a>