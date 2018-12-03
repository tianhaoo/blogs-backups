---
title: ubuntu18
date: 2018-04-27 19:26:41
categories:
- 日常
tags:
    - linux
---
# ubuntu18环境搭建

## 科学上网

### chrome

```bash
dpkg -i xxx.deb
```

## shadowsocks

### 安装图像化界面客户端

```bash
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

这种方法已经不能在ubuntu18上面使用，索性使用命令行版本的

<!--more-->

### 安装命令行模式的

```bash
sudo apt-get install python-pip
sudo pip install shadowsocks
```

### 有两种方式运行本地ss

1. 直接在命令的参数里指定配置信息

    ```bash
    sslocal -s 1.1.1.1 -p 8388 -k "your passwd" -b 127.0.0.1 -l 1080
    ```

    -s后面跟你的服务器ip ， -p后面跟你远程端口号（默认8388） ，-k后面跟你的密码（写在双引号之间），其他的用默认选项就好（想改的参见帮助文档）

2. 通过配置文件指定配置信息

    ```bash
    mkdir ~/.shadowsocks
    cd .shadowsocks
    touch ss.json

    {
        "server":"1.1.1.1",
        "server_port":8388,
        "local_address": "127.0.0.1",
        "local_port":1080,
        "password":"your passwd",
        "timeout":300,
        "method":"aes-256-cfb"
    }

    ```

    其中，server填你的服务器ip，sever_port填远程端口号，local_address本地ip，local_part本地端口，password填密码，timeout是延迟时间，method是加密方式，按照实际情况填写并保存

    ```bash
    sslocal -c ~/.shadowsocks/ss.json
    ```

    后台运行

    ```bash
    sslocal -c ~/.shadowsocks/ss.json -d start
    ```

    [报错](https://blog.csdn.net/blackfrog_unique/article/details/60320737)

    这个问题是由于在openssl1.1.0版本中，废弃了EVP_CIPHER_CTX_cleanup函数

    (完美解决报错，给博主点赞！）

### 配置浏览器代理

我使用chrome的SwitchyOmega插件来实现

### 配置代理模式

1. 全局模式

    系统设置 >> 网络 >> 网络代理 >> 方法 >> 手动

    然后将Socks主机的ip和端口填好，如图，然后点击应用到整个系统

2. pac模式

    在配置URL处填写file:// 后面跟你的pac文件路径，如图，然后点击应用到整个系统

    [参考](https://www.cnblogs.com/Dumblidor/p/5450248.html)

## 美化

### 安装gnome-tweak-tool

```bash
sudo apt install gnome-tweak-tool
```

### 更换gnome里面的各种主题

* [Arc](https://github.com/horst3180/arc-theme)
* [Papirus](https://launchpad.net/~papirus/+archive/ubuntu/papirus/+packages?field.name_filter=papirus-icon-theme)

### 安装gnome插件

方法很多，推荐使用chrome插件安装，方法如下

1. 安装GNOME Shell integration
2. `sudo apt-get install chrome-gnome-shell`

我觉得好用的插件如下

* Dash To Dock 就是一个dock
* Clipboard Indicator 可以复制粘贴的历史保存下来，很方便
* Drop Down Terminal 它可以通过设定的快捷键（默认为tab键上面的键），随时呼出命令行。
* Coverflow Alt-Tab 让Alt+tab切换更美观

## 同步hexo博客

1. 安装nodejs和npm和n

    ```bash
    sudo apt install nodejs
    sudo apt install npm
    sudo npm install -g n
    ```

    ```bash
    git clone https://github.com/tianhaoo/tianhaoo.github.io.git blog
    cd blog
    sudo npm -g install hexo-cli
    sudo npm install hexo
    sudo npm install hexo-util
    sudo npm install hexo-deployer-git
    ```

2. 然后将公钥放在github的仓库上，就完成了博客的部署了，一切都和以前一样

## oh-my-zsh

[参考](https://blog.csdn.net/u010138906/article/details/78778627)

### 安装

```bash
sudo apt install zsh
wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | sh
chsh -s /bin/zsh tian
```

### 主题

[主题](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)推荐

* agnoster
* ys
* avit
* blinks

### 插件

* zsh-autosuggestions
* zsh-syntax-highlighting

### 给zsh添加powerline

[官网链接](https://powerline.readthedocs.io/en/latest/usage/shell-prompts.html#zsh-prompt)
（觉得添加了还不如原生的ys主题好看)

## vim

Spacevim不好用，打算自己写自己的vimrc

### 用到的插件

* vundle
* nerd-tree
* vim-markdown
* powerline
