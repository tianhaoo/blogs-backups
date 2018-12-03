---
title: ubuntu-开机启动
date: 2018-04-28 13:23:34
categories:
- 运维
tags:
	- linux
---

1. 创建软链接

```
ln -fs /lib/systemd/system/rc-local.service /etc/systemd/system/rc-local.servicezo
```
2. 编辑`rc-local.service`

```
cd /etc/systemd/system/
vim rc-local.service
```

在行尾添加

```
[Install]
WantedBy=multi-user.target
Alias=rc-local.service
```

<!--more-->

3. 新建rc.local文件

```
touch /etc/rc.local
chmod 755 /etc/rc.local
```
4. 编辑rc.local

```
#!/bin/bash -e  
#  
# rc.local  
#  
# This script is executed at the end of each multiuser runlevel.  
# Make sure that the script will "exit 0" on success or any other  
# value on error.  
#  
# In order to enable or disable this script just change the execution  
# bits.  
#  
# By default this script does nothing.  
    

# ShadowSocks
sudo sslocal -c ~/.shadowsocks/ss.json -d start


exit 0  
```
