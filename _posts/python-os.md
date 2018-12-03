---
title: python-os
date: 2018-02-01 20:08:49
categories:
- 日常
tags:
  - python
---


从网上下载了全部的Taylor swift的歌，打算传到qq音乐的网盘里便于随时听歌。可是下载下来的文件是.flat和.mp3和.lrc文件混合在一起的，我不需要.lrc和.jpg甚至.log文件，几百首歌手动分太不geek了，于是想到利用python的os库写一个脚本自动处理。



### 用到的函数

[参考](http://www.runoob.com/python/os-file-methods.html)


* `os.getcwd()`返回当前工作目录

* `os.chdir(path)`改变当前工作目录

* `access()`方法语法格式如下：

* `os.access(path, mode)`
<!--more-->
    参数
    path -- 要用来检测是否有访问权限的路径。

    mode -- mode为F_OK，测试存在的路径，或者它可以是包含R_OK, W_OK和X_OK或者R_OK, W_OK和X_OK其中之一或者更多。

    os.F_OK: 作为access()的mode参数，测试path是否存在。
    os.R_OK: 包含在access()的mode参数中 ， 测试path是否可读。
    os.W_OK 包含在access()的mode参数中 ， 测试path是否可写。
    os.X_OK 包含在access()的mode参数中 ，测试path是否可执行。

* `os.listdir(path)`返回path指定的文件夹包含的文件或文件夹的名字的列表。

* 魔术一般的函数`endswith()`

    语法：

    str.endswith(suffix[, start[, end]])

    参数

    suffix -- 该参数可以是一个字符串或者是一个元素。
    start -- 字符串中的开始位置。
    end -- 字符中结束位置。


### 实操

```python
import os
import shutil

path = "F:/TaylorSwift"


folders = os.listdir(path)
count = 0
for folder in folders:
    print(folder)
    subFiles = os.listdir(path+"/"+folder)
    for subFile in subFiles:

        print(subFile)
        if subFile.endswith('.flac') or subFile.endswith('.mp3'):
            shutil.copy(path+'/'+folder+'/'+subFile, 'F:/TaylorSwiftCopy/'+subFile)
            count += 1

print("一共移动了" + str(count) + "首歌\n\n\n")

```

结果很成功，学到了一点magic的函数，python真好用
