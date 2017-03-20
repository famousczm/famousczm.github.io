---
title: Laravel初体验
date: 2017-03-20 17:20:40
tags: Laravel
---
![](http://p1.bpimg.com/1949/6c312410abc694d6.png)
*Laravel*作为一个框架在PHP界久负盛名，以简洁优雅著称。目前以发布了*Laravel* 6，但是刚学习还是以较为流行的*Laravel* 5开始学吧
<!--more-->

Laravel 5.0 开始对 PHP版本的要求是要<code>>=5.4</code>，我的是5.6没问题。下载一键安装包并解压到某个文件夹，命令行进入该文件夹，输入：
```
php artisan serve
```
出现如下提示：
```
Laravel development server started on http://localhost:8000/
```
并打开浏览器输入地址:**localhost:8000**，出现题图的界面说明*Laravel*启动成功

等等！我之前在本机上的*Apache*端口号是8088，说明这不是运行在*Apache*上的，这怎么好像*Laravel*内置了个Web服务器的感觉？？

其实这的确是使用了其它的服务器，但并不是 *Laravel* 内置了服务器，而是**PHP内置的服务器**，PHP从5.4版本以来就内置了一个Web服务器（所以这就是 *Laravel* 5必须PHP要有5.4版本以上的原因？？）并且*Laravel* 的 **artisan** 命令也支持这个内置 web 服务器，所以 *Laravel* 能运行在内置的服务器里。

内置服务器与传统服务器（Apache/Nginx之流的）的区别是内置服务器不适合用作生产环境中，只能在本地环境中运行和解析PHP代码，而且这也是极其方便，对于在本地上开发网站来说。而且打开内置服务器也十分简单，在当前项目的根目录里输入：
```
php -S localhost:8000
```
就可以了，也能正常解析PHP代码，不用为配置Apache或Nginx错误而烦恼不已。

继续扯回 *Laravel* ... **artisan** 的serve命令还 **host** 和 **port** 这两个参数，看名字就知道这是配置主机地址和端口号的，如：
```
php artisan serve --port=8080
```
就可以把端口号改为8080了

因为我使用的是一键安装包，项目建在哪里就直接往哪里解压就是了，没有下载安装网上说的 **composer** （隐约感觉到后面会有一大波错误袭来）。果不其然，使用牛逼哄哄的Auth系统时（*Laravel* 内置的简单注册登录功能）：
```
php artisan make:auth
```
激活Auth（这要先关闭服务器，激活后再打开服务器），界面是出来了，如下：
![](http://p1.bpimg.com/1949/d953c3c8d9b667de.png)

但是，点击HOME时却弹出这个东西（深感不妙，该来的总会来）
![](https://segmentfault.com/img/bVyK0W)

说是什么 **Encryption.php** 里的 **openssl_encrypt()** 函数没定义，然后我当然就是去找 **Encryption.php** 这个文件了，可是不知道是我用一键安装包的原因还是我没仔细找的原因，找了半天都没不知道这个文件躲哪里了。后来另寻它路，把各种安装教程都仔细看一遍，**解决方法是：**

在PHP的根目录里找到php.ini这个配置文件，打开，在大约893行的位置找到
```
;extension=php_openssl.dll
```
并把前面的分号去掉，保存，重启服务器就好了，（nice，问题解决了就兴奋得想大叫）
![](http://p1.bpimg.com/1949/00e74b35c40b14d5.png)

<hr>

*Laravel* 先探索到这里，感觉还有很多东西要学的 >__<

