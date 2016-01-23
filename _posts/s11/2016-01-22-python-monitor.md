---
layout: post
title:  【Howie玩python】-图解python监控
category: 
-   python
- redis

---

* content
{:toc}

今天来讨论一下用python监控服务器集群的一些主要指标该怎么做。需要用到以下知识：

- python类与对象    
- redis原理，订阅与发布        
- python操作redis    
- linux基础    

## 从整体流程图说起

先看图：
**简单版本：**

![](http://7xio9f.com1.z0.glb.clouddn.com/pythonday12-monitor.png)    

**复杂版本**

![](http://7xio9f.com1.z0.glb.clouddn.com/python%E7%9B%91%E6%8E%A7%E6%B5%81%E7%A8%8B%E5%9B%BE%20(1).png)    

从图上我们可以看出，使用python监控服务器主机的各项指标可以分为两大部分：**服务器端，客户端。**

> 服务器端    
主要负责通过各种服务模板生成各个主机所需要的监控信息，然后通过redis的频道发布出去。当模板发生更改的时候，可以第一时间通知到影响的主机。
同时，还可以通过订阅频道得到各个主机发送过来的监控信息。进行分析，警告等操作。

> 客户端      
通过redis的订阅频道和内部配置的插件，计算出来自己的各项指标，然后通过redis发布频道传递给服务器端。



## 代码结构图与uml类图

首先我们看一下代码结构图。

> 客户端      
客户端主要有3个目录，分别用来存放配置信息，主程序，和插件。其中，配置信息包含redis服务ip，端口号，订阅和发布频道。

各个插件都是单独的函数，返回每项服务的一个字典。主程序通过redis的订阅发布频道处理信息。

> 服务器端    


    [root@localhost monitor]# tree
    .
    ├── client
    │   ├── conf   # 配置目录
    │   │   ├── __init__.py
    │   │   └── settings.py   # 配置信息
    │   ├── core   # 核心程序
    │   │   ├── __init__.py
    │   │   ├── main.py  # 主程序
    │   │   └── redishelper.py  # redis订阅发布
    │   ├── __init__.py
    │   ├── MonitorClient.py  # 入口程序
    │   └── plugins  # 插件目录
    │       ├── cpu_mac.py
    │       ├── cpu.py
    │       ├── __init__.py
    │       ├── load.py
    │       ├── memory.py
    │       └── plugin_api.py  # 插件函数入口
    ├── __init__.py

    ├── server
    │   ├── conf  #配置目录
    │   │   ├── hosts.py  # 主机信息
    │   │   ├── __init__.py
    │   │   ├── services   # 服务目录
    │   │   │   ├── generic.py  # 通用服务
    │   │   │   ├── linux.py  # linux需要监控的服务
    │   │   ├── settings.py   #配置
    │   │   ├── templates.py  # 模板，包含所有主机信息
    │   ├── core  # 核心程序目录
    │   │   ├── __init__.py
    │   │   ├── main.py  # 主程序
    │   │   ├── redishelper.py   #redis订阅，发布
    │   │   └── serialize.py  # 序列化
    │   ├── __init__.py
    │   └── MonitorServer.py   # 服务端入口程序



![](http://7xio9f.com1.z0.glb.clouddn.com/pythondiagram-client.png)    


![](http://7xio9f.com1.z0.glb.clouddn.com/pythondiagram-server-all.png)    