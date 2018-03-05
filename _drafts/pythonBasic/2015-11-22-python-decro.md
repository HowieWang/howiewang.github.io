---
layout: post
title:  【De8ug玩python】-装饰器很好玩
category: 
- python  
---

* content
{:toc}

## 装饰模式

在面向对象（OOP）的设计模式中，decorator被称为装饰模式。OOP的装饰模式需要通过继承和组合来实现，而Python除了能支持OOP的decorator外，直接从语法层次支持decorator。Python的decorator可以用函数实现，也可以用类实现。

decorator可以增强函数的功能，定义起来虽然有点复杂，但使用起来非常灵活和方便。

## 基本用法  

初创公司有N个业务部门，1个基础平台部门，基础平台负责提供底层的功能，如：数据库操作、redis调用、监控API等功能。业务部门使用基础功能时，只需调用基础平台提供的功能即可。如下：

        def f1():
            print 'f1'

调用给你：

        f1()

目前公司有条不紊的进行着，但是，以前基础平台的开发人员在写代码时候没有关注验证相关的问题，即：基础平台的提供的功能可以被任何人使用。现在需要对基础平台的所有功能进行重构，为平台提供的所有功能添加验证机制，即：执行功能前，先进行验证。

写代码要遵循开发封闭原则，虽然在这个原则是用的面向对象开发，但是也适用于函数式编程，简单来说，它规定已经实现的功能代码不允许被修改，但可以被扩展，即：

- 封闭：已实现的功能代码块   
- 开放：对扩展开发

如果将开放封闭原则应用在上述需求中，那么就不允许在函数 f1 、f2、f3、f4的内部进行修改代码，

        def w1(func):
            def inner():
                # 验证1
                # 验证2
                # 验证3
                return func()
            return inner
         
        @w1
        def f1():
            print 'f1'

当写完这段代码后（函数未被执行、未被执行、未被执行），python解释器就会从上到下解释代码，步骤如下：

        def w1(func):  ==>将w1函数加载到内存
        @w1

没错，从表面上看解释器仅仅会解释这两句代码，因为函数在没有被调用之前其内部代码不会被执行。

从表面上看解释器着实会执行这两句，但是 @w1 这一句代码里却有大文章，@函数名 是python的一种语法糖。

如上例@w1内部会执行一下操作：

        执行w1函数，并将 @w1 下面的 函数 作为w1函数的参数，即：@w1 等价于 w1(f1)
        所以，内部就会去执行：
            def inner:
                #验证
                return f1()   # func是参数，此时 func 等于 f1
            return inner     # 返回的 inner，inner代表的是函数，非执行函数
        其实就是将原来的 f1 函数塞进另外一个函数中
        将执行完的 w1 函数返回值赋值给@w1下面的函数的函数名
        w1函数的返回值是：
           def inner:
                #验证
                return 原来f1()  # 此处的 f1 表示原来的f1函数
        然后，将此返回值再重新赋值给 f1，即：
        新f1 = def inner:
                    #验证
                    return 原来f1() 

所以，以后业务部门想要执行 f1 函数时，就会执行 新f1 函数，在 新f1 函数内部先执行验证，再执行原来的f1函数，然后将 原来f1 函数的返回值 返回给了业务调用者。
如此一来， 即执行了验证的功能，又执行了原来f1函数的内容，并将原f1函数返回值 返回给业务调用着

## 带参数的怎么办？

        def w1(func):
            def inner(*args,**kwargs):
                # 验证1
                # 验证2
                # 验证3
                return func(*args,**kwargs)
            return inner
         
        @w1
        def f1(arg1,arg2,arg3):
            print 'f1'

## 高阶玩法  

参考：[liaoxuefeng-python](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014318435599930270c0381a3b44db991cd6d858064ac0000)

函数也是对象，它有__name__等属性，但你去看经过decorator装饰之后的函数，它们的__name__已经从原来的'now'变成了'wrapper'：

        >>> now.__name__
        'wrapper'

因为返回的那个wrapper()函数名字就是'wrapper'，所以，需要把原始函数的__name__等属性复制到wrapper()函数中，否则，有些依赖函数签名的代码执行就会出错。

不需要编写wrapper.__name__ = func.__name__这样的代码，Python内置的functools.wraps就是干这个事的，所以，一个完整的decorator的写法如下：

        import functools

        def log(func):
            @functools.wraps(func)
            def wrapper(*args, **kw):
                print('call %s():' % func.__name__)
                return func(*args, **kw)
            return wrapper

或者针对带参数的decorator：

        import functools

        def log(text):
            def decorator(func):
                @functools.wraps(func)
                def wrapper(*args, **kw):
                    print('%s %s():' % (text, func.__name__))
                    return func(*args, **kw)
                return wrapper
            return decorator

import functools是导入functools模块。