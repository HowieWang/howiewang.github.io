---
layout: post
title:  "Python随身听基础篇02-查找帮助与基本类型"
date:   2015-10-25 20:06:05
categories:  
- python
- python随身听
excerpt:  Python随身听基础篇02-查找帮助与基本类型
stickie: false
---

* content
 {:toc}

## 介绍

本文是《Python随身听》基础篇02的文字稿，查找帮助与基本类型。

>《Python随身听》是由第8哥（De8ug）录制的一档Python节目。致力于带给你全新的python学习和复习体验，打造一门听着就能学会python的空中课堂。

点击链接，立即收听。

- 喜马拉雅：
<object type="application/x-shockwave-flash" id="ximalaya_player" data="http://www.ximalaya.com/swf/sound/red.swf?id=54359085" width="340" height="36"></object>

- 网易云音乐：
[https://music.163.com/#/program?id=909955987](https://music.163.com/#/program?id=909955987)

<embed src="//music.163.com/style/swf/widget.swf?sid=909955987&type=3&auto=1&width=320&height=66" width="340" height="86"  allowNetworking="all">


正式开始本期节目之前，我假设你已经在自己的电脑上安装好了python环境。而且我强烈推荐你安装的是python的虚拟环境，如果你还没有安装，可以到我的简书上，搜索“小灶时间 虚拟环境”找到文章，去进行安装。

现在，打开一个控制台环境，敲上`python`，开始练习吧！


## python帮助指南

python中查找帮助非常的简单，在python的控制台中，敲一个`help`就可以了。   

这时候，其实python会给你返回一句话

```bash
Type help() for interactive help, or help(object) for help about object.
```

这句话，就是说，要想寻求帮助，需要在`help`后面加上括号，来执行`help`程序，或者在括号里加上要查询的对象，来查找帮助。

你应该早就听说过，python是面向对象语言，处处皆对象，简单理解，你可以在help后面的括号里，写入任何你想查询的python提供的库名或方法名。至于**面向对象，后面会单独有一期节目来详细解释**。

现在，请在控制台输入：help()

python会带你进入交互式的帮助页面，这时候输入int，就会看见int的帮助，输入str就是str的帮助信息。
这是一个帮助主题页面，显示的是当前类的用途和各个函数的使用方法。

**在每一个主题的帮助页面，用空格翻页，如果想退出，就直接输入q**

示例如下：

```bash
help()
help>int 
class int(object)
 |  int(x=0) -> integer
 |  int(x, base=10) -> integer
 ...
 (空格翻页，输入q退出当前帮助)
help>str
Help on class str in module builtins:

class str(object)
 |  str(object='') -> str
 |  str(bytes_or_buffer[, encoding[, errors]]) -> str
...
(空格翻页，输入q退出当前帮助)
help> q

You are now leaving help and returning to the Python interpreter.
If you want to ask for help on a particular object directly from the
interpreter, you can type "help(object)".  Executing "help('string')"
has the same effect as typing a particular string at the help> prompt.
>>>
```

退回到python控制台，在`>>>`后面，直接输入help(int)，会直接进入int的帮助页面。

```bash
>>> help(int)
(空格翻页，输入q退出当前帮助)
(查询完毕，用下面语句退出python控制台)
>>> exit() 
```

现在，你已经会查看任何帮助了，可以继续往下看。关于帮助，后面的节目也会经常帮你复习，这是你学习的好帮手。

下面我们看基本数据类型。

## 基本数据类型

首先，我们说说为什么要有基本数据类型。

数据类型是为了满足描述不同事物的不同需求。比如数字的整数，小数，说话写字的字符串，很多事情需要判断对错，每天都在流失的时间等等。不同的数据需要的计算机存储空间不同。  

具体来说：

- 整型int   -> 1，2，3.  
- 浮点float -> 2.1, 3.5, 5.0  
- 字符串str  -> “你好”，'hello',需要单引号或双引号
- 布尔类型bool      -> True, False
- datetime  -> 时间，以后节目具体讲
- None      -> 空类型，没有值                            

对于整型和浮点型，可以进行加减乘除，指数等数学计算。
对于字符串，可以进行长度计算，字符串拼接，大小写转换等。

介绍完数据类型，我们还需要了解一下变量的概念，然后再进行练习。

## 变量与命名

所谓变量，就是用一个东西去代表一些数字或字符串等，去执行各种操作。你需要通过变量去内存中占领一块地盘，进行各种数据运算。

python允许使用任何的字母与数字的组合表示变量，甚至特殊字符，比如汉字。但是，超级不建议你使用汉字当变量。

简单命名规则如下：
- 变量不能用数字开头
- 不能是纯数字
- 可以用下划线连接。


## 练习时间

下面请打开python控制台，进行练习。

```python
>>> 1 # 输入数字，直接返回
1
>>> 2+3 # 可以当作计算器使用
5
>>> 4.6+3.8
8.399999999999999
>>> 'hello' # 输入字符串
'hello'
>>> "晚上好" # 双引号，单引号都可以
'晚上好'
>>> True # 布尔类型，注意开头字母大写
True
>>> False
False
>>> a-d=1 # 不能直接写计算式，这也说明，不能用-当作变量命名
  File "<stdin>", line 1
SyntaxError: can't assign to operator
>>> a="hi" # 变量命名，内存中开辟一个空间a，保存的值“hi”，后续操作用a指代“hi”
>>> a
'hi'
>>> a_b="hello" # 变量命名，可以用下划线
>>> a_b
'hello'
>>> a + a_b # 字符串可以相加
'hihello'
>>> len(a) # 求长度
2
>>> type(a) # 查看类型
<class 'str'>
>>> type(2)
<class 'int'> 
>>> a. # 变量名后面，敲一个. 然后按tab键，就可以查看可以使用哪些函数
a.__add__(           a.__getnewargs__(    a.__new__(           a.casefold(          a.isalpha(           a.ljust(             a.rstrip(
a.__class__(         a.__gt__(            a.__reduce__(        a.center(            a.isdecimal(         a.lower(             a.split(
a.__contains__(      a.__hash__(          a.__reduce_ex__(     a.count(             a.isdigit(           a.lstrip(            a.splitlines(
>>> a.upper() # 大写
'HI'
>>> a.lower() # 小写
'hi'
>>> a.startswith('a') # 是否以a开头
False
>>> a.startswith('h')
True

>>> help(a.center) # 查看具体某个方法的帮助
center(...) method of builtins.str instance
    S.center(width[, fillchar]) -> str

    Return S centered in a string of length width.
(输入q退出帮助页面)
>>> a.center(6, '*') # 根据帮助提示，第一个参数为最终字符串宽度，第二个为用什么字符填充
'**hi**'
（更多方法的使用请自己查看帮助，后续节目中也会在遇到时候进行讲解）
```

## 总结

今天我们学习了如下知识点：

- 帮助方法
- 常用基本数据类型
- 变量

请打开python控制台一起练习。有任何问题或建议欢迎留言。


python随身听，程序任我行。下一期，我们将一起学习python中常用的内置数据结构，字典，列表等的使用，敬请关注！