---
title: http_proxy
date: 2018-06-06 16:33:51
categories:
- 运维
tags:
    - linux
---

# 给linux设置网络代理，让命令行和apt都走代理

继实验室电脑莫名其妙断网事件后，又有了新的状况，学校内网有台服务器，想在上面部署docker环境，但是学校的主机没有办法连接外网，上次写的登录网关的脚本总是提示 “未从有效位置登录”

于是想到跑一台可以连接外网，也可以连接苏大网的服务器做代理，所有的流量都从这里转发，下面是配置过程，仅作记录。

## 代理服务器配置

### 安装squid3

<!--more-->

我是在Ubuntu18.04 64位环境下使用squid3搭建的代理服务器。squid3是一个主流的可配置的、健壮、低消耗的代理服务器。

```bash
sudo apt-get install squid3
```

### 配置

squid3的配置文件在/etc/squid/squid.conf，我们使用vim编辑器来配置。

```bash
sudo vim /etc/squid3/squid.conf
```

我们在配置文件的末尾加入以下几行：

```bash
# 允许的客户端ip
acl allcomputers src 0.0.0.0/0.0.0.0
# 配置用户名密码，后面会生成passwords文件
auth_param basic program /usr/lib/squid3/basic_ncsa_auth /etc/squid3/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
http_access allow authenticated allcomputers
```

找到http_access deny all并注释掉

```bash
# http_access deny all
```

最好修改一下默认的3128端口，因为这个端口是默认的，很容易被网络上的代理爬虫探测到。

```bash
http_port 8128
```

#### 用户名密码认证

网络上有很多专门爬免认证的代理的爬虫，如果我们自己搭建的代理服务器不加认证的话，会被这些爬虫探测到然后沦为了免费代理。

使用htpasswd来创建passwords文件，htpasswd命令在软件包apache2-utils中。


#### 生成password文件安装squid3

```bash
sudo htpasswd -c -d /etc/squid/passwords 自定义用户名
```

#### 然后输入两次至少8位的密码，会在/etc/squid3/目录下生成passwords文件，要保证该文件是可读的。

```bash
sudo chmod o+r /etc/squid/passwords
```

#### 启动服务，也可以使用restart，stop进行重启和关闭。

```bash
sudo service squid start
```

### 验证代理是否起作用

squid3的访问日志文件在/var/log/squid/access.log

```bash
tail -f /var/log/squid3/access.log
```

#### 另找一台linux机器打开shell，将我们的代理配置上：

```bash
export http_proxy="http://用户名:密码@代理IP:代理端口"
curl -l "http://www.baidu.com"
```

如果代理配置正确，回输出html，同时代理服务器上的access.log会记录这次请求。

## 客户端配置

### 编辑.bashrc

```bash
export http_proxy="http://tian:cssus123@10.10.65.139:8128"
export https_proxy="http://tian:cssus123@10.10.65.139:8128"
```

### 编辑/etc/apt/apt.conf

```bash
Acquire::http::proxy "http://tian:cssus123@10.10.65.139:8128";
Acquire::https::proxy "http://tian:cssus123@10.10.65.139:8128";
```

**需要注意的是，上述方法只支持http代理，连https的支持都要稍微配置一下，更不用提socks4/socks5代理**

## 使用proxychains-ng为程序设置代理

### 简介

linux下代理一般是通过http_proxy和https_proxy这两个环境变量，但是很多软件并不使用这两个变量，导致流量无法走代理。在不使用vpn的前提下，linux并没有转发所有流量的真全局代理。但是可以用proxychains-ng为程序指定走代理，proxychains-ng是proxychains的加强版，主要有以下功能：

* 支持http/https/socks4/socks5
* 支持认证
* 远端dns查询
* 多种代理模式

不足：

* 不支持udp/icmp转发
* 少部分程序和在后台运行的可能无法代理

### 安装和下载源码

```bash
git clone https://github.com/rofl0r/proxychains-ng
```

### 编译和安装  

```bash
./configure --prefix=/usr --sysconfdir=/etc
make 
make install
make install-config
```

### 配置文件在`/etc/proxychains.conf`

添加下面一行
```bash
socks5  127.0.0.1 1080
```

### 添加环境变量

按照以上方法安装的已经自动添加环境变量，无需配置，只需键入proxychains4即可调用

但是这个命令比较长，我们可以设置一个别名。在.zshrc里面

```bash

```

