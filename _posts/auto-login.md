---
title: auto-login
date: 2018-06-01 12:40:15
categories: 
- 运维
tags:
    - linux
    - shell
---
# ubutnu下自动检测网络连接并自动登录网关的shell脚本

## 实验室有台服务器在连接校园网，偶尔会出现问题需要重新登录网关，写了个shell脚本在后台运行，每隔十分钟ping一次百度，如果ping不通的话就自动登录网关

先贴代码，具体的有空再整理
<!--more-->

```bash
#!/bin/bash
#检测网络连接

#判断输出日志文件是否存在
log=./network.log
if [ ! -f ${log} ]
then
   touch ${log}
fi

# 每隔十分钟判断一次
while [ true ]; do
   echo "checking inernet connecting..."
   # 判断网络是否连接
   ping -c 1 www.baidu.com > /dev/null 2>&1
   if [ $? -eq 0 ];then
      echo `date` good >> ${log}
      echo "connecting is good"
   else
      echo "========================================" >> ${log}
      echo `date` bad >> ${log}
      echo "connecting is bad"
      # 尝试进行网络连接
      echo `date` trying to connect internet... >> ${log}
      echo "trying to connect internet..."
      echo `date` `curl -d "username=20175227015&domain=&password=aG1qMTQ3MTU5&enablemacauth=0" http://a.suda.edu.cn/index.php/index/login` >> ${log}
      echo "=======================================" >> ${log}
   fi
   seconds_left=600
   echo "Please wating for ${seconds_left} s..."
   while [ $seconds_left -gt 0 ];do
      echo -n $seconds_left
      sleep 1
      seconds_left=$(($seconds_left - 1))
      echo -ne "\r     \r" #清除本行文字
   done

done
```

