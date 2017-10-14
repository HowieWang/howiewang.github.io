---
layout: post
title: docker科学上网使用说明
category: 
- web
- docker

tag: 
- yunti
- svn
---

* content
{:toc}

其实科学上网，就下面三步，分分钟搞定。当然，具备一台境外机器是必须的。

## update log

- 或许除了docker版本，其他都已经不好用了。
- 请遵守法律，科学上网。gl

## docker 的安装
CentOS 7系统安装docker很方便。

由于CentOS-Extras源中已内置Docker，读者可以直接使用yum命令进行安装：

    $ sudo yum install -y docker   #**【之后要重启系统，然后再start服务才可以！！！】**
    $ sudo service docker start


其他系统请移步这里：http://howiewang.github.io/2015/08/01/install-docker/
其实貌似大部分直接这一句就搞定了：

运行Docker的安装脚本，如下命令：
    
    $ curl -sSL https://get.docker.com/ | sh


## 使用ss镜像

不会玩docker？ 无所谓，直接运行命令就搞定！  

1）拿到镜像，如下命令：

    docker pull oddrationale/docker-shadowsocks
2）运行镜像，启动Shadowsocks。

    docker run -d -p 2022:2022 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 2022 -k yourpasswd -m aes-256-cfb

简单命令解释：

    run -d 后台运行
     -p 2022:2022 服务器与docker内部端口映射
    oddrationale/docker-shadowsocks docker镜像，如果这个不好用了。联系我。
     -s 0.0.0.0 -p 2022 -k yourpasswd -m aes-256-cfb 镜像内部命令，建立容器用的。

哪些要修改？

**2022 和你自己的密码！**

## 客户端科学上网

客户端下载地址：https://github.com/shadowsocks/shadowsocks-gui。

我猜你下载不顺利，因为这个在亚马逊s3，如果我没记错的话，会特别慢。所以你需要到国内某云端去搜。



--------
`下面的内容主要是以前用的vpn，还是不错的。留着备用。`

##下面简单介绍一下翻墙好帮手-云梯。
>云梯致力于提供专业的 VPN 服务，用我们的技术和热情，为大家打造快速、安全、高效的 VPN 接入及解决方案，满足自由访问互联网、网络传输加密、保护隐私等多种需求。成熟的技术、专业的管理以及真诚的服务，让所有人都能享受开放的互联网。
云梯 VPN - 连接世界的梯子！

<!--more-->

已经用了一个多月了，感谢stormzhang的介绍，目前感觉还不错。家里和公司同时在用。

##* 优点：
- 设置简单
- 速度比较快
- 花费不多，每月最少10元，15G,不看视频的话肯定够用了。

##* 缺点：
- 有时候不太稳定，两种设置方法都要试一遍，现在我是公司和家里各用一种方式。
- 服务器也需要根据网络情况换一下才找到合适的，目前我用的是新加坡一号。
- speedup的脚本有bug，帮助说是开机后，用vpn前运行一下就好，但是实际发现，每次重新开启vpn前，必须运行一下，否则您的ip就飞到国外了，比如新加坡，如果这时候刚巧在看优酷之类的，那么，流量就哗哗的木有了。


good luck！
happy out of wall！