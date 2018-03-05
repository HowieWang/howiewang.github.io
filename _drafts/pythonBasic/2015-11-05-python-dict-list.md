---
layout: post
title:  【De8ug玩python】-字典与列表
category: 
- python  
---

* content
{:toc}

## 帮助三剑客  

- dir  
- help  
- google

```
>>> dir(list)
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
>>> 
>>> dir(dict)
['__class__', '__cmp__', '__contains__', '__delattr__', '__delitem__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'clear', 'copy', 'fromkeys', 'get', 'has_key', 'items', 'iteritems', 'iterkeys', 'itervalues', 'keys', 'pop', 'popitem', 'setdefault', 'update', 'values', 'viewitems', 'viewkeys', 'viewvalues']
>>> 
```

## 用法举例  

> 练习：元素分类
有如下值集合 [11,22,33,44,55,66,77,88,99,90...]，将所有大于 66 的值保存至字典的第一个key中，将小于 66 的值保存至第二个key的值中。
即： {'k1': 大于66 , 'k2': 小于66}

        values = [11, 22, 33,44,55,66,77,88,99,90]

        my_dict = {}

        for value in  values:
            if value>66:
                if my_dict.has_key('k1'):
                    my_dict['k1'].append(value)
                else:
                    my_dict['k1'] = [value]
            else:
                if my_dict.has_key('k2'):
                    my_dict['k2'].append(value)
                else:
                    my_dict['k2'] = [value]

Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

举个例子，假设要根据同学的名字查找对应的成绩，如果用list实现，需要两个list：

        names = ['Michael', 'Bob', 'Tracy']
        scores = [95, 75, 85]

给定一个名字，要查找对应的成绩，就先要在names中找到对应的位置，再从scores取出对应的成绩，list越长，耗时越长。

如果用dict实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用Python写一个dict如下：

        >>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
        >>> d['Michael']
        95

为什么dict查找速度这么快？因为dict的实现原理和查字典是一样的。假设字典包含了1万个汉字，我们要查某一个字，一个办法是把字典从第一页往后翻，直到找到我们想要的字为止，这种方法就是在list中查找元素的方法，list越大，查找越慢。

第二种方法是先在字典的索引表里（比如部首表）查这个字对应的页码，然后直接翻到该页，找到这个字，无论找哪个字，这种查找速度都非常快，不会随着字典大小的增加而变慢。

dict就是第二种实现方式，给定一个名字，比如'Michael'，dict在内部就可以直接计算出Michael对应的存放成绩的“页码”，也就是95这个数字存放的内存地址，直接取出来，所以速度非常快。

你可以猜到，这种key-value存储方式，在放进去的时候，必须根据key算出value的存放位置，这样，取的时候才能根据key直接拿到value。

把数据放入dict的方法，除了初始化时指定外，还可以通过key放入：

        >>> d['Adam'] = 67
        >>> d['Adam']
        67

由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉：

        >>> d['Jack'] = 90
        >>> d['Jack']
        90
        >>> d['Jack'] = 88
        >>> d['Jack']
        88

如果key不存在，dict就会报错：

        >>> d['Thomas']
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        KeyError: 'Thomas'

要避免key不存在的错误，有两种办法，一是通过in判断key是否存在：

        >>> 'Thomas' in d
        False

二是通过dict提供的get方法，如果key不存在，可以返回None，或者自己指定的value：

        >>> d.get('Thomas')
        >>> d.get('Thomas', -1)
        -1

> 注意：返回None的时候Python的交互式命令行不显示结果。

要删除一个key，用pop(key)方法，对应的value也会从dict中删除：

        >>> d.pop('Bob')
        75
        >>> d
        {'Michael': 95, 'Tracy': 85}

请务必注意，dict内部存放的顺序和key放入的顺序是没有关系的。

和list比较，dict有以下几个特点：

查找和插入的速度极快，不会随着key的增加而增加；
需要占用大量的内存，内存浪费多。
而list相反：

查找和插入的时间随着元素的增加而增加；
占用空间小，浪费内存很少。
所以，dict是用空间来换取时间的一种方法。

dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。

这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key：

        >>> key = [1, 2, 3]
        >>> d[key] = 'a list'
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        TypeError: unhashable type: 'list'

## 实际工程应用    

### 冒泡排序  

先要记住range的用法：  

    >>> help(range)
    Help on built-in function range in module __builtin__:

    range(...)
        range(stop) -> list of integers
        range(start, stop[, step]) -> list of integers
        
        Return a list containing an arithmetic progression of integers.
        range(i, j) returns [i, i+1, i+2, ..., j-1]; start (!) defaults to 0.
        When step is given, it specifies the increment (or decrement).
        For example, range(4) returns [0, 1, 2, 3].  The end point is omitted!
        These are exactly the valid indices for a list of 4 elements.

    # 冒泡排序
    li = [13, 22, 6, 99, 11, 1]

    print li

    for n in range(1, len(li)):
        for m in range(len(li)-n): 
            num1 = li[m]
            num2 = li[m+1]
            if num1 > num2:
                temp = num1
                li[m] = num2
                li[m+1] = temp

    print li