---
layout: post
title:  【Howie玩python】-我所理解的面向对象
category:
-   python

---

* content
{:toc}

> 一切皆对象

## 啥是面向对象
上面那句话，大家估计早就有所耳闻了。但究竟啥是面向对象呢？我来简单说说自己的看法，也算是对几年里面向对象编程的一个总结。

面向对象起源于生活，我们每天看见的，听见的，用的，吃的，玩的，不分动物，植物，微生物其实都是个对象。英文object，错略翻译一下就是个东西嘛！
小到组成原子的小东西，大到宇宙中时间空间无限的大东西，世间万物都是个东西，也就是各种对象。

对象有实体的，也有抽象的。实体的就是我们看得见摸得着的，公交，地铁，坦克，大炮，米饭，馒头，电脑，键盘都是对象。抽象的博客，文章，时间，历史其实也是个对象，我们脑海中的对象。

编程语言发展好几十年了，老前辈们不断总结，发挥聪明才智，把编程技术也抽象出了对象这个概念。把不同的代码结构分门归类，组成了各种所谓的对象，每个对象可以表示一个思路，一种方法，又有她自己的特点，也就出现了面向对象编程。

总结一下，编程里的面向对象，其实就是一种思维抽象的产物。


## 为什么要来个对象
为什么编程里也要来个对象呢？这当然还要从实际生活说起。  

很多编程语言都会拿动植物举例子。比如动物，动物这个词本身是个广义的概念，和你说个动物，你根本不知是啥！对吧？！天上飞啊，水里游啊，有毛没毛啊，好不好吃啊，你根本不知道这个动物是神马东西！所以，我们老祖宗可能刚会说话时候就开始给动物们起名字了，海马，大象，猴子，山羊，小白兔，等等。也就是给动物们分出了类别，注意！是`类别`！

说到`类别`，记得我们刚才说的是`比如动物`吧，为啥？因为自然界还有很多其他对象（东西）啊！植物，微生物，妖魔鬼怪，第4物种，非生物都算吧。所以自然界里，**动物本身就是一个类别，只不过是个不具体的类，抽象的类。** 她有几个主要的特点，会动，能吃，能喝，有眼有嘴有毛。老祖宗们给起了具体名字的那些动物，海马，大象，猴子，山羊，小白兔，等等又都有各自的长相，习性，也就是动物这个类别里面的一个具体类别了，但是这样还没完，单独说`小白兔`，你可能只是想到她的长相，动作，但是没看见摸到啊，还不算，当你真的抱了一只兔子在怀里，你就可以指着她说，这是一个具体的``兔子对象``。  

回到编程领域，老前辈们编程久了，发现`1，2，3，4，5，`都是个整数，可以加减乘除，于是就个起了个名字叫`int`，用来表示所有的整数对象的类，当你用了一个具体的数字时候，这就是个整数对象。再比如，前辈们发现``“上山大老虎”``是一串文字，可以写文章，描述东西，就起了个名字叫`字符串`，这就出现个不同于整数的类别，再说一句`“大王不在家”`的时候，这句话就是一个具体的字符串了，也就是个字符串类的对象了。于是就出现python的str了。同理，想把一系列东西放在列表里面去描述，就出现了`list`的类,这其实就是个抽象的列表对象，只有当我们定义出来一个`1，2，3，4，5，`的序列时，才出现一个具体的list对象。`dict`这个字典就是类似的意思了，你可以自动脑补一下。


## python里的面向对象
Python的官方版本，用C语言实现，使用也最为广泛。范爷在1989年就在c语言的基础上，添加了面向对象的概念，写出了python的核心代码，之后，python就成了个处处有对象的语言了，于是又用自己的各种带对象的细胞组成了各种各样有用的轮子。

那么，咱们是怎么知道python是面向对象的呢？当然是撸代码啦！  

> 小提示：下面这小段也许需要你有点c语言的基础。

