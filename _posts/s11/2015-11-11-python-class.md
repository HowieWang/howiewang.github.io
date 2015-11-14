---
layout: post
title:  【Howie玩python】-类与对象
category: 
- python  
---

* content
{:toc}


## 帮助三剑客  

- dir  
- help  
- google


## 用法举例  
面向对象最重要的概念就是类（Class）和实例（Instance），必须牢记类是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”，每个对象都拥有相同的方法，但各自的数据可能不同。

仍以Student类为例，在Python中，定义类是通过class关键字：

```
class Student(object):
    pass
    ```

class后面紧接着是类名，即Student，类名通常是大写开头的单词，紧接着是(object)，表示该类是从哪个类继承下来的，继承的概念我们后面再讲，通常，如果没有合适的继承类，就使用object类，这是所有类最终都会继承的类。

定义好了Student类，就可以根据Student类创建出Student的实例，创建实例是通过类名+()实现的：
```
>>> bart = Student()
>>> bart
<__main__.Student object at 0x10a67a590>
>>> Student
<class '__main__.Student'>
```

类是创建实例的模板，而实例则是一个一个具体的对象，各个实例拥有的数据都互相独立，互不影响；

方法就是与实例绑定的函数，和普通函数不同，方法可以直接访问实例的数据；

通过在实例上调用方法，我们就直接操作了对象内部的数据，但无需知道方法内部的实现细节。

和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同：

```
>>> bart = Student('Bart Simpson', 59)
>>> lisa = Student('Lisa Simpson', 87)
>>> bart.age = 8
>>> bart.age
8
>>> lisa.age
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'age'
```

## 实际工程应用  

写个自己的练习小例子吧。   
大家知道在pycharm里面，我们是可以给控制台输出带上颜色的。于是，我给这个颜色做了个类。代码如下：   

```
#!/usr/bin/env Python
#-*- coding:utf-8 -*-

__author__ = 'SothisHowie'

class ColorMe(object):
    """
    give me color see see

    """
    def __init__(self, some_str):
        self.color_str = some_str

    def blue(self):
        str_list = ["\033[34;1m", self.color_str, "\033[0m"]
        return ''.join(str_list) #"\033[34;1m" + self.color_str + "\033[0m"

    def green(self):
        str_list = ["\033[32;1m", self.color_str, "\033[0m"]
        return ''.join(str_list) #"\033[34;1m" + self.color_str + "\033[0m"

    def yellow(self):
        str_list = ["\033[33;1m", self.color_str, "\033[0m"]
        return ''.join(str_list) #"\033[34;1m" + self.color_str + "\033[0m"

    def red(self):
        str_list = ["\033[31;1m", self.color_str, "\033[0m"]
        return ''.join(str_list) #"\033[34;1m" + self.color_str + "\033[0m"

```

实际用起来很简单，就是

        ColorMe('somestr').blue()

具体使用看我这个修改ha配置代码的例子吧！欢迎指正！

[examlpe on github ](https://github.com/HowieWang/pythonStudy/tree/master/s11/day3)