---
layout: post
title:  【Howie玩python】-图解python监控
category: 
-   python
- redis

---

* content
{:toc}

## 从代码结构图开始

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
