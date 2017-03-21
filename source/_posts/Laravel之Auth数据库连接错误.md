---
title: Laravel之Auth数据库连接错误
date: 2017-03-21 11:03:41
tags: Laravel
---
学习 *Laravel* 不足一天，我就迎来了第二个坑，在 **Auth** 的登录页面随便输入Email地址和密码后，点击登录，弹出如下错误：
![](http://p1.bpimg.com/1949/35c6f9efe32ac13c.png)

看了一下，勉强知道是数据库连接不上的问题，不过这里出错也是正常的嘛，我又没有配置数据库，还以为 **PHP** 会内置数据库的我实在是太天真了。那么就赶紧去配置数据库吧。
<!--more-->
**解决方法是：**

在 *Laravel* 的根目录下，打开 **.env** 文件，修改为自己的数据库设置
```
DB_HOST=127.0.0.1
DB_DATABASE=laravel5
DB_USERNAME=root
DB_PASSWORD=password
```
我用的是MySQL，我先在MySQL里新创建了一个数据库，命名为laravel5(建议创建一个新数据库，不用添加表，因为后面要迁移Auth的内容到这个数据库内)。然后`DB_USERNAME`里建议用root登录，`DB_PASSWORD`为你数据库root的密码，这里一定要填准确。

后面要关闭服务器，并输入：
```
php artisan make:migration
```
开始迁移，之后打开 laravel5 数据库，就可以发现有三个新添加的表，包括 user 用户表，然后我们就可以重新打开服务器，愉快地注册登录了，数据自动会保存到 laravel5 数据库中。登录成功的界面如下：
![](http://p1.bpimg.com/1949/e2b355c144f41d2f.png)

总的来说这个坑不算深，是我自己误踩进去的