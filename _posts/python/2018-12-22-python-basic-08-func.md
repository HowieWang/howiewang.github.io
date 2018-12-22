---
layout: post
title:  Python随身听基础篇08-函数的概念与应用
categories:  
- python
- python随身听
excerpt:  Python随身听基础篇08-函数的概念与应用
stickie: true
---

* content
{:toc}

## 介绍

本文是《Python随身听》基础篇08的文字稿。

>《Python随身听》是由第8哥（De8ug）录制的一档Python节目。致力于带给你全新的Python学习和复习体验，打造一门听着就能学会Python的空中课堂。

点击链接，立即收听。

- [喜马拉雅](https://www.ximalaya.com/jiaoyu/11009485/)
- [网易云音乐](https://music.163.com/#/djradio?id=350523062)



## 函数概念

### 函数式编程和面向对象的区别

函数式：将某功能代码封装到函数中，日后便无需重复编写，仅调用函数即可
面向对象：对函数进行分类和封装，让开发“更快更好更强...”

函数式编程最重要的是增强代码的重用性和可读性。

如果把写代码比作写文章的话，每一句代码就是一句话，一个函数就是能灵活应用的一段话。

### 函数的定义和使用

    def 函数名(参数1,参数2...):
         
        ...
        函数体
        return 返回值
        ...

函数的定义主要有如下要点：

- def：表示函数的关键字  
- 函数名：函数的名称，日后根据函数名调用函数    
- 参数：为函数体提供数据  
- 返回值：当函数执行完毕后，可以给调用者返回数据。

以上要点中，比较重要有参数和返回值。

### 参数  

函数里面的参数代表的函数的输入，常用有三种不同的参数：

- 普通参数
- 默认参数
- 动态参数

```
# 普通参数

def add(a, b):
    return a+b

add(55, 66)

# 默认参数

def add(a=1, b=2):
    return a+b

add(55, 66)
add()

# 动态参数

In [1]: def add(*a, **b):
   ...:     print(a, b)
   ...:

In [2]: add(1,2,3,4, {"a":2, "b":3, "c": 6})
(1, 2, 3, 4, {'a': 2, 'b': 3, 'c': 6}) {}


```

### 返回值

函数是一个功能块，该功能到底执行成功与否，需要通过返回值来告知调用者。


## 应用  

编程中最常处理的数据是文字和数字，下面用1个接近生活的例子来表示。

### 故事

程序猿小明的老婆小红让小明去买东西，给了他100块钱，告诉他买2斤鸡蛋，1斤西红柿，如果遇见卖牛肉的买2斤。我们假设鸡蛋2块1斤，西红柿1块1斤，牛肉5块一斤。剩下的给小明当零花钱。

下面小明就可以写程序来算算买完东西还剩下多少零花钱了。

### 代码1-流水账

```
all_money = 100

egg_price = 2
tomato_price = 1
beef_price = 5

egg_count = 2
tomato_count = 1

see_beef = True
if see_beef:
    tomato_count = 2

buy_egg = egg_price * egg_count
buy_tomato = tomato_price * tomato_count

left = all_money - buy_egg - buy_tomato

print(left)

```

### 代码2-构造函数

- 整合
- 提参
- 定返

```
def buy():
    all_money = 100

    egg_price = 2
    tomato_price = 1
    beef_price = 5

    egg_count = 2
    tomato_count = 1

    see_beef = True
    if see_beef:
        tomato_count = 2

    buy_egg = egg_price * egg_count
    buy_tomato = tomato_price * tomato_count

    left = all_money - buy_egg - buy_tomato

    print(left)
```

```
def buy(egg_count, tomato_count, all_money=100, egg_price=2, tomato_price=1, beef_price=5, see_beef=True):
    if see_beef:
        tomato_count = 2

    buy_egg = egg_price * egg_count
    buy_tomato = tomato_price * tomato_count

    left = all_money - buy_egg - buy_tomato

    print(left)
    return left
```

### 代码3-优化函数

- 优化参数
- 提炼函数

```
price_dic = {
    egg_price:2, 
    tomato_price:1, 
    beef_price:
}


def buy(egg_count, tomato_count, all_money=100, see_beef=True, price_dic):
    if see_beef:
        tomato_count = 2

    buy_egg = price_dic[egg_price] * egg_count
    buy_tomato = price_dic[tomato_price] * tomato_count

    left = all_money - buy_egg - buy_tomato

    print(left)
    return left
```

```
price_dic = {
    egg_price:2, 
    tomato_price:1, 
    beef_price:
}


def count_price(price_dic, egg_count, tomato_count):
    buy_egg = price_dic[egg_price] * egg_count
    buy_tomato = price_dic[tomato_price] * tomato_count
    return buy_egg, buy_tomato

def buy(egg_count, tomato_count, all_money=100, see_beef=True, price_dic):
    if see_beef:
        tomato_count = 2

    buy_egg, buy_tomato = count_price(price_dic, egg_count, tomato_count)

    left = all_money - buy_egg - buy_tomato

    print(left)
    return left
```


## 总结

今天，Python随身听给你介绍的是函数的概念和使用，并且讲解了从流水账的代码变成函数并优化的过程。上面的示例是伪代码，实际运行会有很多小bug，你是不是已经看出来了呢？

今天的问题是：现在是指定了买什么物品，如果让我们的函数通用性更强，支持购买任何物品的计算，该怎么继续优化呢？

欢迎留言评论。也欢迎关注我的公众号：`第8哥小灶时间`，有任何问题可以直接提问，并第一时间获取节目更新信息。