---
title: 微信小程序--demo代码分析
date: 2017-06-20 19:28:15
tags: 微信小程序
---
微信小程序已经出来很久了，很多公司都开发了自家产品的小程序版本来迎合这股热潮，大家都非常看好小程序，认为这就是未来。我也是这么认为的，虽然好像反响不大，朋友圈里也没怎么看到相关消息，不免觉得有点失望。我觉得大部分的原因是大家都找不到小程序的入口在哪哈哈，因为要激活，然后大部分必要的应用自己手机都有安装，而且用习惯了就直接打开手机上的 APP 就行了，所以就感觉小程序没什么新奇的地方。其实小程序不但对于开发者来说很方便（跨平台，包装很好的 API ，开发成本低等），而且小程序作为 WEB APP 的代表，对于大众来说不仅能节省手机的内存空间还不用花时间去下载安装，个人认为迟早是要替换手机原生 APP 的。

之前刚出的时候是听说只有企业和政府能开发和发布小程序，我就没有去凑一脚了，现在有了个人开发者这一选项，我决定去一探究竟。
<!--more-->

顺利注册下载了个微信 web 开发者工具之后，就可以边读文档边开发了，刚开始不太懂小程序的项目架构是怎样的，它还很贴心地为我们准备了个 demo 版本的。作为新手的我肯定是从这里开始学习啦。

先看看开发界面

