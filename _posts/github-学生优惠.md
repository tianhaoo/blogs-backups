---
title: github-学生优惠
date: 2018-01-19 23:56:28
categories:
- 薅羊毛
tags:
  - 学生优惠
  - shadowsocks
---


众所周知，各大公司对在校大学生有各种各样的优惠，如果不好好利用这样的资源无疑是一种浪费。

利用学生身份享受各项服务又称薅羊毛。今天给大家介绍薅github羊毛的方法，主要是获取digital ocean的服务器并用ss搭建自己的翻墙服务器的教程。

Do最低配置是512M内存，20GSSD硬盘，1T月流量，5刀一个月，github的优惠活动是免费送50刀到账户，最低配置可以用10个月。


## 前期准备

1. 注册paypal账户

因为do需要最低充值5刀才能激活账户，激活账户后才能使用优惠码，由于穷学生没有信用卡，我们选择用paypal支付。

去[paypal官网](http://www.paypal.com)注册一个账户，注册过程中会让你绑定一张信用卡，其实有网银的银联的借计卡都可以绑定，比如学校的交通银行借记卡、邮政、农行、建行等等，有了paypal以后还可以方便在海外购物。
<!--more-->
2. 获取edu邮箱

edu邮箱是认证成为在校大学生的重要凭证，凭这个可以享受非常多的优惠，薅非常多的羊毛，像jet brains、microsoft imagine、office、腾讯云、阿里云、github学生包等等，所以没有用过的同学建议按照[这篇教程](http://tieba.baidu.com/p/1797649831)去开通.

3. 申请一个github账号

~~gayhub是全球最大的程序员同性交友平台，没有gayhub账号的程序员不是一个合格的gay~~

进入[github官网](http://github.com) 输入用户名，邮箱（就用edu邮箱就好了），密码，点击sign up for github ，直接跳到第二步，点击finish sign up就行了。这样就注册成功，其他设置可以以后再添加和修改，关于git（分布式版本控制系统）的使用教程我也许会另外写一篇博客吧。

然后去[苏州大学邮件系统](http://mail.suda.edu.cn/)登陆邮箱点击验证链接，点击confirm就激活了账号。

4. 在github上面申请学生包，获取优惠码

打开<https://education.github.com/pack> 点get your pack 进入下一个页面，yes , i’m a student. 需要填写的 只有一个how do you plan to use github?

随便写一个, 比如I'll public my program at github and meet some like-minded ~~gays~~ guys.

然后点submit request 就会出现提示：

Thanks for submitting!

重新打开<https://education.github.com/pack>, 然后点get your pack, 按钮消失，你就可以使用所有github提供的免费的东西了

你可以看到有好多免费的资源，比如.me域名、atom、bitnami之类的，这些我们先不看，找到Digital Ocean图标，点击your offer code, 就会生成自己的优惠码了。

记住这个优惠码，然后看下一步。~~当然不是让你用脑子记住啊摔~~

## VPS基本操作（Digital Ocean为例）

vps是Virtual Private Server 虚拟专用服务器的缩写，可以看作是运行在服务器厂商机房里的，接入互联网的，24小时不关机的一台电脑。在不同的服务器厂商购买和使用vps的方法大同小异。

1. 注册digital ocean并使用优惠码

去Digital Ocean[官网注册](https://m.do.co/c/828052101474) 打开这个链接会显示你是我推荐过来的，注册成功并激活后你的账户里会多出10刀，我当时也是点了别人的邀请链接注册的，这样账户就是65刀，当然你也可以百度DO官网自行注册，不过实际账户就只有55刀。

直接填写email地址和密码，然后点create account，就创建一个账号了，email可以用你自己的常用邮箱。进入你的邮箱,打开do发来的验证邮件 ，点CONFIRM EMAIL验证邮箱。

验证会进入一个界面，下拉到最下面，点左侧的billing，然后再Promo Code中输入你在上一步得到的优惠码，如果没反应的话，你可以先删除最后一个字符，然后再输入这个字符就行了。然后会显示You have received $50.00 in credits!

这时候你账户里有60刀了，但是还不能使用的哦，还有一步激活账户。

2. 支付5美元激活账户

点paynow,就会进入paypal支付，会自动切换到中文，有两种方式付款，一种是用你的paypal账号付款，一种是直接用银行卡或借计卡付款

借计卡支付的是人民币，信用卡支付的是美元，5美元应该是34.61块，paypal要比支付宝安全很多，很容易争议退款。

支付完后账户就能使用了，会有65刀，用5刀的配置可以用23个月，下一步是具体怎样使用。


3. 新建服务器

创建VPS，点左侧的DropLet,然后点右边的create droplet，随便起个名字，一般选第一个最低配置5刀一个月的就够用了，土豪随意（话说我们这里不是薅羊毛么，土豪请走开）

然后选择机房位置，Do现在提供阿姆斯特丹 伦敦 新加坡 纽约 圣弗朗西斯科（旧金山）五个数据中心，其中以旧金山对国内访问最好，其次纽约，新加坡线路并不好。电信推荐旧金山，移动推荐新加坡（听大佬说的，我也不知道为啥）

选择操作系统，下面的教程基于ubuntu，建议安装ubuntu，大佬随意（大佬为什么要看我写的博客，大佬请走开）

最后点create droplet，等上几十秒，就创建成功了。

4. 连接服务器

那么服务器建好之后，怎样去使用它呢，一般使用ssh连接，windows下可以下载类似putty的远程连接程序，我介绍一种类似在linux系统上连接远程服务器的方法，使用git bash的ssh命令连接。

用ssh连接，首先要知道你的初始密码，一般在你创建好服务器的时候会发送到你的邮箱里，如果没有的话，在DO管理页面点击你的服务器，在Access里面有一个reset root password,点击之后就会将重置后的密码发送到你的邮箱里。

先去[官网](https://git-scm.com/download/win)下载64-bit Git for Windows Setup,安装的时候一直点击下一步就可以安装完成了。

安装好git之后，右击桌面，选择git bash here 会出现一个对话框，输入

```
ssh root@服务器的ip地址
```

其中ip地址是你新买的服务器的公网ip,登陆do官网后可以看到，输入上面一段代码后，输入发到你邮箱里的服务器密码，会显示如下内容，说明你已经成功登陆上去了。

```
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

4 packages can be updated.
0 updates are security updates.


Last login: Thu Nov 30 08:25:19 2017 from 180.99.150.28
root@ubuntu-512mb-sfo2-01:~#

```

## 配置shadow socks翻墙服务

1. 配置shadowsocks服务器

登陆成功后，需要对服务器进行简单的配置，其实只有几行代码

更新源

```
sudo apt update
```

安装pip

```
sudo apt install python-pip
```

安装shadow socks

```
sudo pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

[github来源](https://github.com/shadowsocks/shadowsocks/tree/master)

至此shadow socks就安装完成了，接下来对shadow socks进行配置。

新建一个配置文件

```
touch /etc/shadowsocks.json
```

编辑这个配置文件

```
sudo vi /etc/shadowsocks.json
```

在其中粘贴进去如下配置项，先按i选择insert模式，然后用鼠标中键粘贴，然后esc,输入:wq保存并退出。 [更多vi的用法](http://blog.csdn.net/xueziheng/article/details/2048054)

```
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

把server改成自己的ip地址，密码改一下，然后记住这个配置。

输入下面的命令使shadow socks在后台运行

```
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```

[more](https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File)

2. 下载并配置shadow socks客户端

[windows客户端](https://github.com/shadowsocks/shadowsocks-windows/releases/download/4.0.6/Shadowsocks-4.0.6.zip)

[android客户端](https://github.com/shadowsocks/shadowsocks-android/releases/download/v4.2.5/shadowsocks-nightly-4.2.5.apk)

[more](https://github.com/shadowsocks)

按照之前shadowsocks.json里面的配置项配置客户端，要改的基本上只有一个ip和密码。

3. 完成

*打这么多字好累啊* X（
