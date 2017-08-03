---
title: Angular2 Cannot find name '$'问题
date: 2017-07-26 20:54:25
tags: Angular2
---
边做项目边学着 AngularJS ，MVC 模式，双向数据绑定等特性用着觉得很新鲜很舒服。然后突然网上看到 Angular2 的出现，继承了 Angular1.x 和 react.js 的优点，更方便快捷的开发体验。更重要的是它不是 Angular1.x 的延伸，而是全新的重写，新的概念和使用方法。犹豫了几秒，决定改战 Angular2.....

搭建好开发环境后，在引入 jquery 和 bootstrap 第三方插件的时候遇到一个问题，使用 JQuery 语法编译的时候显示 **Cannot find name '$'** 。无法识别这个符号，估计是没有引用到 JQuery 的原因，但我检查了一下那些配置确定正确无误了。

后来在 *stack overflow* 上找到了解决方法：在 **app.module.ts** 中使用：
```
import * as $ from 'jquery';
```
引入 JQuery，很多人不需要这句也能正常编译，虽然我也不知道具体原因是什么，但这方法确实解决了我的问题

参考链接：<a href="https://stackoverflow.com/questions/40994719/cannot-find-name-jquery-in-angular2">Cannot find name 'jquery' in angular2

</a>