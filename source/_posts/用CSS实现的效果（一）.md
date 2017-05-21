---
title: 用CSS实现的效果（一）
date: 2017-05-21 15:07:18
tags: CSS
---
怎么用 CSS 实现鼠标指向某个图标会自动在该图标上方显现相关内容呢？效果如下：

![](http://i4.buimg.com/1949/52baf72c9964a050.png)
<!--more-->

可以在该内容区块内再定义一个 div 区块，这个新区块就是要显示的内容了。
```
<div class="icon" tabindex="1">
    <i class="fa fa-smile-o" aria-hidden="true"></i>
    <div class="title">Smile</div>
</div>
```

然后就是为该内容设置样式了，要做一个悬浮于图标上方的有点圆角黑色类似对话框的样式。先做圆角黑框。
```
background:rgba(0,0,0,0.75);
padding:10px 15px;
border-radius:4px;
```
目前这样挤在图标中很难看，要让它们分离出去，用绝对布局的方式
```
z-index:2;
position:absolute;
top:-110%;
left:50%;
-webkit-transform:translateX(-50%);
	    transform:translateX(-50%);
```
就变成了这个样子

![](http://i1.piimg.com/1949/8bd1d652a50e9720.png)

接下来是做成对话框的样子，要加个小尾巴，这要用到`:after`选择器加上绝对布局了。通过设置`border:10px solid transparent`位小尾巴准备了一个实心的 20*20 的透明正方形空间，然后用`border-top:10px solid rgba(0,0,0,0.75)`来显示正方形上边的三角形，也就是我们要实现的小尾巴了。代码如下：
```
.icons > .icon .title:after{
	content:'';
	position:absolute;
	border:10px solid transparent;
	border-top:10px solid rgba(0,0,0,0.75);
	bottom:-20px;
	left:50%;
	-webkit-transform:translateX(-50%) translateY(0px);
	        transform:translateX(-50%) translateY(0px);
}
```

![](http://i2.muimg.com/1949/e329b89bc5c6448e.png)

然后要把提示信息隐藏起来，比较常见的做法是用`display:none`，但这里我用`opacity:0`来隐藏，即把这个区块完全透明了，然后当鼠标指向图标的时候再把它显示，即：
```
.icons > .icon:hover .title{
	opacity:1;
}
```

为了使这个提示信息平稳地显示，再加点延迟：
```
.icons > .icon .title{
    -webkit-transition:all 0.25s;
	transition:all 0.25s;
}
```
就完成了。

还有一个也是用 CSS 实现的功能。当点击图标的时候，图标能稍微欢快地弹跳起来再落下去回到原来的位置上，这种动画效果像下面这样：

![](http://i2.muimg.com/1949/bae8295578e4f2a7.png)

这效果感觉需要用到 JS 的事件处理吧，其实并不需要，纯 CSS 就可以实现了。

首先要用到`:focus`选择器，它可以选择那些获得焦点的元素。然后再把焦点图标的背景色变透明一点，以给用户一种它被点击了的感觉。然后就是弹跳！弹跳的实现要用到 animation 属性。代码如下：
```
.icons > .icon:focus{
    background:rgba(255,255,255,0.5);
	-webkit-animation:bounce 1s;
	        animation:bounce 1s;
}
```
然后具体实现跳跃部分：
```
@keyframes bounce{
	0%{
		-webkit-transform:translateY(0px) translateX(0.1px);
		        transform:translateY(0px) translateX(0.1px);
	}
	25%{
		-webkit-transform:translateY(-40px) translateX(0.1px);
		        transform:translateY(-40px) translateX(0.1px);
	}
	50%{
		-webkit-transform:translateY(0px) translateX(0.1px);
		        transform:translateY(0px) translateX(0.1px);
	}
	75%{
		-webkit-transform:translateY(-20px) translateX(0.1px);
		        transform:translateY(-20px) translateX(0.1px);
	}
	100%{
		-webkit-transform:translateY(0px) translateX(0.1px);
		        transform:translateY(0px) translateX(0.1px);
	}
}
```

嗯，可以愉快地弹跳了。

<hr>

到目前为止，大三仅剩下一个课程设计的答辩和一个四级考试 ORZ，基本上算结束了，新的征程即将开始，这个暑假要去实习了！！！
