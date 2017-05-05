---
title: CSS学习小记（二）
date: 2017-05-02 20:32:05
tags: CSS
---
### :root伪类

**:root** 伪类用于匹配文档树的根元素，在 HTML 文档中，**:root** 表示为`<html>`
元素，声明全局 CSS 变量的时候很有用，用法如下：
```
:root{
  background-color: cornflowerblue;
  padding: 3em;
}
```

相当于在声明 html 标签的 CSS 变量，不过伪类选择器具有类的特性，优先级比标签选择器要高
```
html{
  background-color: cornflowerblue;
  padding: 3em;
}
```

### CSS变量
在很多编程语言中，都会有变量这种基本的概念，可以方便地传值赋值，使代码变得清晰易懂，还可以提高程序的性能。但是我在学习 CSS 的过程中一直没有发现有变量的存在，CSS 一直用于描述文档的样式，好像变量的用处也不大。但久而久之，例如开发一些大型网站，很多样式都有重复的值，例如颜色，光写这6个字符组成的十六进制数就很容易让人出错。所以也就越发觉得变量的必要。随着 Sass 等 CSS 预编译工具的流行，变量也逐渐被官方所认可并且规定出来了。

声明一个变量的方法，我们可以在`:root`里面当做全局变量声明：
```
:root{
   --light: #ddd;
   --dark: #333;
}
```

在变量名前加上`--`就可以啦。当在需要使用变量时，用`var(变量名)`来是使用，十分方便：
```
body{
   color: var(--light);
   background-color: var(--dark);
}
```

由于这还是个在实验中的功能，所以未来 CSS 变量的语法可能还会改变，让我们继续期待它的成长
<!--more-->

### opacity属性
使一个元素变透明，除了使用`rgba()`中的设置外，还可以使用`opacity`属性，更加方便快捷，`opacity`属性用于指定一个元素的透明度，常与`animation` 属性使用（个人觉得），取值0到1。0为完全透明，1就是完全不透明啦，用法示范：
```
{
   opacity: 0.5;
}
```

不过`opacity`属性有一个特点，当一个元素使用`opacity`属性透明时，其子元素也紧随该元素一起透明，即使该元素的子元素设置了不同的`opacity`值。也就是说一旦元素设置了`opacity`，那么子元素的`opacity`就失效了。

对于想实现父元素透明度和子元素透明度不一样的效果，我的做法是父元素使用`rgba()`来设置透明度，而子元素使用`opacity`来设置透明度。顺序反过来的话达不到想象中的效果，我也不清楚是怎么回事。

### transform属性
用 CSS 画图时我最喜欢的属性，功能太强大了（迟些累积好经验后写一篇 CSS 画图的心得）。`transform`属性有四大功能，分别是`translate(转换)`、`rotate(旋转)`、`scale(缩放)`、`skew(倾斜)`。因为这个也是 CSS3 的新元素来的，所以实验中，使用最好加前缀。不过只对块级元素有效。

下面简单介绍一下四大功能与用法：

**translate 用于平移**
```
transform: translate([x,y]);   /*[x,y]为平移的坐标*/
transform: translateX(x);     /*X方向平移*/
transform: translateY(y);     /*Y方向平移*/
```

**rotate 用于旋转**
```
transform:rotate(angle);    /*按顺时针方向旋转一定角度，可以取负数，单位为deg*/
```

**scale 用于缩小和放大**
```
transform:scale(x,y);      /*表示一个二维的缩放操作，如果y未指定，则默认为与x相同*/
transform:scaleX(x);       /*在X方向上的缩放*/
transform:scaleY(y);       /*在y方向上的缩放*/
```

**skew 用于倾斜**
```
transform:skew(x,y);      /*元素在x轴和y轴方向以指定的角度倾斜，如果y没有指定，则y轴没有倾斜*/
transform:skewX(angle);    /*绕x轴以指定的角度倾斜*/
transform:skewY(angle);    /*绕y轴以指定的角度倾斜*/
```

我用得最多的就是 `rotate`，将图像旋转来旋转去的，很容易就达到想要的效果，其次就是`translate` ，配合`animation`属性使用，立竿见影。