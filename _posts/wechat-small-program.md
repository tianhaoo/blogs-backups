---
title: wechat-small-program
date: 2018-01-25 23:03:15
categories:
- 后端
- 前端
- 微信小程序
tags:
  - wechat
---

微信小程序学习笔记



## 注册开发者账号，获取APPID，下载微信小程序开发环境

小程序页面设计基本上也是遵循 MVC 结构进行构建。

## 配置小程序

### app.json 小程序配置，比如导航、窗口、页面http请求跳转等

其实这里，共有五个部分可以配置，分别是pages, window, tabBar, networkTimeout和debug
<!--more-->
#### pages：定义的是这个小程序由哪些页面组成。

pages是一个数组，而数组的第一项就是小程序的初始页面

#### window: 定义的是窗口的配置信息。

用于设置小程序的状态栏、导航条、标题、窗口背景色。

navigationBarBackgroundColor    HexColor    #000000 导航栏背景颜色，如"#000000"
navigationBarTextStyle  String  white   导航栏标题颜色，仅支持 black/white
navigationBarTitleText  String  a   导航栏标题文字内容
backgroundColor HexColor    #ffffff 窗口的背景色
backgroundTextStyle String  dark    下拉背景字体、loading 图的样式，仅支持 dark/light
enablePullDownRefresh   Boolean false   是否开启下拉刷新

#### tabBar

通过 tabBar 配置项指定 tab 栏的表现，以及 tab 切换时显示的对应页面。
tabBar 配置数组，只能配置最少2个、最多5个 tab，tab 按数组的顺序排序。

#### networkTimeout

可以设置各种网络请求的超时时间。

### app.js 小程序逻辑，初始化APP


### app.wss 公共样式配置

## 页面

1. 页面由4个文件构成

* js：页面逻辑，相当于控制层（C）；也包括部分的数据（M）
* wxml：页面结构展示，相当于视图层（V）
* wxss：页面样式表，纯前端，用于辅助wxml展示
* json：页面配置，配置一些页面展示的数据，充当部分的模型（M）

## APP() 和 Page()

小程序主要由两个部分构成，app和page。
app，就是小程序的框架。支撑pages，逻辑层的调用，对数据，wxss，wxml的解析；
page，主要是业务层，用于实现界面化操作功能，是程序员使用频率最高的部分。

### APP()
用来注册一个小程序。在小程序启动的时候调用，并创建小程序，直到销毁。在整个小程序的生命周期过程中，它都是存在的。很显然它是单例的，全局的。所以，

**只能在app.js中注册一次。**

**在代码的任何地方都可以通过 getApp() 获取这个唯一的小程序单例**

比如 var appInstance = getApp();

App()的参数是 object 类型 {} ，指定了小程序的声明周期函数。

#### onLaunch 函数

监听小程序初始化。当小程序初始化完成时，会触发 onLaunch（全局只触发一次）.

```
onLaunch: function() {
    //
}
```

#### onShow 函数

监听小程序显示.当小程序启动，或从后台进入前台显示，会触发。

```
onShow: function() {
    //
}
```

#### onHide 函数  

监听小程序隐藏。当小程序从前台进入后台，会触发。所谓前后台的定义，类似于手机上的app，比如当不在使用微信时，就进入了后台。

```
onHide: function() {
    //
}
```

#### globalData 对象

全局数据。

```
globalData: {
    userInfo:{username:'jack'}
}
```

### Page()

通过App()注册完成小程序之后，框架就开始注册页面。所以不要在App()的 onLaunch 中调用 getCurrentPage() 方法，因为此时页面还没有注册完成。

同样的Page()也是有生命周期的。当页面注册完成之后，可以在 page.js 文件中调用 getCurrentPage() 方法，获取当前页面对象。

#### onLoad  

监听页面加载，页面刚开始加载的时候触发。只会调用一次。

#### onReady

监听页面初次渲染完成，类似于html的 onReady。只会调用一次。

#### onShow

监听页面显示，比如页面切换

#### onHide

监听页面隐藏

#### onUnload

监听页面卸载

#### onPullDownRefresh   

监听用户下拉动

* 需要在config的window选项中开启enablePullDownRefresh。
* 当处理完数据刷新后，wx.stopPullDownRefresh 可以停止当前页面的下拉刷新。

#### onReachBottom  

页面上拉触底事件的处理函数

#### data

页面的初始数据


。。。理论太枯燥了，下面是微信小程序实现一个简单的留言板
page:bbs.js

bbs.js
```javascript
// pages/bbs/bbs.js

var app = getApp();
Page({

  /**
   * 页面的初始数据
   */
  data: {
    msgData: [],
  },

  changeInputValue(ev){
    this.setData({
      inputVal: ev.detail.value
    })
  },

  // 删除留言
  DelMsg(ev) {
    var n = ev.target.dataset.index;

    var list = this.data.msgData;
    list.splice(n, 1)

    this.setData({
      msgData:list
    });

  },

  // 添加留言
  addMsg() {

    // console.log(this.data);
    var list  = this.data.msgData;
    list.push({
      msg:this.data.inputVal
    });
    console.log(list);
    // 更新
    this.setData({
      msgData:list,
      inputVal:'',
    });
  },
})
```


bbs.wxml
```
<!--pages/bbs/bbs.wxml-->
<view class="msg-box">
<!--留言-->
<view class="send-box">
      <input bindinput="changeInputValue" class="input" type="text" value="{{inputVal}}" placeholder="请输入留言……" placeholder-class="place-input"/>
      <button size="mini" type="primary" bindtap="addMsg">添加</button>
</view>
   <!--留言列表-->
   <text class="msg-info" wx:if="{{msgData.length==0}}">暂无留言……^_^</text>
    <view class="list-view">
       <view class="item" wx:for="{{msgData}}" wx:key="{{index}}">
        <text class="text1">第{{index}}条留言:</text>
        <text class="text1">{{item.msg}}</text>
        <!--button size="mini" plain class="close-btn" type="default">删除</button-->
        <icon type="cancel" bindtap="DelMsg" data-index="{{index}}" class="close-btn" />
       </view>
    </view>
</view>
```

bbs.wxml
```
/* pages/bbs/bbs.wxss */

/**index.wxss**/
.msg-box{
  padding: 20px;
}
.send-box{
  display: flex;
}
.input{
  border: 1px solid #B0C4DE;
  padding: 5px;
}
.msg-info{
  display: block;
  margin: 10px 0 0 0 ;
  color: #339900;

}
.place-input{
  color: salmon;
}
.list-view{
  margin: 20px 0 0 0;
}
.item{
  overflow: hidden;
  border-bottom: 1px dashed #87CEFF;
  height: 30px;
  line-height: 30px;
}
.text1{
  float: left;
}
.close-btn{
  float: right;
  margin: 5px 5px 0 0;
}
```
