---
layout: post
title:  从socket到自定义web框架
category: 
-  html
- css

---

* content
{:toc}



本文讨论一下怎么用python做个简易的web框架

## web框架的基础-socket

网络上的两个程序通过一个双向的通信连接实现数据的交换，这个连接的一端称为一个socket。
Socket的英文原义是“孔”或“插座”。作为BSD UNIX的进程通信机制，取后一种意思。通常也称作"套接字"，用于描述IP地址和端口，是一个通信链的句柄，可以用来实现不同虚拟机或不同计算机之间的通信。在Internet上的主机一般运行了多个服务软件，同时提供几种服务。每种服务都打开一个Socket，并绑定到一个端口上，不同的端口对应于不同的服务。Socket正如其英文原意那样，像一个多孔插座。一台主机犹如布满各种插座的房间，每个插座有一个编号，有的插座提供220伏交流电， 有的提供110伏交流电，有的则提供有线电视节目。 客户软件将插头插到不同编号的插座，就可以得到不同的服务

![](http://7xio9f.com1.z0.glb.clouddn.com/socketsocker-sever.jpg)

根据连接启动的方式以及本地套接字要连接的目标，套接字之间的连接过程可以分为三个步骤：服务器监听，客户端请求，连接确认。
（1）服务器监听：是服务器端套接字并不定位具体的客户端套接字，而是处于等待连接的状态，实时监控网络状态。
（2）客户端请求：是指由客户端的套接字提出连接请求，要连接的目标是服务器端的套接字。为此，客户端的套接字必须首先描述它要连接的服务器的套接字，指出服务器端套接字的地址和端口号，然后就向服务器端套接字提出连接请求。
（3）连接确认：是指当服务器端套接字监听到或者说接收到客户端套接字的连接请求，它就响应客户端套接字的请求，建立一个新的线程，把服务器端套接字的描述发给客户端，一旦客户端确认了此描述，连接就建立好了。而服务器端套接字继续处于监听状态，继续接收其他客户端套接字的连接请求。 

所以说：socket是一个通信的桥梁，连接了服务端与客户端。

![](http://7xio9f.com1.z0.glb.clouddn.com/socketsocket-useful.jpg)

如果通过python的方式来实现socket通信流程，大概是这个样子：  

```python
#!/usr/bin/env python
#coding:utf-8
  
import socket
  
def handle_request(client):
    buf = client.recv(1024)
    client.send("HTTP/1.1 200 OK\r\n\r\n")
    client.send("Hello, Seven")
  
def main():
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.bind(('localhost',8000))
    sock.listen(5)
  
    while True:
        connection, address = sock.accept()
        handle_request(connection)
        connection.close()
  
if __name__ == '__main__':
    main()
```

## 什么是web框架

“Web 框架”，其实是建立 web 应用的一种方式。



## 框架的标准-wsgi


## 用python实现简单web框架


## 搭配模板才更美


## 参考

- [socket的百度百科](http://baike.baidu.com/subview/13870/15994413.htm)  
- [详细解释socket](http://www.cnblogs.com/dolphinX/p/3460545.html)  
- [Socket通信原理和实践](http://blog.csdn.net/dlutbrucezhang/article/details/8577810)


-------------
最后来个视频吧，墙外面的世界很精彩！

<iframe width="560" height="315" src="https://www.youtube.com/embed/g0gNWGg2JxM" frameborder="0" allowfullscreen></iframe>

