---
title: 'Property ''find'' does not exist on type XXX[]'
date: 2017-09-07 13:03:40
tags: Angular2
---
使用 数组的 `find`属性功能时，编译器提示错误，说什么数组不存在 `find`这个属性。

简单的解决方法：

在 TypeScript 的配置文件 `tsconfig.json`中把 `target` 设置为 `es6`，即把目标编译成 es6 规范的脚本，我之前设置的是 es5 ，es5 可能不支持 find 属性。