![](http://i4.piimg.com/1949/07dfb6deed7763ba.png)

分三部分，左边就是手机模拟界面，上面有手机型号（默认是 iphone6 ）和 网络选择，中间是项目目录结构，最重要的是以 app 为名字的几个文件，待会分析一下。右边就是编辑器了，相对于其它编辑器来说的确有点简陋，但是胜在界面简洁明了还有自动补全语法提示功能。所以对于开发项目不大的情况下还是可以将就一下的。

马上开始看看这三个文件，`app.js `、` app.json` 、 `app.wxss`。前两个学过前端的都懂是啥，后面的这个 `wxss`文件是什么？其实就是微信（wx）自己的 `css`文件，就是用来写页面样式的，与之对应的 html 就变为 `wxml`文件，用法就是 html 和 xml 的结合。

**app.wxss**
```
/**app.wxss**/
.container {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 200rpx 0;
  box-sizing: border-box;
}
```
小程序中每个页面都对应一个目录，每个目录都有四个基本文件（js、json、wxss、wxml）。这个样式表起到一个全局的作用，在这里定义的样式在所有页面中都能起作用。由代码可以看出，小程序都用上了流行的弹性盒子 flex 布局的方法。移动端网页设计必备。

**app.json**
```
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle":"black"
  }
}
```
配置文件中的 `pages`用来放置每个页面的路径，以 [ 路径 + 页面名 ] 的格式存储，创建页面的时候，也可以直接在这里想要创建的页面路径和页面名然后保存，就会自动生成对应的目录和文件。

`window`也就是配置窗口信息。用来设置最顶端的标题栏的内容。字体颜色、背景颜色等。

**app.js**
```
//app.js
App({
  onLaunch: function () {
    //调用API从本地缓存中获取数据
    var logs = wx.getStorageSync('logs') || []
    logs.unshift(Date.now())
    wx.setStorageSync('logs', logs)
  },
  getUserInfo:function(cb){
    var that = this
    if(this.globalData.userInfo){
      typeof cb == "function" && cb(this.globalData.userInfo)
    }else{
      //调用登录接口
      wx.login({
        success: function () {
          wx.getUserInfo({
            success: function (res) {
              that.globalData.userInfo = res.userInfo
              console.log(res.userInfo);
              typeof cb == "function" && cb(that.globalData.userInfo)
            }
          })
        }
      })
    }
  },
  globalData:{
    userInfo:null
  }
})
```
app.js 负责处理逻辑的脚本文件。纵观整个文件，只是调用了一个 App() 函数而已。这个函数是小程序里内置的一个函数，用来注册一个小程序，接受一个 object 参数。该参数里面指定小程序的生命周期函数。这个有点像 Android 。不知道是特意借鉴的还是怎样，看来生命周期函数对于一个 App 来说是很重要的。

先看看 `onLauch:`，当小程序初始化完成时会被触发，执行里面的函数。全局下只能触发一次，也就是每次打开小程序就会触发一次。触发后先使用 `wx.getStorageSync('logs') || []` 从本地缓存中获取 logs 的数据，如果 logs 有数据的时候，就把数据赋值给变量 logs 。如果没数据或者不存在 logs ，则把变量 logs 初始化为空数组。`logs.unshift(Date.now())`把当前时间插入到数组的第一个元素前，记录每次打开小程序的时间。然后`wx.setStorageSync('logs', logs)`把数组 logs 的内容存储到本地缓存的 logs 中。

将`wx.getStorageSync('logs')` 打印出来可看到里面记录的值

![](http://i4.piimg.com/1949/a8bac6f629f69e05.png)

以数组的形式记录着每次登录时间的时间戳。

再看看最下面的`globalData:`，从名字可以知道这是定义一些全局数据的。这里只有一个数据`userInfo: null`，记录用户信息，初始为空。

`getUserInfo:`由名字可知获取用户信息，此函数中有个形参 `cb`，这里先不管它是什么，继续看下面的代码。有一个 if else 语句，当`this.globalData.userInfo`存在或有值的时候，就执行 if 里面的，否则执行 else 里的语句。这个`this.globalData.userInfo`就是我们刚说的初始化为 null 的用户信息，所以一开始都执行 else 里面的代码。

`wx.login()`有注释，调用登录接口。`success:`表示调用成功后执行的回调函数，与此对应的还有`fail:`调用失败后执行的和`complete:`调用结束后执行的，即成功失败都执行。所以，调用成功后就来这个`wx.getUserInfo()`获取用户信息了，不过这个 API 生效的前提是必须先调用`wx.login()`，获取用户信息函数调用成功后，就把`this.globalData.userInfo`设置为刚登录的用户信息`res.userInfo`。再看看这个`res.userInfo`里到底存储了什么样的用户信息

![](http://i4.piimg.com/1949/47eafa0eaea92ed4.png)

可看出用户信息是 Object 类型的，`avatarUrl`就是用户头像的图片地址，还有名字、语言、国家、省份、城市什么的。

接下来就看这个是什么意思了
```
typeof cb == "function" && cb(this.globalData.userInfo)
```
先判断 cb 是不是一个 function 类型，是的话，把刚取得的用户信息作为实参传进函数中，关于使用用户信息的地方由界面可以知道是在 index 这个页面中，再来看看 index.js，最下面那里有这么一段：
```
onLoad: function () {
    console.log('onLoad')
    var that = this
    //调用应用实例的方法获取全局数据
    app.getUserInfo(function(userInfo){
      //更新数据
      that.setData({
        userInfo:userInfo
      })
    })
  }
```
在页面加载的时候调用`app.getUserInfo(function(userInfo){...})`来获取全局数据，没错，这个语句就是在 app.js 中定义的用来获取用户信息的那一段，参数 cb 就是这里的函数
```
function(userInfo){
      //更新数据
      that.setData({
        userInfo:userInfo
      })
    }
```
结合 index.js 前面设置的
```
data: {
    motto: 'Hello World',
    userInfo: {}
  }
```
这个函数是用来设置`userInfo`的数据的，`userInfo:userInfo`中前一个 userInfo 是 data 里的 userInfo ，后一个是传进来的参数 userInfo。再看回 app,js 里的`cb(this.globalData.userInfo)`，嗯，这里就是把用户信息传进去的。有种绕了几圈的感觉哈哈。

**index.wxml**
```
<!--index.wxml-->
<view class="container">
  <view  bindtap="bindViewTap" class="userinfo">
    <image class="userinfo-avatar" src="{{userInfo.avatarUrl}}" background-size="cover"></image>
    <text class="userinfo-nickname">{{userInfo.nickName}}</text>
  </view>
  <view class="usermotto">
    <text class="user-motto">{{motto}}</text>
  </view>
</view>
```
嗯，可看出`{{userInfo.xxx}}`就是使用用户信息了，这个`bindtap="bindViewTap"`用来绑定事件，当点击头像或者名字的时候就会触发事件执行 index.js 中设置的事件处理函数 bindViewTap
```
bindViewTap: function() {
    wx.navigateTo({
      url: '../logs/logs'
    })
  }
```
`wx.navigateTo()`用来跳转页面，跳转到日志页面 logs 里去

![](http://i4.piimg.com/1949/37a933d40e958a23.png)

**logs.js**
```
var util = require('../../utils/util.js')
Page({
  data: {
    logs: []
  },
  onLoad: function () {
    this.setData({
      logs: (wx.getStorageSync('logs') || []).map(function (log) {
        return util.formatTime(new Date(log))
      })
    })
  }
})
```
开头引入 util.js 。util.js 是用来把时间戳转化为我们能读懂的时间格式，具体实现这里就不深究了。logs 页面加载时从本地缓存中取得日志信息，然后用 map 方法加 util 里的转换格式方法，把数组里的时间戳都转换为可阅读的时间格式。查看 logs 数组信息如下：

![](http://i2.muimg.com/1949/379d2b8bed6b43b5.png)

大概整个 demo 就是这么多内容了，希望小程序能够越做越好，被大众所重视。