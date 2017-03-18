---
title: PHP字符编码
date: 2016-10-18 14:31:12
tags: PHP
---
昨天用PHP做项目的时候遇到了浏览器字符编码的问题，用Chrome和FF来做测试。本想着改浏览器网页显示的字符编码就可以了，结果一刷新或重新打开就又乱码了。解决方法是设置要浏览器的默认字符编码

*Chrome*很简单，打开设置->显示高级设置->网络内容->自定义字体->编码，设置为UTF-8即可

*FireFox*的49.01版设置不了UTF-8编码，只有各种语言的选择。只能用alt弹出隐藏的菜单栏，在查看那里设置文字编码，不过这不是默认的，刷新就又重置了。

所以以上都不是办法，别人访问你的网站可不能要让别人要先设置默认的字符编码才能看到中文啊。真正的方法是在PHP中设置*meta*标签

在html的head标签里加上：

```
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
```

即可。我写PHP是用Zend Studio，新建PHP文件的时候没有xml的模板，在Windows->Preferences->PHP->Editor->Templates中新建一个也不行。解决方法是替换原来的html4.01的模板，Windows->Preferences->PHP->Code Style->Code Templates->Code,选中html4.01frameset然后编辑把xml的模板替换原来的模板。到这里还没有完成，因为Zend Studio还没有将charset设置的值转化。

继续设置Generate->WorkSpace,把Text file encoding勾选other选中UTF-8就可以了

今天大雨。
