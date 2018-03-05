---
layout: post
title:  【De8ug玩python】-集合及其扩展
category: 
- python  
---

* content
{:toc}


## 帮助三剑客  

- dir  
- help  
- google

    >>> dir(set)
    ['__and__', '__class__', '__cmp__', '__contains__', '__delattr__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__iand__', '__init__', '__ior__', '__isub__', '__iter__', '__ixor__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__or__', '__rand__', '__reduce__', '__reduce_ex__', '__repr__', '__ror__', '__rsub__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__xor__', 'add', 'clear', 'copy', 'difference', 'difference_update', 'discard', 'intersection', 'intersection_update', 'isdisjoint', 'issubset', 'issuperset', 'pop', 'remove', 'symmetric_difference', 'symmetric_difference_update', 'union', 'update']

## 用法举例  

### set集合

set是一个无序且不重复的元素集合

练习：寻找差异
# 数据库中原有
   
    old_dict = {
        "#1":{ 'hostname':c1, 'cpu_count': 2, 'mem_capicity': 80 },
        "#2":{ 'hostname':c1, 'cpu_count': 2, 'mem_capicity': 80 }
        "#3":{ 'hostname':c1, 'cpu_count': 2, 'mem_capicity': 80 }
    }
     
    # cmdb 新汇报的数据
    new_dict = {
        "#1":{ 'hostname':c1, 'cpu_count': 2, 'mem_capicity': 800 },
        "#3":{ 'hostname':c1, 'cpu_count': 2, 'mem_capicity': 80 }
        "#4":{ 'hostname':c2, 'cpu_count': 2, 'mem_capicity': 80 }
    }

demo

        old_set = set(old_dict.keys())
    update_list = list(old_set.intersection(new_dict.keys()))

    new_list = []
    del_list = []

    for i in new_dict.keys():
        if i not in update_list:
            new_list.append(i)

    for i in old_dict.keys():
        if i not in update_list:
            del_list.append(i)

    print update_list,new_list,del_list


### 计数器（counter）

Counter是对字典类型的补充，用于追踪值的出现次数。

ps：具备字典的所有功能 + 自己的功能

    c = Counter('abcdeabcdabcaba')
    print c
    输出：Counter({'a': 5, 'b': 4, 'c': 3, 'd': 2, 'e': 1})

### 有序字典(orderedDict )

orderdDict是对字典类型的补充，他记住了字典元素添加的顺序

### 默认字典(defaultdict) 
defaultdict是对字典的类型的补充，他默认给字典的值设置了一个类型。

    from collections import defaultdict

    values = [11, 22, 33,44,55,66,77,88,99,90]

    my_dict = defaultdict(list)

    for value in  values:
        if value>66:
            my_dict['k1'].append(value)
        else:
            my_dict['k2'].append(value)

### 可命名元组(namedtuple) 

根据nametuple可以创建一个包含tuple所有功能以及其他功能的类型。

    import collections
     
    Mytuple = collections.namedtuple('Mytuple',['x', 'y', 'z'])


### 双向队列(deque)

一个线程安全的双向队列,可以两头进，两头出。

> 注：既然有双向队列，也有单项队列（先进先出 FIFO ）

    >>> import Queue
    >>> dir(Queue)
    ['Empty', 'Full', 'LifoQueue', 'PriorityQueue', 'Queue', '__all__', '__builtins__', '__doc__', '__file__', '__name__', '__package__', '_threading', '_time', 'deque', 'heapq']
    >>> 

## 实际工程应用  