### 撸撸 object.h  

源文件见github：https://github.com/wklken/Python-2.7.8/blob/master/Include/object.h  

看这个单词，我们就感觉了，object是啥？就是个对象啊！python里所谓的面向对象，就靠他了！

这个头文件很长，c语言定义了各种宏和函数指针，我看着头晕啊，咱们就先看看最上面的一大段注释，然后挑几个关键词看看好了。找找为啥python是面向对象的。

> Objects are structures allocated on the heap.  Special rules apply to
the use of objects to ensure they are properly garbage-collected.
Objects are never allocated statically or on the stack; they must be
accessed through special macros and functions only.   

对象是在堆上分配的结构。使用对象时，应用特别的规则，以确保它们的垃圾回收。
对象从未分配静态或在栈上;它们必须是仅通过特殊宏和函数访问。

> An object has a 'reference count' that is increased or decreased when a
pointer to the object is copied or deleted; when the reference count
reaches zero there are no references to the object left and it can be
removed from the heap.

对象具有一个“引用计数”，当指针对象被复制或删除，会自动增加或减少;当“引用计数”
达到零时，对象的引用就没有了，这时可以从堆移除。

> An object has a 'type' that determines what it represents and what kind
of data it contains.  An object's type is fixed when it is created.
Types themselves are represented as objects; an object contains a
pointer to the corresponding type object.  The type itself has a type
pointer pointing to the object representing the type 'type', which
contains a pointer to itself!.  

一个对象有一个“类型”，决定这个对象代表什么，包含什么样的数据。在创建时对象的类型是固定的。类型本身表示为对象;一个对象包含一个相应的类型的对象的指针。类型本身有一个类型
指针指向表示类型“type”的对象，这包含一个指向本身的指针！。

> Objects do not float around in memory; once allocated an object keeps
the same size and address.  Objects that must hold variable-size data
can contain pointers to variable-size parts of the object.  Not all
objects of the same type have the same size; but the size cannot change
after allocation.  (These restrictions are made so a reference to an
object can be simply a pointer -- moving an object would require
updating all the pointers, and changing an object's size would require
moving it if there was another object right next to it.)

对象不会在内存中到处跑;对象一旦分配就会保持相同的大小和地址。对象必须拥有可变大小数据
可以包含指向该对象的可变长部分的指针。并非所有的同一类型的对象具有相同的大小;但分配后的大小是不能改变的。 （由于这些限制，所以一个对象的引用可以简单的是一个指针 - 移动一个对象将需要
更新所有的指针，改变对象的大小的时候，如果这个对象旁边有另外一个对象，就需要移动它。）

> Objects are always accessed through pointers of the type 'PyObject *'.
The type 'PyObject' is a structure that only contains the reference count
and the type pointer.  The actual memory allocated for an object
contains other data that can only be accessed after casting the pointer
to a pointer to a longer structure type.  This longer type must start
with the reference count and type fields; the macro PyObject_HEAD should be
used for this (to accommodate for future changes).  The implementation
of a particular object type can cast the object pointer to the proper
type and back.
A standard interface exists for objects that contain an array of items
whose size is determined when the object is allocated.

对象总是通过这种“PyObject“的指针访问。PyObject类型是只包含引用计数
和类型指针的结构。分配给对象的实际内存
包含仅能铸造指针后可以访问其他数据
的指针更长结构类型。这种较长的类型必须启动
与引用计数和类型字段;宏PyObject_HEAD应
用于这种（以适应未来的变化）。实施
特定对象的类型可以在对象指针转换为适当的
类型和背部。

存在包含项目的数组对象的标准接口
其尺寸，当对象被分配被确定。

## 面向对象举例
这里简单举个把字符串格式化时候添加颜色的类。

## 参考  
- [PYTHON 源码阅读 – 对象](http://python.jobbole.com/83443/)    
- []()    
- []()    
- []()    
- []()    
