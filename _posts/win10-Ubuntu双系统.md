---
title: win10-Ubuntu双系统
date: 2018-01-19 23:50:04
categories:
- 日常
tags:
  - linux
  - win10
---

在Win10中装双系统的心路历程

装系统从入门到放弃

那啥，随便装一个凑活着过吧




## Ubuntu引导的win10双系统

* 制作Ubuntu启动盘，选择开机从U盘启动，一路默认就是grub引导的双系统了

优点：简单
缺点：不好看，**Ubuntu被搞坏之后重装比较麻烦**

## win10引导的Ubuntu双系统
<!-- more -->
1. 查看windows启动方式是UEFI还是BIOS
    - 在C:\Windows\Panther文件夹中找到setupact.log文件，用记事本打开，然后搜索Detected Boot Environment
    - 按Win+R打开运行，输入msinfo32，回车查看系统信息。在BIOS模式中如果显示“传统”，表示系统启动方式为Legacy BIOS；如果为UEFI，则显示UEFI。
    - 运行powershell ` Confrim-SecureBootUEFI`

2. 如果是BIOS启动，直接下载easybcd就可以方便的配置好了，但是目前市面上比较新的电脑都是UEFI+GRT启动方式，这也是未来的趋势

3. 如果是UEFI启动。。。没找到。。网上都是使用grub引导方式，安装的过程如下：
    - 首先在BIOS->Boot Mode选择UEFI Only或者both,BIOS->UEFI/Legacy Boot Priority选择uefi first.
    - 给ubuntu系统分好空白硬盘，制作好Ubuntu启动盘，控制面板->不启用快速启动,BIOS->Secure Boot->Disabled.
    - 直接安装，一路默认就好，最后结果是grub引导windows.
    - 编辑grub修改启动顺序和默认启动项。

4. 卸载过程如下。。。本来以为很难，网上搜了搜确实也很难，不过看到一个办法，Ubuntu被搞坏了直接重新安装一个就好了呗，不用卸载啊，再次安装的时候会有个选项是覆盖原先的Ubuntu系统，so-easy。

### 报错

```
the grub-efi-amd64-signed package failed to install into /target/. Without the GRUB bootloader the system will not load
```
* 解决办法

> 换个U盘

没错就是这么玄学，不管你信不信反正我装好了

初步推断是U盘文件格式的问题，看来装系统水还很深啊。

以后再也不敢跟别人说我会装系统了，sad。

### grub美化和设置

###### burg美化

* add a couple of PPAs and then install a few things.

```
sudo add-apt-repository ppa:n-muench/burg -y && sudo add-apt-repository ppa:danielrichter2007/grub-customizer -y && sudo apt-get update && sudo apt-get install burg burg-themes grub-customizer
```

* update

```
sudo update-burg
```

* 预览效果

```
sudo burg-emu
```

* 编辑引导菜单`sudo gedit /boot/burg/burg.cfg`，你可以删除不需要的选项。这一步不是必须的。
如果你要安装新的主题，将主题复制到 /boot/burg/themes/ 目录下，`sudo cp -r fortune /boot/burg/themes/`然后`sudo update-burg`即可。

* 又报错算了我还是卸载吧

```
sudo apt-get remove burg burg-themes burg-emu
```

[Read More](http://howtoubuntu.org/how-to-make-your-dual-boot-better-with-burg)

###### 如果不想折腾可以简单的换个背景就好

* 换背景也好麻烦啊。。以后再说吧

* 编辑grub的启动顺序和默认启动项

```
gedit /etc/default/grub
```
修改对应选项即可
