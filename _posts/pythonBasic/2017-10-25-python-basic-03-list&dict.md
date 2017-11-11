---
layout: post
title:  "Python随身听基础篇03-基本数据结构list和dict"
date:   2017-10-25 20:06:05
categories:  
- pythonBasic
- python随身听
excerpt:  Python随身听基础篇03-基本数据结构list和dict
stickie: true
---

* content
 {:toc}

## 介绍

本文是《Python随身听》基础篇03的文字稿，基本数据结构list和dict。

>《Python随身听》是由第8哥（De8ug）录制的一档Python节目。致力于带给你全新的python学习和复习体验，打造一门听着就能学会python的空中课堂。

点击链接，立即收听。

- 喜马拉雅：
<object type="application/x-shockwave-flash" id="ximalaya_player" data="http://www.ximalaya.com/swf/sound/red.swf?id=54359085" width="340" height="36"></object>

- 网易云音乐：
[https://music.163.com/#/program?id=909955987](https://music.163.com/#/program?id=909955987)

<embed src="//music.163.com/style/swf/widget.swf?sid=909955987&type=3&auto=1&width=320&height=66" width="340" height="86"  allowNetworking="all">

list和dict，顾名思义，就是列表和字典，这是python中非常基础的数据结构，也是非常重要且用途最广的数据结构，所以我把它们放在数据结构的第一节来介绍。

## 字典list就是一串糖葫芦

list是python的一种内置数据结构，你把它想象成一串糖葫芦就好了。python提前设定好了list的特点和一些固定操作（或方法或函数），计算机知道该怎么进行具体操作。

list用中括号表示，里面的值用英文的都好分开。


### 特点与操作

list有这么几个简单的特点：

- 长度不固定，就像糖葫芦可以串8个，串10个，当然也有奸商可能就串6个，甚至4个
- 类型不固定，糖葫芦可以串山楂，黑枣，橘子等等，list里面也可以放字符串，数字，设置列表等等
- 可以修改，就好像制作糖葫芦，你本来要串个山楂，结果串了个橘子，可以把对应的那个错的修改了就好啦。

list可以做这样的几个操作：

- `append`: 追加元素，相当于给糖葫芦一个个的串山楂
- `+,extend`: 多个列表相加， 相当于把两串糖葫芦连在一起
- `insert`: 向指定位置插入一个值，假如要给糖葫芦的第二个位置串一个黑枣
- `remove, pop`: 移除某一个元素，糖葫芦中间某个坏了，给剔除
- `a in list_b`: 检查某个元素是否存在，检查一个纯山楂的糖葫芦里是不是有黑枣
- `sort`: 排序，这个糖葫芦比较难办，假装要一串山楂从小到大排列的糖葫芦吧！
- `[:]`: 切片操作，假如你吃糖葫芦时候，就喜欢吃第2个到第5个，该怎么啃？

### 代码演示

```python
>>> help(list)

>>> dir(list)
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
>>> hulu1 = ['zha','zha','zha','zha']
>>> hulu2 = [1,2,3,4,5,6,7]
>>> hulu3 = []
>>> hulu3 = hulu1+hulu2
>>> hulu3
['zha', 'zha', 'zha', 'zha', 1, 2, 3, 4, 5, 6, 7]
>>> hulu3.extend(hulu1)
>>> hulu3
['zha', 'zha', 'zha', 'zha', 1, 2, 3, 4, 5, 6, 7, 'zha', 'zha', 'zha', 'zha']
>>> hulu1.append('zao')
>>> hulu1.append('zao')
>>> hulu1.append('zao')
>>> hulu1
['zha', 'zha', 'zha', 'zha', 'zao', 'zao', 'zao']
>>> len(hulu1)
7
>>> len(hulu3)
15
>>> hulu1.insert(3, 'xxx')
>>> hulu1
['zha', 'zha', 'zha', 'xxx', 'zha', 'zao', 'zao', 'zao']
>>> hulu1.remove(3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
>>> hulu1.remove('xxx')
>>> hulu1
['zha', 'zha', 'zha', 'zha', 'zao', 'zao', 'zao']
>>> hulu1.pop()
'zao'
>>> hulu1
['zha', 'zha', 'zha', 'zha', 'zao', 'zao']
>>> hulu1.pop()
'zao'
>>> hulu1
['zha', 'zha', 'zha', 'zha', 'zao']
>>> hulu1.insert(2, 'yyy')
>>> hulu1
['zha', 'zha', 'yyy', 'zha', 'zha', 'zao']
>>> hulu1.pop(2)
'yyy'
>>> hulu1
['zha', 'zha', 'zha', 'zha', 'zao']
>>> y in hulu1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'y' is not defined
>>> 'yyy' in hulu1
False
>>> 'zha' in hulu1
True
>>> hulu1
['zha', 'zha', 'zha', 'zha', 'zao']
>>> hulu1.sort()
>>> hulu1
['zao', 'zha', 'zha', 'zha', 'zha']
>>> s=[5,2,6,8,34,7,8]
>>> s.sort()
>>> s
[2, 5, 6, 7, 8, 8, 34]
>>> s=[5,2,6,8,34,7,8,'a1','b5', 'a6','b2','a3']
>>> s.sort()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() < int()
>>> s
[2, 5, 6, 7, 8, 8, 34, 'a1', 'b5', 'a6', 'b2', 'a3']
>>> s=[5,2,6,8,34,7,8,'am','bw', 'af','bs','an']
>>> s.sort()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unorderable types: str() < int()
>>> s=['am','bw', 'af','bs','an']
>>> s.sort()
>>> s
['af', 'am', 'an', 'bs', 'bw']
>>> s[1]
'am'
>>> s[3]
'bs'
>>> s[3:]
['bs', 'bw']
>>> s[:2]
['af', 'am']
>>> s[:]
['af', 'am', 'an', 'bs', 'bw']
>>> s[::2]
['af', 'an', 'bw']
>>> s[::-1]
['bw', 'bs', 'an', 'am', 'af']
>>> s='asdfghjk'
>>> s[::-1]
'kjhgfdsa'
>>> s[:5]
'asdfg'
>>> s[3:]
'fghjk'
>>> s[::3]
'afj'
>>>

```

## 字典就是一本字典

python的dict这种数据结构可以直接理解为字典，不管汉语字典还是英语词典，给你一个词，这个词在python的字典里叫做key，中文是键盘的键，用这个词可以找到它相关的意思或者说具体内容，这个内容在python的字典里，叫做key的值或者value，或简单叫作v。

dict用大括号表示，里面用英文的逗号分隔多个组，用冒号隔开每一组的key和v。

### 特点与操作

dict的特点：

- key是不可变的，很简单去理解，查单词的时候，如果查到一半这个词自己变了，那肯定查的就是别的词儿了。这个key的类型可以是int，float，或是string
- 字典默认是无序的。因为字典只是一堆kv的集合。

dict的常用操作：

- `{'k':'v'}, dict(zip(list_k, list_v))`: 创建方法（使用kv或者zip）
- `d['k'],d.get('k')`: 获取元素（直接中括号里用k或get方法）
- `'k' in d`: 检测k是否存在
- `d.keys()`: 获取所有的k
- `d.values()`: 获取所有的值
- `del d['k']`: 删除k
- `d1.update(d2)`: 更新值

### 代码演示

```python
>>> help(dict) # 查看帮助，浏览全局

>>> dir(dict) # 查看有哪些方法
['__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'clear', 'copy', 'fromkeys', 'get', 'items', 'keys', 'pop', 'popitem', 'setdefault', 'update', 'values']
>>> d={'k':2,'x':"ttt"} # 用大括号建立字典
>>> d
{'x': 'ttt', 'k': 2}
>>> list_k=[1,2,'n']
>>> list_v=['hello','python','de8ug']
>>> d=dict(zip(list_k, list_v)) # 用zip和kv的列表建立字典
>>> d
{1: 'hello', 2: 'python', 'n': 'de8ug'}
>>> d['1'] # 用不存在的k会报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: '1'
>>> d[1]
'hello'
>>> d['n']
'de8ug'
>>> d['n'] = 'nb' # 更新key的值
>>> d
{1: 'hello', 2: 'python', 'n': 'nb'} 
>>> d.get('n') # 用get获取
'nb'
>>> d.get('m') # 没有k的时候为空
>>> d.get('m', 'yyy') # 取不到时候设置默认的值
'yyy'
>>> 'm' in d # 判断是否存在k
False
>>> 'n' in d
True
>>> del d['n'] # 删除k
>>> d
{1: 'hello', 2: 'python'}
>>> d['k']='tttt' # 直接添加k和v到字典，k之前可不存在
>>> d
{1: 'hello', 2: 'python', 'k': 'tttt'}
>>> d.keys # keys是方法，调用要加括号
<built-in method keys of dict object at 0x102401d48>
>>> d.keys()
dict_keys([1, 2, 'k'])
>>> d.values()
dict_values(['hello', 'python', 'tttt'])
>>> d2={}
>>> d2[2]='sdfsdf'
>>> d2[k2]="yyyy" # 注意k的取值，一半数字或字符串，否则报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'k2' is not defined
>>> d2[4]='sdfsdf'
>>> d2
{2: 'sdfsdf', 4: 'sdfsdf'}
>>> d2["k2"]="yyyy"
>>> d2
{2: 'sdfsdf', 4: 'sdfsdf', 'k2': 'yyyy'}
>>> d
{1: 'hello', 2: 'python', 'k': 'tttt'}
>>> d.update(d2) # 更新，相同的k会被覆盖
>>> d
{1: 'hello', 2: 'sdfsdf', 4: 'sdfsdf', 'k': 'tttt', 'k2': 'yyyy'}
```

## 总结

今天，主要介绍的字典和列表的概念和使用。上面的示例是远远不够的，需要多加练习，并且多用help去查看dir出来的你不认得的使用方法。

下一期会介绍：元组tuple和集合set，敬请关注！