---
title: CSS学习小记（四）
date: 2017-05-17 22:15:19
tags: CSS
---
#### box-sizing
在做项目的过程中，有时定义了两个宽度相同的 div ，但是为其中一个添加了 padding 和 border 之后，宽度却不一样了，因为被 padding 和 border 硬生生给撑开了，这种事情是非常苦恼的，之前是通过减少宽度来使总宽度保持一致。直到发现了这个属性。

**box-sizing** 属性用于更改用于计算元素宽度和高度的默认的 CSS 盒子模型。可以使用此属性来模拟不正确支持CSS盒子模型规范的浏览器的行为。

我的那个问题只需要在两个 div 内设置`box-sizing:border-box`就 ok 了，这属性设置的意思是此元素的内边距和边框不再会增加它的宽度。这样 div 的宽度就不会因为 padding 和 border 的设置而改变了。

默认值是`box-sizing:content-box`，这个就是平时说的标准盒子模型了，设置边框和内边框会使宽度增加。

由于这也是实验中的属性，使用时最好加上浏览器前缀。

#### transform-style 和 outline-style
`transform-style `这个属性用于指定使用该属性的元素的子元素是位于三维空间还是二维平面上。所以其值有两个，`flat` 和 `preserve-3d`。`flat`指定子元素位于平面内，是默认值。`preserve-3d`指定子元素位于三维空间中。在实现3D效果时，该属性起到非常关键的作用。

`outline-style`属性是用于设置一个元素轮廓的样式。那么轮廓是什么？跟边框有什么区别吗？

轮廓是围绕元素绘制的线，在边框边缘之外，以突出元素。边框是矩形的且占据一定空间，而轮廓虚无缥缈，不一定是矩形也不占据空间。

`outline-style`的取值和`border-style`差不多，而`border`是集合了`border-style`、`border-width`、`border-color`三种属性的简写属性，同理，`outline`也是集合了`outline-style`、`outline-width`、`outline-color`三种属性的简写属性。用法也就和`border`一样。

#### 显示 title 标签内容
以前我一直认为 title 标签只是设置网页顶部的文字，只存在于那里。直到我偶然地使用JQuery选择器将所有的元素显示出来时，即如下语句：
```
$("*").css("display","block");
```
发现多了个奇怪的字符，对比了一下文档中只有 title 元素才设置了这个值。用开发者工具查看，原来 title 元素平时默认是隐藏的。嘛，这的确是些微不足道的细节，但是突然发现这些细节时又会觉得很惊奇像发现什么新大陆似的。

下个星期要去面试了，希望一切都好

