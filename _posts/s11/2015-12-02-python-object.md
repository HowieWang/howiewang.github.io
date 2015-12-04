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

![](http://ww2.sinaimg.cn/bmiddle/96f6069fjw1eyo1s421xsj20i30ddwgt.jpg)

## 为什么要来个对象
为什么编程里也要来个对象呢？这当然还要从实际生活说起。  

很多编程语言都会拿动植物举例子。比如动物，动物这个词本身是个广义的概念，和你说个动物，你根本不知是啥！对吧？！天上飞啊，水里游啊，有毛没毛啊，好不好吃啊，你根本不知道这个动物是神马东西！所以，我们老祖宗可能刚会说话时候就开始给动物们起名字了，海马，大象，猴子，山羊，小白兔，等等。也就是给动物们分出了类别，注意！是`类别`！

说到`类别`，记得我们刚才说的是`比如动物`吧，为啥？因为自然界还有很多其他对象（东西）啊！植物，微生物，妖魔鬼怪，第4物种，非生物都算吧。所以自然界里，**动物本身就是一个类别，只不过是个不具体的类，抽象的类。** 她有几个主要的特点，会动，能吃，能喝，有眼有嘴有毛。老祖宗们给起了具体名字的那些动物，海马，大象，猴子，山羊，小白兔，等等又都有各自的长相，习性，也就是动物这个类别里面的一个具体类别了，但是这样还没完，单独说`小白兔`，你可能只是想到她的长相，动作，但是没看见摸到啊，还不算，当你真的抱了一只兔子在怀里，你就可以指着她说，这是一个具体的``兔子对象``。  

回到编程领域，老前辈们编程久了，发现`1，2，3，4，5，`都是个整数，可以加减乘除，于是就个起了个名字叫`int`，用来表示所有的整数对象的类，当你用了一个具体的数字时候，这就是个整数对象。再比如，前辈们发现``“上山大老虎”``是一串文字，可以写文章，描述东西，就起了个名字叫`字符串`，这就出现个不同于整数的类别，再说一句`“大王不在家”`的时候，这句话就是一个具体的字符串了，也就是个字符串类的对象了。于是就出现python的str了。同理，想把一系列东西放在列表里面去描述，就出现了`list`的类,这其实就是个抽象的列表对象，只有当我们定义出来一个`1，2，3，4，5，`的序列时，才出现一个具体的list对象。`dict`这个字典就是类似的意思了，你可以自动脑补一下。

![](http://ww2.sinaimg.cn/bmiddle/96f6069fjw1eyo1s4g1wxj20i30ddq5u.jpg)

## python里的面向对象
Python的官方版本，用C语言实现，使用也最为广泛。范爷在1989年就在c语言的基础上，添加了面向对象的概念，写出了python的核心代码，之后，python就成了个处处有对象的语言了，于是又用自己的各种带对象的细胞组成了各种各样有用的轮子。

那么，咱们是怎么知道python是面向对象的呢？当然是撸代码啦！  

> 小提示：下面这小段也许需要你有点c语言的基础。

![](http://ww4.sinaimg.cn/bmiddle/96f6069fjw1eyo1s52p1aj20i30ddadb.jpg)

### 撸撸 object.h  

源文件见github：https://github.com/wklken/Python-2.7.8/blob/master/Include/object.h  

看这个单词，我们就感觉了，object是啥？就是个对象！python里所谓的面向对象，就靠他了！

这个头文件很长，c语言定义了各种宏和函数指针，我看着头晕，咱们就先看看最上面的一大段注释，然后挑几个关键词看看好了。找找为啥python是面向对象的。

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

对象总是通过这种“PyObject“类型的指针访问。PyObject类型是只包含引用计数
和类型指针的结构。分配给对象的实际内存
包含仅能通过把指针传给一个更长结构类型指针后才可以访问的其他数据。这种较长的类型必须与引用计数和类型字段同时启动;这个PyObject_HEAD宏就是这样的用法（以适应未来的变化）。特定对象类型的实现可以把对象指针抛给适当的类型并返回。

下面我们只看两个定义：`PyObject_HEAD 和PyObject`

    /* PyObject_HEAD defines the initial segment of every PyObject. */
    #define PyObject_HEAD                   
        _PyObject_HEAD_EXTRA          # 双向链表      
        Py_ssize_t ob_refcnt;               # 引用计数
        struct _typeobject *ob_type;    # 指向类型对象的指针

    typedef struct _object {
         PyObject_HEAD  # PyObject 依赖这个PyObject_HEAD， PyObject_HEAD定义了每一个PyObject的初始化
     } PyObject;

PyObject_HEAD只是c语音的一个宏定义，类似于字符串替换，后面的才是实际内容，也就是可以替换第二段代码里结构体里的PyObject_HEAD，也就是说PyObject这个结构体，实际包含的是，一个双向链表，一个引用计数，一个指向类型的指针。


###  读读官方帮助class一章

对象具有特性，并且多个名称(在多个作用域中)可以绑定在同一个对象上。这在其它语言中被称为别名。在对 Python 的第一印象中这通常会被忽略，并且当处理不可变基础类型(数字，字符串，元组)时可以被放心的忽略。 但是，在调用列表、字典这类可变对象，或者大多数程序外部类型(文件，窗体等)描述实体时，别名对 Python 代码的语义便具有(有意而为!)影响。这通常有助于程序的优化，因为在某些方面别名表现的就像是指针。

类的定义非常巧妙地运用了命名空间，要完全理解接下来的知识，需要先理解作用域和命名空间的工作原理。

命名空间 是从命名到对象的映射。当前命名空间主要是通过 Python 字典实现的，不过通常不关心具体的实现方式(除非出于性能考虑)，以后也有可能会改变其实现方式。以下有一些命名空间的例子：内置命名(像 abs() 这样的函数，以及内置异常名)集，模块中的全局命名，函数调用中的局部命名。某种意义上讲对象的属性集也是一个命名空间。关于命名空间需要了解的一件很重要的事就是不同命名空间中的命名没有任何联系，例如两个不同的模块可能都会定义一个名为 maximize 的函数而不会发生混淆－－用户必须以模块名为前缀来引用它们。

当调用函数时，就会为它创建一个局部命名空间，并且在函数返回或抛出一个并没有在函数内部处理的异常时被删除(实际上，用遗忘来形容到底发生了什么更为贴切)。 当然，每个递归调用都有自己的局部命名空间。

作用域 就是一个 Python 程序可以直接访问命名空间的正文区域。这里的直接访问意思是一个对名称的错误引用会尝试在命名空间内查找。

类定义最简单的形式如下:

class ClassName:
    <statement-1>
    .
    <statement-N>

类的定义就像函数定义( def 语句)，要先执行才能生效(你当然可以把它放进 if 语句的某一分支，或者一个函数的内部。。

习惯上，类定义语句的内容通常是函数定义。进入类定义部分后，会创建出一个新的命名空间，作为局部作用域——因此，所有的赋值成为这个新命名空间的局部变量。特别是函数定义在此绑定了新的命名。

类定义完成时(正常退出)，就创建了一个 类对象。基本上它是对类定义创建的命名空间进行了一个包装；原始的局部作用域(类定义引入之前生效的那个)得到恢复，类对象在这里绑定到类定义头部的类名(例子中是 ClassName)



## 面向对象举例

上面那段官方文档有点绕啊，所以这里简单举个例子吧，把字符串添加颜色输出，需要pycharm。

    class ColorMe(object):
        """
        give me color see see

        """
        def __init__(self, some_str):
            self.color_str = some_str

        def blue(self):
            str_list = ["\033[34;1m", self.color_str, "\033[0m"]
            return ''.join(str_list) #"\033[34;1m" + self.color_str + "\033[0m"

    c = ColorMe('im blue').blue()
    print c

上面的ColorMe就是一个类了，继承自最原始的基类Object，里面有个数据叫color_str，用来接收外面传进来的字符串，还有个方法，也就是blue的函数，用来把字符串格式化成蓝色。

c是ColorMe这个类的一个实例，也就包装出来一个命名空间，blue就是里面的一个函数，'im blue'作为一个参数传到这个空间，然后通过blue函数的修改，在pycharm控制台就能输出蓝色。

## 补刀，神马是堆栈，指针，命名空间？！

这都是些外国词儿。  其实都是折腾计算机的内存。内存就是一个IC卡，通过电流的信号控制传输0和1，逻辑上保存可以想象成楼房的的形式。每一层保存1个字节的数据，楼层号就是地址。不同的变量占据不同的楼层，有自己的数据，指针就是一种特殊的变量，保存的是楼层号，也就是内存地址。

![](http://ww3.sinaimg.cn/bmiddle/96f6069fjw1eyo03u9vavj218g0oyjz1.jpg)
![](http://ww3.sinaimg.cn/bmiddle/96f6069fjw1eyo03uoz4zj218g0oyn47.jpg)

- heap：是由malloc之类函数分配的空间所在地。地址是由低向高增长的。 一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收 。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表

- stack：是自动分配变量，以及函数调用的时候所使用的一些空间。地址是由高向低减少的。由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

程序运行的时候，需要内存空间存放数据。一般来说，系统会划分出两种不同的内存空间：一种叫做stack（栈），另一种叫做heap（堆）。
它们的主要区别是：stack是有结构的，每个区块按照一定次序存放，可以明确知道每个区块的大小；heap是没有结构的，数据可以任意存放。因此，stack的寻址速度要快于heap。一般来说，每个线程分配一个stack，每个进程分配一个heap，也就是说，stack是线程独占的，heap是线程共用的。此外，stack创建的时候，大小是确定的，数据超过这个大小，就发生stack overflow错误，而heap的大小是不确定的，需要的话可以不断增加.

在编程语言中，命名空间是对作用域的一种特殊的抽象，它包含了处于该作用域内的标识符，且本身也用一个标识符来表示，这样便将一系列在逻辑上相关的标识符用一个标识符组织了起来。许多现代编程语言都支持命名空间。在一些编程语言（例如C++和Python）中，命名空间本身的标识符也属于一个外层的命名空间，也即命名空间可以嵌套，构成一个命名空间树，树根则是无名的全局命名空间。

函数和类的作用域可被视作隐式命名空间，它们和可见性、可访问性和对象生命周期不可分区的联系在一起。


## 参考  
- [PYTHON 源码阅读 – 对象](http://python.jobbole.com/83443/)    
- [pythontutorial27-cn](http://www.pythondoc.com/pythontutorial27/classes.html#tut-object)    
- [堆(heap)和栈](http://blog.csdn.net/mybelief321/article/details/8912736)    
- [Stack的三种含义](http://www.ruanyifeng.com/blog/2013/11/stack.html)
- [wiki-命名空间](https://zh.wikipedia.org/wiki/%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)    
- [程序是怎样跑起来的](http://www.ituring.com.cn/book/1136)    
