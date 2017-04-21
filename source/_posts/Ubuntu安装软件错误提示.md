---
title: Ubuntu安装软件错误提示
date: 2017-04-21 22:37:02
tags: Linux
---
![](http://i2.muimg.com/1949/808576e847d69126.png)

如图，Ubuntu 上用 apt-get 下载安装 zsh 的时候遇到莫名的错误提示，看错误提示的意识大概就是`/var/lib/dpkg/lock`这个目录出的问题。网上的说法是，可能有另一个程序正在运行，导致资源被锁不可用。而导致资源被锁的原因可能是上一次运行安装更新时没有正常完成。而我记得上次并没有什么没有正常完成的安装更新，先删除这个目录试试。

解决方法，运行下面命令：

```
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```

又能正常安装更新了