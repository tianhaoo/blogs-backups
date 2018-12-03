---
title: ubuntu-work-environment
date: 2018-01-19 17:08:19
categories:
- 运维
tags:
  - linux
---


记录装Ubuntu后搭建必要的环境的过程

每次装系统之后只要照做就好

再也不用每次都各种搜了



### 切换阿里云的源
```
http://mirrors.aliyun.com/ubuntu
```

### 卸载一些没用的软件
<!-- more -->

```
sudo apt remove libreoffice-common
sudo apt remove unity-webapps-common
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot
sudo apt-get remove gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install
sudo apt-get remove onboard deja-dup

```
然后升级

```
sudo apt update
sudo apt upgrade
```



### 美化

* unity-teak-tool

```
sudo apt install unity-tweak-tool
```

* flatabulous themes and icons

```
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install flatabulous-theme
sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ultra-flat-icons
```

然后更换为相应主题和图标

* dock

```
sudo apt-get install docky
```

千万别安装cairo-dock,难看就算了，还是个流氓软件，卸载都卸载不掉

### 安装搜狗输入法

Linux版的搜狗输入法和Fcitx有冲突，在安装前移除fcitx：
```
sudo apt remove fcitx*

sudo apt autoremove
```


* 然后是在[官网](http://pinyin.sogou.com/linux/?r=pinyin)下载64位deb包

安装
```
dpkg -i xxx.deb

sudo apt -f install
```

安装完成之后，注销并重新登录


### 安装spacevim

```
curl -sLf https://spacevim.org/install.sh | bash
```
一条命令搞定
自己安装插件在如下配置文件里添加

```
~/.SpaceVim.d/init.vim
```

### 安装fish

```
sudo apt install fish
```
* 更改用户的shell

```
sudo chsh -s /usr/bin/fish [username]
```

### powerline

选择用fish安装powerline插件真是巨坑，网上到处都是其他shell的安装教程，唯独没有fish的，辣鸡fish 的配置文件还得自己创建，直到看到powerline的bindings里面有个fish文件夹才好像明白了要怎么做。

```
pip install powerline-status
```

* 然后查看powerline的安装路径

```
pip show powerline-status
```
记住这个路径,在~/.config/fish/路径下面新建文件config.fish

```
set fish_function_path $fish_function_path "/usr/local/lib/python2.7/dist-packages/powerline/bindings/fish"
powerline-setup
```

把路径替换成powerline的安装路径，把上面的代码复制进去
然后重新登录，就可以在fish里使用powerline了

* 最后是powerline字体的安装

```
sudo apt-get install fonts-powerline
```
### 安装sublime text3

1. 自行下载安装（软件版本较老，不推荐）

* 建立软件安装目录
```
# mkdir /opt
# cd /opt
```

* 下载软件
```
wget http://c758482.r82.cf2.rackcdn.com/sublime_text_3_build_3083_x64.tar.bz2
```

* 解压软件包，绿色软件，解压即用
```
tar jxvf sublime_text_3_build_3059_x64.tar.bz2
```

* 命令下直接运行
```
# cd /opt/sublime_text_3
# ./sublime_text
```

* 创建桌面快捷方式

复制文件

```
# cp /opt/sublime_text_3/sublime_text.desktop /usr/share/applications
```

更改配置文件
```
# vim /usr/share/applications/sublime_text.desktop
// 我的配置如下
// 主要是修改软件可执行文件的路径
[Desktop Entry]
Version=1.0
Type=Application
Name=Sublime Text
GenericName=Text Editor
Comment=Sophisticated text editor for code, markup and prose
Exec=/opt/sublime_text_3/sublime_text %F
Terminal=false
MimeType=text/plain;
Icon=/opt/sublime_text_3/Icon/48x48/sublime-text.png
Categories=TextEditor;Development;
StartupNotify=true
Actions=Window;Document;

[Desktop Action Window]
Name=New Window
Exec=/opt/sublime_text_3/sublime_text -n
OnlyShowIn=Unity;

[Desktop Action Document]
Name=New File
Exec=/opt/sublime_text/sublime_text_3 --command new_file
OnlyShowIn=Unity;
```

* 添加环境变量
```
sudo ln -s /opt/sublime_text_3/sublime_text /usr/bin/subl   # 创建链式文件一定要用绝对路径
```

2. apt安装（经常容易找不到服务器）

* 配置插件

[安装Package Control](https://packagecontrol.io/installation)

### 安装atom

```
sudo add-apt-repository ppa:webupd8team/atom  
sudo apt-get update  
sudo apt-get install atom  
```
### 安装chrome

```
// 官网下载deb包
dpkg - i ×××.deb
```

### 安装shadowsocks客户端
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

浏览器代理pac文件备份
https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
