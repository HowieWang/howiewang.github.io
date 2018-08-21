---
layout: post
title:  "Python攻略-字符串的编码与多个奇特前缀"
date:   2018-08-21
categories:  
- python
- python随身听
excerpt:  Python攻略-字符串的编码与多个奇特前缀
stickie: true
---

* content
 {:toc}

## 前言

在Python中，字符串的使用占有很大的比例。因为我们生活中各种文字，符号，信息的交流在程序里都是字符串的状态。

但有时候，Python里面的字符串不那么听话，经常会弹出各种错误信息，比如：

```
TypeError: a bytes-like object is required, not 'str'

或者

UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb9 in position 0: invalid start byte
```

还有时候，我们还经常在代码里面见到一些带着各种奇葩前缀的字符串，比如

```
b''
u''
r''
f''
```

这些到底都怎么回事呢？下面来讨论一下。

## Python中的字符串

### 概念

概念：str是内置的字符串类型，也就是说Python里面的字符串用str这个类来表示。

作用：用来处理文本类型的数据

表示方式：单引号，双引号，三引号（都是成对的），这里面的三引号强大一些，允许跨行。放在代码里，他们分别是这个样子，这里特别特别注意两点：

- 引号是搭配的成对的。比如单对单，多对多，单对多肯定报错
- 引号嵌套时候，要使用不同引号嵌套，比如单套多，或者多套单，否则电脑就傻傻分不清楚哪些引号是一家子了。

```
In [1]: s='单引号'

In [2]: s="双引号"

In [5]: sss = """三引号"""

In [6]: sss = """三引号
   ...: 换行
   ...: 换换换行
   ...: """

In [7]: s = "这里要嵌套引号'需要不和外面的重复'"
```

### 构造函数

这里我们重点看一下str这个类的构造函数。

```
class str(object='')
class str(object=b'', encoding='utf-8', errors='strict')
```

这个类的作用很简单，就是得到一个字符串对象。表示方法有两种，第一种，直接扔进去一个我们想用的字符串就ok，第二种复杂一点，特别是两个参数很关键，包含任何一个的时候，传入的object应该为类似字节的对象（bytes-like object）。

这时候，好玩的就来了，我们来重点说说bytes-like object


