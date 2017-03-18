---
title: Hexo中文乱码
date: 2016-10-05 13:52:43
tags: Hexo
---
用*Hexo*搭建的博客title默认为Hexo，我想修改为自己设置的值。于是在public文件夹下的index.html中修改title的值，可是重新生成静态文件时一切又恢复为默认值Hexo。

原因是每次更新博客时都是在**Git Shell**中清除缓存文件和已生成的静态文件，然后再重新生成新的静态文件。最后deploy到github上，所以原来的index.html文件已经被毁灭了再生成，原先的设置也不复存在了

这要在文件_config.yml中修改title的值，把

`title： Hexo`

修改为

`title: 柱铭（填要修改的值）`

即可，要注意的是，：后面要空一个格

到这里重新部署时，网页的title出现乱码

**解决方法：**

把_config.yml文件编码成UTF-8即可

我用*Notepad++*的操作是：**格式->转为UTF-8编码格式->保存**