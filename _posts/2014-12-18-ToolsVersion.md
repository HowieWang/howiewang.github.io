---
layout: post
title: vs2012提示 ToolsVersion12.0错误
category: cpp
tags: vs2012
---

* content
{:toc}

###项目文件    
包含 ToolsVersion="12.0" 设置，但此工具集未知或缺失

<!--more-->
编译时候提示这个错误：
>1>项目文件包含 ToolsVersion="12.0" 设置，但此工具集未知或缺失。您可以通过为此工具集安装相应的 .NET Framework 来解决此问题。将项目视为具有 ToolsVersion="4.0" 设置。


今天在编译vs12工程时候一直提示这个错误，编译不过，一开始还以为是.net framework的版本不对，检查了一下，本机win7 64bit默认安装的是.net4.5，于是傻乎乎的下载安装.net4.0，结果人家提示已安装了更高版本。没有办法只能继续找答案。

最后终于找到了。其实是我没有安装vs2013.

**为什么呢？没有安装vs2013怎么会在vs2012编译时候出错？**

答案是微软的proj配置文件这两个编译版本居然差别很大。

打开俩工程的proj文件可以看到，在第二行

* vs2012建立的工程，编译正常


```cpp
<Project DefaultTargets="Build" ToolsVersion="4.0" ...
```


* 出错误的vs2012的工程，默认是这个：
```cpp
<Project DefaultTargets="Build" ToolsVersion="12.0" 
```

这也就找到为啥了，因为这个工程是老大先建立的，他的机器上有vs2013，猜测是默认配置或者他先是vs13建立的工程，后来又在vs12修改的，但是这个默认配置已经是12了，所以就出现了我的问题。现在直接改成

```cpp
<Project DefaultTargets="Build" ToolsVersion="4.0" ...
```

编译正常。

问题答案参考[这里](http://blog.csdn.net/civilman/article/details/40109483),非常感谢！
