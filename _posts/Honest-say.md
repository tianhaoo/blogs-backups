---
title: Honest-say
date: 2018-04-04 14:59:39
categories:
- 教程
tags:
    - andriod
---


## 用手机查看对你坦白说的好友是谁

## 工具

- 安卓手机
- Packat Capture软件
- 手机ＱＱ

## 教程

<!-- more -->

### 下载Packet Capture(后两个要外网)

- [酷安下载](https://www.coolapk.com/apk/app.greyshirts.sslcapture)

- [百度网盘下载](https://pan.baidu.com/s/17qKgl6ESpkrUiZlWmjUS0Q) 密码:wae6

- [官网下载](https://apkpure.com/packet-capture/app.greyshirts.sslcapture)

- [google play下载](https://play.google.com/store/apps/details?id=app.greyshirts.sslcapture&hl=zh_CN)

    
### 安装
#### 选择Install Certificate
<img src="/images/Honest-say/0.png" width="50%" height="50%">
#### 确定安装证书
<img src="/images/Honest-say/1.png" width="50%" height="50%">

### 手机抓包
#### 点击上面带数字一的绿色三角形，开始对某一个应用抓包
<img src="/images/Honest-say/2.png" width="50%" height="50%">
#### 选择QQ
<img src="/images/Honest-say/3.jpeg" width="50%" height="50%">
#### Packet Capture像VPN一样对你所有的流量进行监控，这样才能抓包，确定就好。
<img src="/images/Honest-say/4.jpeg" width="50%" height="50%">
#### 会弹出好多个证书不一致，这是由于Packet Capture正在监控流量造成的，点击继续访问。
<img src="/images/Honest-say/5.jpeg" width="50%" height="50%">
#### 进入到这个页面，这时你想要的信息就已经被Packet Capture获取到了
<img src="/images/Honest-say/6.jpeg" width="50%" height="50%">
#### 返回Packet Capture，点击上方红色的正方形停止抓包。点击第一条对qq的抓包结果，会出现很多信息，我们需要的，给你发坦白说的人的信息就在这里面，需要点进去慢慢找
<img src="/images/Honest-say/7.png" width="50%" height="50%">
#### 经过试验包含坦白说的信息是SSL加密的，而且大小为20KB左右，很好找
<img src="/images/Honest-say/8.jpeg" width="50%" height="50%">
#### 点进去拉到最后，会看到json格式的数据，找到`fromEncodeUin`对应的值，这就是我们要的信息了，复制下来。
<img src="/images/Honest-say/9.jpeg" width="50%" height="50%">

### 解密

我们抓到的是类似这个的信息，相应的fromEncodeUin已经经过加密，可以利用github上的解密方法进行解密。

```
moumoumou: {
  "code": 0,
  "data": {
    "list": [
      {
        "fromNick": "Nickname",
        "fromEncodeUin": "###S1*fwdNKE59owsdf7z",
        "fromFaceUrl": "woman.png",
        "fromGender": 1,
        "toUin": 12345678,
        "toNick": "",
        "topicId": 19,
        "topicName": "你真帅",
        "timestamp": 1522641990
      },
      {
        "fromNick": "Nickname",
        "fromEncodeUin": "*S1*o7csdfg5owCgf9z",
        "fromFaceUrl": "woman.png",
        "fromGender": 1,
        "toUin": 12345678,
        "toNick": "",
        "topicId": 71,
        "topicName": "我要给你生猴子",
        "timestamp": 1522641977
      }
    ],
    "cookie": "CiAQzasdf1QU\u03d",
    "finish": 1
  }
}
```
例如我们得到的加密字符串是`*S1*fwdNKE59owsdf7z`和`*S1*o7csdfg5owCgf9z`，打开[解密网站](https://tai7sy.github.io/honest-say/index.html),将这两个信息复制进去，就可以得到解密后的QQ号了。


<img src="/images/Honest-say/10.jpeg" width="50%" height="50%">

参考:

[@Tai7sy](https://github.com/Tai7sy/honest-say)




