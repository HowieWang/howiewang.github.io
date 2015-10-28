---
layout: post
title: ubuntu12.04 hexo install error
category: help
tags: hexo
---

* content
{:toc}

##ubuntu hexo安装错误问题

主要原因就是nodejs和npm的版本太低啦，看[这里](http://stackoverflow.com/questions/12913141/installing-from-npm-fails)得到解决

<!-- more-->

>I had this issue with npm v1.1.4 (and node v0.6.12), which are the Ubuntu 12.04 repository versions.

>It looks like that version of npm isn't supported any more, updating node (and npm with it) resolved the issue.

>First, uninstall the outdated version (optional, but I think this fixed an issue I was having with global modules not being pathed in).
```
sudo apt-get purge nodejs npm
```

Then install from Chris Lea's repo:

```
sudo apt-get update
sudo apt-get install -y python-software-properties
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

##几个需要转移的文章：
[1](http://www.cnblogs.com/lanxuezaipiao/p/3604533.html?ADUIN=64172633&ADSESSION=1404210633&ADTAG=CLIENT.QQ.5329_.0&ADPUBNO=26349
)
[2](http://lanxuezaipiao.blog.163.com/blog/static/93779965201291710458275/
)
[3](http://lanxuezaipiao.blog.163.com/blog/static/9377996520128141161460/)

