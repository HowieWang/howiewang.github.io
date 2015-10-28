---
layout: post
title: git使用技巧2-如何使用gogs和scm-manager
tags: git
category: 
- help
---

总结自己的git使用方法。使用gogs和scm-manager管理本地源代码。
<!-- more-->

####使用gogs(go语言搞出来的，高大上的选择)

Gogs(Go Git Service) 是一个基于 Go 语言的自助 Git 服务。

![Demo](http://gogs.qiniudn.com/gogs_demo.gif)

-----

##学习小结：  
- 系统win7 x64
- 环境，已有gogs，git

###安装mysql    
1.下载安装包；  
2.设置环境变量；  
3.启动mysql  
`mysql -h localhost -u root -p`

[mysql下如何执行sql脚本](http://www.2cto.com/database/201303/199367.html)

4.创建gogs的数据库 `\. etc/mysql.sql`

###启动gogs  

####部署模式

**脚本均放置在 scripts 目录，但请在仓库根目录执行它们**

默认支持 3 种方式的启动：  

- 普通：只需执行 ./gogs web  

- Supervisor：  

> 1.启动：./scripts/gogs_supervisord.sh start  
2.停止：./scripts/gogs_supervisord.sh stop  
3.重启：./scripts/gogs_supervisord.sh restart  

然后访问 /install 来完成首次运行的配置工作

官方参考[在这里](http://gogs.io/docs/installation/configuration_and_run.html)

----------------

###使用scm-manager管理

这个更容易一些，直接下载安装就好了。

具体参考如下：  

####配置scm-manager  

在机器上安装java环境，从[http://java.com/zh_CN/download/manual.jsp](http://java.com/zh_CN/download/manual.jsp)选择脱机版下载安装
从[https://bitbucket.org/sdorra/scm-manager/wiki/download](https://bitbucket.org/sdorra/scm-manager/wiki/download)下载最新的scm-server-1.24-app.zip
安装为系统服务：
`scm-server.bat install`

然后在系统服务里设置为自动启动，然后启动服务就可以通过[http://localhost:8080](http://localhost:8080)访问了，默认用户名和密码都是`scmadmin`。
登进系统以后通过Repository Types修改Git中心库存放的路径。
修改默认的管理员用户名或者是其他的一些配置都可以找到%userprofile%\.scm\config目录下的相应xml配置文件来修改，注意要重启scm-server服务。
如果要启用邮件以及提醒，还需要安装插件scm-mail-plugin、 scm-notify-plugin，然后在设置中把Mail相关的SMTP设置填写好。  

如果代码提交要跟Redmine关联，还需要安装插件scm-redmine-plugin，然后在项目信息里的Redmine选项卡配置Redmine地址为http://localhost:3000；
如果要启用更新、自动关闭，还需要scm-manager和redmine使用同样的用户名密码，而且Redmine设置里必须启用REST API；  

然后在提交的时候需要用`git commit –m “(#问题ID) fix `修复内容”来关闭问题，可以用的关键字如fix,fixed等可以自行配置。

####客户端安装  

不管最终是使用那一种客户端[Git Extensions](http://code.google.com/p/gitextensions/)或[TortoiseGit：](http://code.google.com/p/tortoisegit/)，`msysgit`都是必须安装的：从[http://msysgit.github.com/](http://msysgit.github.com/)下载最新的Git-1.8.0-preview20121022.exe安装即可。  

偏向使用命令行的只装msysgit就足够了；如果要使用图形界面可以选择msysgit自带的git gui，或者是另外安装git extensions或tortoisegit；个人感觉git extensions的图形界面比较强大，如果安装它的话一定要安装上KDiff3。  

更便捷的就是与Visual Studio集成了，Git Extensions安装的时候可以选择安装Visual Studio插件集成；另外还有一个工具可以进一步提升便捷性[Git Source Control Provider：](http://gitscc.codeplex.com/)，它依赖于`msysgit`和`git extensions`，可以在官方网站下载安装或者在visual studio扩展管理里面搜索git进行安装。

####Git配置

Git客户端安装好之后最好配置一个全局的用户名及邮箱：
  >    git config --global user.name "Your Name Here"   
      git config --global user.email your_email@youremail.com

这个配置在新建Repository时可能会用到；另外可以通过一下命令来查看配置：
 >      git config --list