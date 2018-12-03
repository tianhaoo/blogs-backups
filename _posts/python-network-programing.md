---
title: python-network-programing
date: 2018-01-25 15:31:28
categories:
- 教程
tags:
  - python
---

python的网络编程



### TCP客户端

```Python
import socket

target_host ="www.google.com"
target_port = 80

# 建立一个socket对象
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 连接客户端
client.connect((target_host, target_port))

# 发送一些数据
client.send("GET / HTTP/1.1\r\nHost: google.com\r\n\r\n")

# 接受一些数据
response = client.recv(4096)

print(response)

```
<!-- more -->
AF_INET参数说明我们将使用标准的IPV4地址或者主机名，SOCK_STREAM说明这将是一个TCP客户端

###　TCP服务端
```Python
import socket
import threading

bind_ip = "0.0.0.0"
bind_port = 9999

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 绑定服务器需要监听的服务器和端口
server.bind((bind_ip, bind_port))

# 开始监听并将最大连接数设置为5
server.listen(5)

print "[*] LIstening on %s:%d" % (bind_ip, bind_port)

# 这是客户处理线程
def handle_client(client_socket):

    # 打印出客户端发送的内容
    request = client_socket.recv(1024)

    print "[*] Received: %s" % request

    # 返还一个数据包
    client_socket.send("ACK!")

    client_socket.close()

while True:

    client, addr = server.accept()
    print "[*] Accepted connection from: %s:%d" % (addr[0], addr[1])

    # 挂起客户端，处理传入的数据
    client_handler = threading.Tread(target=handle_client, args=(client,))
    client_handler.start()

```

我们将接收到的客户端套接字对象保存到client变量中，将远程连接的细节保存到addr变量中。接着。我们以handle_client函数为回调函数创建了一个新的线程对象，将客户端套接字作为一个句柄传递给它。然后我们启动线程开始处理客户端的连接。handle_client函数执行recv()函数之后将一段信息发送给客户端。

### 取代netcat

```Python

```
