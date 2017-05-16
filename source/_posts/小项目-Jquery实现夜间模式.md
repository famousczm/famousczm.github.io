---
title: 小项目:Jquery实现夜间模式
date: 2017-05-16 12:54:03
tags: 项目
---
##### 项目起源

手机的看书软件有夜间模式来降低屏幕的亮度，在黑夜的环境下使眼睛没有那么疲惫。所以想到在用户浏览网页的时候也有这个功能就好了。而且实现也不难，在借鉴一些前辈的思路完成了这个作品

##### 项目功能
按下按钮使背景颜色变深，文字颜色变亮，再按一下就回复原状

##### 实现思路
按钮用隐藏 checkbox ，把 label 设置成按钮的方法。然后监听 checkbox 如果发生 onchange 事件，就判断 checkbox 是否为 changed ，若是改变背景和字体颜色，若不是，变为原来的颜色。

##### 代码实现
**HTML**
```
<div class="mode">
       <input type="checkbox" name="check" id="mode" class="mode__i" />
       <label class="mode__l" for="mode"></label>
</div>
<p>Today is a nice day</p>
```
label 标签要关联 input

**CSS**
这里我用定义全局变量的做法把背景颜色和文字颜色定义好：
```
:root{
	--light:#ddd;
	--dark:#333;

	--text:var(--dark);
	--bg:var(--light);

	--size:100%;
}
```
隐藏 checkbox
```
.mode__i{
	display:none;
}
```
用 label 代替 checkbox
```
.mode__l{           /*准备代替checkbox*/
	display:block;
	position:relative;
	width:140px;
	height:40px;
	margin:0 auto;
	border-radius:20px;
	background:#888;
}
```
画出按钮的样式
```
.mode__l:hover{
	cursor:pointer;
}

.mode__l::before{       /*画圆*/
	position:absolute;
	top:0;
	left:calc(50% - 20px);
	background:#555;
	border-radius:20px;
	transition:all 300ms;
	transform:translateX(-50px);
	width:40px;
	height:40px;
	content:'';
	z-index:1;
}
.mode__l::after{
	position:absolute;
	top:50%;
	left:50%;
	transform:translate(-50%,-50%);
	font-size:14px;
	font-family:monospace;
	color:#fff;
	content:'day';
}

.mode__i:checked + label::before{
	transform:translateX(50px);
}
.mode__i:checked + label:after{
	content:'night';
}
```

**JavaScript**
```
$('#mode').on('change',function(){
	if($('#mode').is(':checked') === true){
		$('body').css('--bg','var(--dark)');
		$('body').css('--text','var(--light)');
	}else{
		$('body').css('--bg','var(--light)');
		$('body').css('--text','var(--dark)');
	}
});
```
记得要先引入 JQuery 文件

##### 运行结果
![](http://i4.buimg.com/1949/65baed9d62bdc441.png)

![](http://i4.buimg.com/1949/cdf481f8100654d0.png)

详细代码我放在了 github 上 <a href="https://github.com/famousczm/Night-Mode/tree/master" target="_blank">点我</a>